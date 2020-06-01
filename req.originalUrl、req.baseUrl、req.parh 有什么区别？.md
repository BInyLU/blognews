url在解析过程是会被逐级删减。如：

```js
// app.js
...
var express = require('express');
var users = require('users');
var app = express();

app.use('/users', users);

module.exports = app;

// users.js
var express = require('express');
var router = express.Router();

router.count = 0;

// 错误的写法
/*
router.get('/users/id', function(req, res, next) {
    res.send('/user/id');
});
*/

// 正确的写法
router.get('/id', function(req, res, next) {
    res.send('/user/id');
});

module.exports = router;
```

采用第一种写法，浏览器会得到404 Not Found。第二种则正常。原因即在于，url在解析过程是会被逐级删减。

通过node-inspector调试上面的例子，可得到运行中req.url，req.originalUrl，req.baseUrl的值。

```js
// Debug Console
> req.url
"/id"
> req.originalUrl
"/user/id"
> req.baseUrl
"/user"
```

解释如下：
req.url = req.originalUrl - req.baseUrl。也是router.get传入的匹配路径的匹配对象。这也就可以解释上面的例子的运行结果了。all，post，put等等函数同理。

req.originalUrl与req.url类似。不同的是，它用于备份最初的请求url，从而你可以随意重写req.url来到处跳转，而不用担心丢失初始的url。比如，用于挂载中间函数的app.use()就会重写req.url来压缩挂载点的长度。

req.baseUrl存储当前router挂载的路径。即`app.use('/path', router);`。此时/path就是baseUrl。即便在挂载的时候使用的是正则等匹配表达式，baseUrl存储的也会是匹配的字符串，而不是正则表达式。