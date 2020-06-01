# Node常用模块


## express

[Express](http://expressjs.com/) 是基于[Node.js](http://nodejs.org/)，高性能、一流的 web 开发框架。

```js
const app = express();
//设置可读取的静态目录
app.use('/', express.static(__dirname + '/'));
//全局use
app.use('/',(req,res,next)=>{})
//指定局部post，新增
app.post('/',(req,res,next)=>{})
//指定局部get，获取
app.get('/',(req,res,next)=>{})
//其他:put更新修改，delete删除
app.put('/',(req,res,next)=>{})
app.delete('/',(req,res,next)=>{})
// 打开监听端口
app.listen(3000, () => {
	console.log('running => 3000')
})
//设置允许跨域访问该服务
app.all('*', function(req, res, next) {
	res.header('Access-Control-Allow-Origin', '*');
	res.header('Access-Control-Allow-Methods', '*');
	// res.header('Access-Control-Allow-Headers', 'Content-Type');
	// res.header('Content-Type', 'application/json;charset=utf-8');
	next();
});
```

## express-session

登陆验证。

```js
var express = require('express');
var session = require('express-session');
//设置session参数
app.use(session({
    secret : "afei" //密钥，值随便填写
    ,rolling:true //只要用户和后端有交互（访问页面，跳转页面，ajax……），刷新存储时间
    ,resave:false //是否每次请求都重新存储session数据
    ,saveUninitialized:false //初始值
    ,cookie : {maxAge:1000*60*10} //设置session过期时间
    ,store : new mongooseSession({
        url : "mongodb://localhost:27017/blog"
    })//不设置store是服务器内存中存储session信息，我们可以通过设置store将session数据存到数据库
}));
//路由
app.use(function(req, res, next){
	// 如果cookie中存在，则说明已经登录
	if( req.session.user ){
        //res.locals = res.render
		res.locals.user = {
			uid : req.session.user.uid,
			username : req.session.user.username
		}
	}else{
		res.locals.user = {};
	}
	next();
})



//JWT与session归纳对比
//1、JWT无状态，可扩展和解耦。使用JWT不需要后端进行记录，每个token都是独立的。而session的诞生就是为了解决http无状态的问题，这也就说明服务端是有存储每个用户对应的session对象的，扩展性会更繁琐些
//2、跨域和CORS。每次发送请求到后端都需要检查JWT，只要验证通过就能处理请求。而Cookie只能在单域和子域中发挥作用
//3、JWT生成消耗一定的内存，而且体积较大，最小的它都比cookie要大，如果JWT里包含了许多声明，那问题就比较严重了，由于每次向服务器发起请求都要携带token，太大了会造成请求缓慢
//4、session比JWT好的地方在于在请求时浏览器会自动带http头部带上cookie，并且在用户持续使用时会不断地刷新session的过期时间，当浏览器关闭时自动清除session。相比之下JWT本身没法做到随着用户的使用而更新或手动清除，只能等自动过期
```

## express-ejs

模板，express自带，无需require，语法较与原生匹配。

```js
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');
```

## express-generator

Express 应用程序快速生成器。Express 脚手架。

```js
//安装
npm install -g express-generator
//创建
express -e

//创建完成，目录结构
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.pug
    ├── index.pug
    └── layout.pug

//其他命令
    -h, --help          输出使用方法
        --version       输出版本号
    -e, --ejs           添加对 ejs 模板引擎的支持
        --hbs           添加对 handlebars 模板引擎的支持
        --pug           添加对 pug 模板引擎的支持
    -H, --hogan         添加对 hogan.js 模板引擎的支持
        --no-view       创建不带视图引擎的项目
    -v, --view <engine> 添加对视图引擎（view） <engine> 的支持 (ejs|hbs|hjs|jade|pug|twig|vash) （默认是 jade 模板引擎）
    -c, --css <engine>  添加样式表引擎 <engine> 的支持 (less|stylus|compass|sass) （默认是普通的 css 文件）
        --git           添加 .gitignore
    -f, --force         强制在非空目录下创建
```



## stream

node自带。从流中读取数据。例子如：读取视频从流水的方式传输前端而不是一次性加载。

```javascript
var fs = require("fs");
var data = '菜鸟教程官网地址：www.runoob.com';

// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt');

// 使用 utf8 编码写入数据
writerStream.write(data,'UTF8');

// 标记文件末尾
writerStream.end();

// 处理流事件 --> data, end, and error
writerStream.on('finish', function() {
    console.log("写入完成。");
});

writerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
```



## Joi

验证规则器。检测客户端的传值是否为字符串，不能为字符串以外的东西，利于防止XXS攻击。

```js
// 引入joi模块
const Joi = require('joi');

// 定义对象的验证规则
const schema = {
	username: Joi.string().min(2).max(5).required().error(new Error('username属性没有通过验证')),
	birth: Joi.number().min(1900).max(2020).error(new Error('birth没有通过验证'))
};

async function run () {
	try {
		// 实施验证
		await Joi.validate({username: 'ab', birth: 1800}, schema);
	}catch (ex) {
		console.log(ex.message);
		return;
	}
	console.log('验证通过')
	
}

run();
```

## bcrypt

加密算法。

```js

```



## async

可控制并发、串行执行函数。

```js
//async.waterfall(tasks, [callback]);
//task是函数组成的数组，callback是中途出错或者全部执行完后的回调函数。
async.waterfall([
    function(callback){
        callback(null, 'one', 'two');
    },
    function(arg1, arg2, callback){
        callback(null, 'three');
    },
    function(arg1, callback){
        // arg1 now equals 'three'
        callback(null, 'done');
    }
], function (err, result) {
   // result now equals 'done'    
});

//async.mapLimit(arr, limit, iterator, callback)
//mapLimit是同时发起多个异步操作，然后一起等待callback的返回，返回一个就再发起下一个。
superagent.get(cnodeUrl).end(function(err, res) {
    if (err) {
        return console.error(err)
    }
    // 存放标题url的数组
    var topicUrls = [];
    var $ = cheerio.load(res.text);
    //获取首页所有的链接
    $('#topic_list .topic_title').each(function (idx, el) {
        if (idx < 40) {
            var $el = $(el);
            var href = url.resolve(cnodeUrl, $el.attr('href'));
            topicUrls.push(href);           
        }
    });
    //并发连接数的计数器
    var concurrencyCount = 0;
    var fetch = function (url, callback) {
        console.time('  耗时');
        concurrencyCount++;
        superagent.get(url).end( function (err, res) {
            console.log('并发数:', concurrencyCount--, 'fetch', url);
            //var $ = cheerio.load(res.text);
            callback(null, [url, res.text]);
        });

    }
    async.mapLimit(topicUrls, 11, function (topicUrl, callback) {
        fetch(topicUrl, callback);
        console.timeEnd("  耗时");
    }, function (err, result) {
        result = result.map( function (pair) {
            var $ = cheerio.load(pair[1]);
            return ({
                title: $('.topic_full_title').text().trim(),
                href: pair[0],
                comment1: $('.reply_content').eq(0).text().trim(),
                author1: $('.reply_author').eq(0).text().trim() || "评论不存在",    
            });
        });
        console.log('final:\n',result);
    });
});
```

## node-xlsx

生成 xlsx 表格文件。

```js
fs.writeFile('cnbNews.xlsx', xlsx.build(dataArr), 'utf-8', function(err){
	if(err){
		console.log('write file error!');
	}else{
		console.log('write file success!');
	}
});
```

## mailer

NodeJS 邮件发送模块，支持定制基于 Mustache 的模板正文。

```js
var nodemailer = require('nodemailer').createTransport({
	service: 'qq',
	auth: {
		user: 'binylu@qq.com',
		pass: 'ohclxvvmydtejejf' //授权码,通过QQ获取
	}
});
var mailOptions = {
	from: 'binylu@qq.com', // 发送者
	to: ['1559867170@qq.com,1393302015@qq.com'], // 接受者,可以同时发送多个,以逗号隔开
	subject: '小陆测试nodemailer2.5.0邮件发送', // 标题
	//text: 'Hello world', // 文本
	html: `<h2>小陆测试nodemailer基本使用:</h2>`,
	attachments: [
		{
			filename: 'package.json',
			path: './package.json'
		},
		{
			filename: 'content',
			content: '发送内容'
		}
	]
	//attachments:1可发送文件,2可发送自定义文件内容
};
nodemailer.sendMail(mailOptions, function(err, info) {
	if (err) {
		console.log(err);
		return;
	}
	...
});
```

## socket.io

适合构建跨浏览器的实时应用，提供类似 WebSockets 的API。常用：聊天室。

```js
//后端
io.on('connection', function(socket){
  console.log('a user connected')
  socket.on("join", function (name) {
    usocket[name] = socket
    io.emit("join", name)
  })
  socket.on("message", function (msg) {
    io.emit("message", msg) //将新消息广播出去
  })
});
```

```js
//前端
var name   = prompt("请输入你的昵称：");
var socket = io()

//发送昵称给后端，并更改网页title
socket.emit("join", name)
document.title = name + "的群聊"

socket.on("join", function (user) {
  addLine(user + " 加入了群聊")
})

//接收到服务器发来的message事件
socket.on("message", function(msg) {
  addLine(msg)
})

//当发送按钮被点击时
$('form').submit(function () {
  var msg = $("#m").val() //获取用户输入的信息
  socket.emit("message", msg) //将消息发送给服务器
  $("#m").val("") //置空消息框
  return false //阻止form提交
})

function addLine(msg) {
  $('#messages').append($('<li>').text(msg));
}
```


## node_redis

是为 NodeJS 而写的 Redis client，它支持所有 Redis 命令。

## body-parser

主要处理post的请求数据。

```js
//post支持传送的json数据量
app.use(bodyParser.json({
	limit: '100mb'
}));
// extended: true：表示使用第三方模块qs来处理
app.use(bodyParser.urlencoded({
	limit: '100mb',
	extended: false
}));
```

## swig

模板。语法有点生涩。

```js
// 静态页面类型为 html
app.engine('html', swig.renderFile);
// 读取地址固定: ./views
app.set('views', './');
app.set('view engine', 'html');
// 缓存关闭 
swig.setDefaults({
	cache: false
});
```

## formidable

上传文件处理。

```js
app.use('/upload', (req, res) => {
	var form = new formidable.IncomingForm();
	form.keepExtensions = true;
	form.uploadDir = "./upload";
	form.multiples = true;
	form.maxFieldsSize = 50 * 1024 * 1024;
	form.parse(req, function(err, fields, files) {
		for (var k in files) {
			fs.rename(files[k].path, "./upload/" + files[k].name,()=>{});
		}
		res.json({
			msg: 'success'
		})
	})
});
```

## querystring

字符串、对象处理。

```js
//querystring.parse
name=lisa&age=20&sex=女 => { name: 'lisa', age: '20', sex: '女' }
//querystring.stringify
{ name: 'lisa', age: '20', sex: '女' } => name=lisa&age=20&sex=女
//querystring.escape
name=大卫 => name%3D%E5%A4%A7%E5%8D%AB
//querystring.unescape
name%3D%E5%A4%A7%E5%8D%AB => name=大卫
```

## morgan

请求日志。

```js
// 请求日志
app.use(logger('dev'));
```



## log4js

所有请求以及错误、成功、警告日志管理。

```js
var log4js = require('log4js');

//logger 是log4js的实例
var logger = log4js.getLogger();

logger.trace('this is trace');
logger.debug('this is debug');
logger.info('this is info');
logger.warn('this is warn');
logger.error('this is error');
logger.fatal('this is fatal');
```



## compressing

文件压缩与解压缩。

```js
// 解压缩 参数:压缩包路径,输出路径
compressing.zip.uncompress(newName, './files/' + filesName + '/').then(() => {
	console.log('解压缩成功');
}).catch(err => {
	console.error(err);
});

// 压缩 参数:文件路径,压缩包输出路径
compressing.zip.compressDir('demo.js', 'demo/demo.zip').then(() => {
	console.log('success');
}).catch(err => {
	console.error(err);
});
```

## mysql

数据库操作。

```js
var connection = mysql.createConnection({
	host: 'localhost',
	user: 'root',
	password: 'root',
	port: '3306',
	database: 'book',
	multipleStatements: true //是否支持多行语句执行
});

connection.connect(function(err) {
	if (err) {
		console.log('数据库连接失败!');
		return;
	}
	console.log('数据库连接成功!');
});

var sql = 'SELECT COUNT(*) FROM book; SELECT * FROM book limit 1,10';
connection.query(sql, function(err, result) {
	if (err) {
		console.log('数据查询发生错误!', err);
		return;
	} else {
		...
	}
});
```



## mongoose

数据库。

```js
const mongoose = require('mongoose')

//1.链接数据库
mongoose.connect('mongodb://localhost/demo', {
        useNewUrlParser: true,
        useUnifiedTopology: true
    })
    .then(() => {
        console.log('连接成功')
    })
    .catch((err) => {
        console.log(err)
        console.log('连接失败')
    })

//2.创建Schema（模型对象）
let Schema = mongoose.Schema;
let personSchema = new Schema({
    name: String,
    age: Number,
    sex: {
        type: String,
        default: "男"
    },
    chat: String
});

//3.创建Model对象
let personModel = mongoose.model("person", personSchema);

// 3.1 增加
personModel.create([{
        name: "马红灯",
        age: 19,
        chat: "红灯1992"
    },
    {
        name: "龚志敏",
        age: 42,
        chat: "龚1992"
    },
    {
        name: "李发华",
        age: 32,
        chat: "发华1992"
    },
    {
        name: "李建华",
        age: 22,
        chat: "建华1992"
    },
    {
        name: "依依",
        age: 22,
        chat: "依依1992",
        sex: "女"
    },
], (err) => {
    if (!err) {
        console.log('插入成功')
    } else {
        throw err;
    }
})

// 3.2 查询
personModel.find({
    name: "依依"
}, (err, docs) => {
    if (!err) {
        console.log(docs)
    } else {
        throw err
    }
})


personModel.find({}, {
    name: 1,
    _id: 0
}, (err, docs) => { //定义查询结果显示字段
    if (!err) {
        console.log(docs)
    } else {
        throw err
    }
})

/* skip:2 开始位置  limit:2 取出信息条数*/
personModel.find({}, "-_id name sex chat", {
    skip: 2,
    limit: 2
}, (err, docs) => { //定义查询结果显示字段
    if (!err) {
        console.log(docs)
    } else {
        throw err
    }
})

// 修改
personModel.update({
    name: "钱森"
}, {
    $set: {
        age: 96
    }
}, {
    multi: true
}, (err) => {
    if (!err) {
        console.log("修改成功！！")
    } else {
        throw err
    }
});

// 删除
/* 
    Model.deleteMany()
    Model.deleteOne()
    Model.remove()
*/
personModel.remove({
    name: "钱森"
}, (err) => { //删除所有匹配
    if (!err) {
        console.log("删除成功！！")
    } else {
        throw err
    }
})
```



## apidoc

自动生成API文档。

```js
// 安装：npm install apidoc -g
// 生成：apidoc -i ./ -o apidoc/
// -i 后面是注释文件地址（可改动），-o 后面是生成文件地址（默认不改动）

/**
@api {post} http://www.binylu.cn:3005/login
@apiVersion 1.0.0
@apiGroup 登陆接口
@apiDescription 通过POST请求传递账号密码获取返回参数。

@apiParam {String} username 账号
@apiParam {String} password 密码
@apiSuccess {String} code 提示代码
@apiSuccess {String} msg 提示信息

@apiSuccessExample 请求成功例子
  {
    code: "200",
    msg: "登陆成功"
  }

@apiErrorExample  错误信息1
  {
    code: "-1",
    msg: "账号错误，没有此用户"
  }

@apiErrorExample  错误信息2
  {
    code: "-2",
    msg: "密码错误"
  }
**/
```

## cheerio

类似JQ操作DOM。

```js
var $ = cheerio.load(data.text);
```

## superagent

请求获取页面数据信息。

```js
superagent.get(url).end((err, res) => {
    if (err) {
        console.log('抓取失败 ', err)
    } else {
        var $ = cheerio.load(res.text);
		...
    }
})
```

## chalk

控制台打印字体颜色。

```js
console.log(chalk.green('成功'));
```

## request

请求获取页面数据信息。

```js
request({
    method: 'POST',
    url: url,
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    },
}, (err, red, body) => {
    let $ = cheerio.load(body);
	...
})
```

## nightmare

自动化测试包，处理动态页面。

```js
const Nightmare = require('nightmare'); // 自动化测试包，处理动态页面
const nightmare = Nightmare({
	show: true
}); // show:true  显示内置模拟浏览器

nightmare
    .goto('http://news.baidu.com/')
	.wait("div#local_news")
	.evaluate(() => document.querySelector("div#local_news").innerHTML)
	.then(htmlStr => {
		// 获取本地新闻数据
		localNews = getLocalNews(htmlStr)
	})
	.catch(error => {
		console.log(`本地新闻抓取失败 - ${error}`);
	})
```

## pkg

打包生成软件。

```js
//首先安装pkg
npm install -g pkg
//然后在项目目录下执行
pkg -t win package.json
```

```js
//注意这里使用path.join(__dirname, 'dist')而不是'dist'，虽然在命令行中执行起来效果是一样的，不过pkg打包会无法识别到dist目录
app.use(express.static(path.join(__dirname, 'dist')));
```

```json
{
    //其他配置项
    "bin": "service.js",//指定入口文件
    "pkg": {
        "assets": [
            "dist/**/*"//指定要打包的静态文件目录
        ]
    }
}
```

## nrm

用来换npm源的 nrm ls，能看每个源的延迟 nrm test ，然后nrm use XXX。

```js
npm i nrm -g
nrm ls
nrm use taobao
```

