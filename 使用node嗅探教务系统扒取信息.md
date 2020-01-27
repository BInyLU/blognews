### 使用Node嗅探南宁师范大学教务系统扒取信息

> 这个爬虫练手小项目早在我的数据库课程时就已经打了个算盘，写着一小半没啥思路就一直留坑，直到昨天，才完整写完。
>
> 为什么选择南宁师范大学教务系统的原因，是因为舍友的数据库项目正好是掌上校园系统，他们的项目数据库只是测试数据，非真实数据，所以就想了一下，扒数据下来用。
>
> 最后我把代码封装了一下，做成了一个在线的`API`接口放在自己的服务器下，有需要的小伙伴可以通过接口取数据调用噢。



**API参数：**

- 请求地址：http://121.43.124.47:3001/nsd

- 需要传递登陆账号和密码值：`username`、`password`

- 目前可以调用的数据只有个人信息查询、考试安排查询、学期课表查询；后面会逐一更新增加接口数据，如专业执行计划查询，空教室查询等。

- **API例子**：http://121.43.124.47:3001/nsd?username=1915100317&password=lu785898


**步骤：**

- 首先，拿数据之前，当然先要嗅探。这里的嗅探技巧无非就是打开控制台，选择`Network`选择栏，这串英文意思貌似是网络传输；当我们刷新页面，让它处于请求状态时，监听网站的动向，就能找出传递值在什么接口位置。
- 以登陆为例，当它每次发起请求时，都会向 http://210.36.80.160/jsxsd/xk/LoginToXk 这个网址以`POST`传递登陆账号和密码，同时我还发现，每次请求它都会有`Cookies`随机固定长度值同时传过去，传递过程中，还需要指定一个`Content-Type`值和`Cookies`值传递，`request header`才会成功跳转至用户界面，这里我们取到的`Content-Type`值是`application/x-www-form-urlencoded`，先记住。
- 所以我们发现这个请求条件规律后，造一个思路，我们需要造一个`Cookies`随机固定长度值，同时这`Cookies`值在浏览器里，每一个页面都需要用到`Cookies`，所以要保存下来，当我们每次请求不一样的页面时，都是传递浏览器中的`Cookies`后与`Content-Type`才算成功模拟一个登陆。



**代码：**

这里我们用到了常用的几个模块去写。

```javascript
const http = require('http');
const queryString = require('querystring');
const request = require('request');
const cheerio = require('cheerio');
const express = require('express');
const fs = require('fs');
```

定义好扒取网址需要的常量和变量：

```javascript
//网址常量
const hostName = '210.36.80.160';
const port = 80;

//每个页面都需携带有cookie值
let cookieAll;

// 最终输出结果
let jsonData = [];
```

然后就是上面我所说的请求条件：

```javascript
//发送的请求的条件
let option = {
    hostname: hostName,
    method: "POST",
    port: port,
    path: '/jsxsd/xk/LoginToXk',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    }
};
```

然后我们试着携带`cookie`值和`Content-Type`值一起进行发送的请求，最后测试完美成功。

以下代码有点多，就一并放出来了。

全部代码：

```javascript
const http = require('http');
const queryString = require('querystring');
const request = require('request');
const cheerio = require('cheerio');
const express = require('express');
const fs = require('fs');
const app = express();
const server = new http.Server();

const hostName = '210.36.80.160';
const port = 80;

// 跨域支持
app.all('*', function(req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    res.header('Access-Control-Allow-Methods', '*');
    res.header('Content-Type', 'application/json;charset=utf-8');
    next();
});

// 打开监听端口
var servers = app.listen(3001, function() {
    var host = servers.address().address;
    var port = servers.address().port;
    console.log('请打开 http://localhost:' + port + '/');
});

//每个页面都需携带有cookie值
let cookieAll;

// 最终输出结果
let jsonData = [];

//发送的请求的条件
let option = {
    hostname: hostName,
    method: "POST",
    port: port,
    path: '/jsxsd/xk/LoginToXk',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    }
};

// 接收参数 username password
app.get('/nsd', function(req, res) {
    var result = req.query;
    console.log(result.username);
    console.log(result.password);

    postData = queryString.stringify({
        USERNAME: result.username,
        PASSWORD: result.password
    });

    //携带cookie值进行发送的请求
    const contentGet = http.request(option, function(res){
        res.setEncoding('utf-8');
        if (res.headers.location) {
            let cookie = res.headers['set-cookie'];
            cookieAll = cookie[0];
            classs();
        } else {
            console.log("登陆信息错误");
            status = 0,
            code = 201;
            msg = '登陆信息错误'
            jsonData = [];
            return;
        }
        res.on("error", function(error){
            console.log("错误的信息", error.message)
        })
    });
    contentGet.write(postData);
    contentGet.end();

    // 发送数据 延迟2秒 因为扒取需要时间
    setTimeout(function(){
        // 返回状态码
        if(jsonData == ''){
            status = 0,
            code = 201;
            msg = '登陆信息错误'
        }else{
            status = 1,
            code = 200;
            msg = '登陆信息正确'
        }
        res.send({
            status:status,
            code: code,
            msg: msg,
            data: jsonData
        })
    },3000)

    //扒取课表逻辑
    function classs(){
        request({
            method: 'POST',
            url: 'http://' + hostName + ':' + port + '/jsxsd/xskb/xskb_list.do',
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded',
                'Cookie': cookieAll,
            },
        }, function(err, red, body){
            console.log(body);
            let $ = cheerio.load(body);
            let conTexts = $('#kbtable').html();
            let userName = $('#Top1_divLoginName').text();
            //根据返回的页面进行爬取和解析数据
            let conText = '<table>' + conTexts + '</table>';
            let news = {
                classs : conText
            }
            let names = {
                user:userName
            }
            jsonData[2] = news;
            jsonData[0] = names;
        })
    }
})

```

**注意，这里我只放课表扒取的逻辑，其余如个人信息查询、考试安排查询这些逻辑没放进去，小伙伴们可以自己试着写一下噢，需要完整代码也可以找我本人要滴，当然要有`HTTP`的概念和`JavaScript``Node`的基础才会写。**