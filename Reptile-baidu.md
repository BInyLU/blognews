> Reptile-baidu，这是我第一次写的node爬虫程序。
>
> 它基于用户输入的关键字自动下载百度图库里的图片，省去在网站上下载的麻烦事~



废话不多说咱们直接开始。

我们一般从网站图库里面找图看，先输入关键字，再进行搜索。

好，那么从这里看出，我们不难找出它的服务器请求，打开控制台，选择Network，再点击XHR，刷新，从这里可以看到网站向服务器请求资源，那么，我们就得到它的地址，通过这个地址，我们就可以动手写代码了。

![1](http://binylu.cn/new/rb/1.png)

创建`index.js`我们先引入request、IO和path，用来后面读写文件操作。

然后我们通过给定一些变量再去写一个请求和创建文件夹的处理。

![0](http://binylu.cn/new/rb/0.png)

![2](http://binylu.cn/new/rb/2.png)

![3](http://binylu.cn/new/rb/3.png)

这里的`word`值就是提供关键字的入口，我们再创建一个`test.js`，引入`index.js`

![4](http://binylu.cn/new/rb/4.png)

我们安装依赖之后，在命令行工具测试运行一下

![5](http://binylu.cn/new/rb/5.png)

成功运行，我们在根目录可以看到成功生成通过关键字得到的文件夹。

![6](http://binylu.cn/new/rb/6.png)

![7](http://binylu.cn/new/rb/7.png)





注意：关键字重复搜索下载可能会出问题。

（出问题我也不打算去更新，嘻嘻闹着玩就好）



Github地址：https://github.com/BInyLU/Reptile-baidu

**请确保node环境已安装再进行下面的步骤**

1、安装依赖

```
npm install
```

2、在  `test.js` 中输入图片关键字

```
let downImg = require("./index");

downImg({
    word : "在这里输入图片关键字"
});
```

3、运行 `test.js` 

```
node test.js
```

4、然后node程序就会在当前文件夹创建存放图片的文件夹，打开就能看见下载好的图片。