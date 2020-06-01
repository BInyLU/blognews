## Node中的流传输

> Node.js的流(Stream) API 非常强大，它是处理流数据的抽象接口。流可以看成是一种数据的集合，但它并不是一下子全部读到内存里面，而是一块一块地去产生、消耗，这种方式最显而易见的好处是可以方便地处理大文件。数据流可以是可读流、可写流，实际上Node.js中的流分为4种类型 : Readable、Writable、Duplex、Transform。
>

- Readable Stream：可读流是对可消费的数据源进行的抽象，比如fs.createReadStream
- Writable Stream：可写流是对流的目的地（destination）的抽象，destination运允许数据写入，比如fs.createWriteStream
- Duplex Stream：双工流是同时实现了 Readable 和 Writable 接口的流，既能写又能读。比如TCP socket
- Transform Stream：交换流本质上是一种Duplex流，可以将其看成输入Writable流，输出的是Readable流，也可以称之为“通过流”（through streams）。比如zlib streams。

### pipe方法

对于Readable流而言，有两种消费数据流的方式：Paused Mode 和 Flowing Mode。简单来说，Paused Mode就像是把水缸里面的水一瓢瓢舀出来，可以根据需要使用read()方法去消费数据流；Flowing Mode就像是给水缸接了根管子，水可以从高处流到低处，我们可以监听data事件得到一块数据流。

所有的Readable流默认是Paused Mode，使用resume()、pause()方法可以与 Flowing Mode 相互切换，resume()方法就像是给水缸接好管子，水自动流动；pause()方法就像移除管子，我们得手动去舀水。这种切换方式很简单，是有时候是自动发生的。 

当Readable流使用pipe()方法时，就相当于给数据流接上了管子，数据流会自动从上游流到下游。在使用pipe()时，需要注意的是上游是Readable，下游是Writable，即：

```js
readableSrc.pipe(writableDest)
```

由于Duplex流 实现了Readable、和Writable，可以将Duplex或Transform 放在“中游”：

由于Duplex流 实现了Readable、和Writable，可以将Duplex或Transform 放在“中游”：

```javascript
readableSrc
  .pipe(transformStream1)
  .pipe(transformStream2)
  .pipe(finalWrtitableDest)
```

我们知道，Node中的Server端HTTP response是Writable流，而通过fs.createReadStream读取视频数据得到的是Readable流。因此，Node的Server端可以直接使用pipe()方法将视频流发到前端：

### 第一种写法

前端：

```html
<video src="/video"></video>
```

服务端：

```javascript
router.get('/video', function(req, res, next) {
let head = { 'Content-Type': 'video/mp4' };
//需要设置HTTP HEAD
res.writeHead(200, head);
//使用pipe
fs.createReadStream('./assets/sintel.mp4')
    .pipe(res);
});
```

前端结果： 

可以注意到前端下载了一个html和video。

![这里写图片描述](https://img-blog.csdn.net/20170802133819429?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1eWFxaTE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


### 第二种写法

**HTTP 206**

- HTTP/1.1 206状态码表示的是：客户端通过发送范围请求头Range获取资源的部分数据。这种请求可以将服务端文件分割成多个部分传给客户端，可用于解决网络问题以及大文件下载问题。对于一个很大的视频，就可以采用这种请求将视频流分成多个部分下载。 

**需要关注的HTTP Headers有：**

- Range：用于请求头中，指定第一个字节的位置和最后一个字节的位置，一般格式：Range:(unit=first byte pos)-[last byte pos] 。如 Range:bytes=0- 表示请求服务端第0及以后bytes的数据； Range:bytes=0-999 表示0到999 bytes的数据，注意这个区间的长度是1000bytes。
- Accept-Ranges：用于响应头，表明服务器支持Range请求,以及服务器所支持的单位是字节(这也是唯一可用的单位)；Accept-Ranges: none 响应头表示服务器不支持范围请求。
- Content-Range：用于响应头，指定整个实体中的一部分的插入位置，一般格式： Content-Range: bytes (unit first byte pos) - [last byte pos]/[entity legth]。如Content-Range:bytes 1000000-1999999/3332038 表示的是服务端资源总大小3332038 bytes，此次返回的是其中第1000000到1999999 bytes 的数据。
- Content-Length：用于响应头，表明了响应实体的大小，它应该等于Content-Range中的 last byte pos - first byte pos + 1。

将视频流分成多个部分发给前端，只要注意控制好流的数据区间即可，服务端代码如下：

```javascript
router.get('/video', function(req, res, next) {
    let path = './assets/sintel.mp4';
    let stat = fs.statSync(path);
    let fileSize = stat.size;
    let range = req.headers.range;

    // fileSize 3332038

    if (range) {
        //有range头才使用206状态码

        let parts = range.replace(/bytes=/, "").split("-");
        let start = parseInt(parts[0], 10);
        let end = parts[1] ? parseInt(parts[1], 10) : start + 999999;

        // end 在最后取值为 fileSize - 1 
        end = end > fileSize - 1 ? fileSize - 1 : end;

        let chunksize = (end - start) + 1;
        let file = fs.createReadStream(path, { start, end });
        let head = {
            'Content-Range': `bytes ${start}-${end}/${fileSize}`,
            'Accept-Ranges': 'bytes',
            'Content-Length': chunksize,
            'Content-Type': 'video/mp4',
        };
        res.writeHead(206, head);
        file.pipe(res);
    } else {
        let head = {
            'Content-Length': fileSize,
            'Content-Type': 'video/mp4',
        };
        res.writeHead(200, head);
        fs.createReadStream(path).pipe(res);
    }

});

```

前端结果：

![这里写图片描述](https://img-blog.csdn.net/20170802170329511?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1eWFxaTE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这次的视频就被分成了4个部分，每个video请求及结果如下：

| 顺序 | Request              | Response                                                     |
| ---- | -------------------- | ------------------------------------------------------------ |
| 1    | Range:bytes=0-       | Content-Range:bytes 0-999999/3332038 Content-Length:1000000  |
| 2    | Range:bytes=1000000- | Content-Range:bytes 1000000-1999999/3332038 Content-Length:1000000 |
| 3    | Range:bytes=2000000- | Content-Range:bytes 2000000-2999999/3332038 Content-Length:1000000 |
| 4    | Range:bytes=3000000- | Content-Range:bytes 3000000-3332037/3332038 Content-Length:332038 |

