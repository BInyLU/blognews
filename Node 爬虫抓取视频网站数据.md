> **Node爬虫抓取视频网站数据**
>
> 本文章只取一部分代码作为解说并学习，如有需要可向博主提供；如涉及侵权请及时通知博主关闭。
>
> 扒取对象网站：https://1090ys.com/
>
> 扒取测试结果:
>
> - https://1090ys.com/play/3738~0~0.html
> - 该网站有随机四位字符串进行传值加密,不得多量访问
> - 所以先扒取该视频简介id,发送到前端渲染后 (id例子如上方:3738)
> - 前端点击id,后端接收前端id,再进行扒取单个视频
> - 使用`superagent.get()`方法来访问页面

### 初始化：

```javascript
npm install
```

```javascript
node demo.js
```

- 先初始化后端，再打开前端进行请求后端

### **Node部分：**

- 主要使用`superagent`和`cheerio`。
- `superagent`它是一个强大并且可读性很好的轻量级`ajaxAPI`，是一个关于`HTTP`方面的一个库，而且它可以将链式写法玩的出神入化。
- `cheerio`是类似于`Jquery`的一个库，它可以支持`Jquery`的写法去获取当前访问的页面`DOM`。
- 其余模块如`express`，`fs`，都是`Node`常用模块，就不一一解释。
- 我们这里用到了`superagent`的`get`请求方式取获取当前页面信息进行扒取，找到我们需要的相应信息后用`cheerio`去获取元素信息，再把它保存到一个数组里去，就可以发送给前端调用了。

**新建一个demo.js**

```javascript
//引入模块
const express = require('express');
const cheerio = require('cheerio');
const superagent = require('superagent');
const fs = require('fs');
const app = express();

// 跨域支持
app.all('*', function(req, res, next) {
	res.header('Access-Control-Allow-Origin', '*');
	res.header('Access-Control-Allow-Headers', 'Content-Type');
	res.header('Access-Control-Allow-Methods', '*');
	res.header('Content-Type', 'application/json;charset=utf-8');
	next();
});

// 打开监听端口
var server = app.listen(3000, function() {
	var host = server.address().address;
	var port = server.address().port;
	console.log('请打开 http://localhost:' + port + '/');
});

// 接收前端id
app.get('/1090ys', function(req, res) {
	var result = req.query;
	console.log(result.id);
	sid = result.id;
	getMain(sid);
    
	//把数据返回给前端
	res.send({
		success: '1',
		singNews: singNews
	})
})

// 对指定id进行扒取
function getMain(sid) {
	superagent.get('https://1090ys.com/play/' + sid + '~0~0.html').end((err, res) => {
		if (err) {
			console.log(`抓取失败 - ${err}`)
		} else {
			var $ = cheerio.load(res.text);
			var news = {
				title: $('.center>font:nth-child(1)').html(),
				content: $('.stui-player__video').html(),
				table: $('.stui-pannel:nth-child(4)').html()
				// link: $('table table a').text(),
			};
			singNews = news;
			console.log(`抓取成功`)
		}
	})
}
```

**新建一个配置文件：package.json**

```json
{
  "name": "demo",
  "version": "1.0.0",
  "description": "demo",
  "main": "demo.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "cheerio": "^1.0.0-rc.3",
    "express": "^4.17.1",
    "fs": "0.0.1-security",
    "http": "0.0.0",
    "superagent": "^5.1.2"
  }
}

```

**前端部分：demo.html**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<title></title>
	</head>
	<style type="text/css">
		iframe{width: 550px;height: 400px; }
		.title{padding: 10px;margin: 10px; background: #000;color: #FFFFFF;}
	</style>
	<body>
		<input type="number" value="9" id="ids"/>
		<button type="button" onclick="inputs()">点我扒取</button>
		
		<div class="box"></div>
		
		<script type="text/javascript">
			
			// 请求地址
			var server = 'http://localhost:3000/1090ys';

            // 获取用户输入ID
			function inputs(){
				var ids = document.querySelector('#ids').value;
				sends(ids);
				
			}
            
            //发送ID
            function sends(ids){
                xmlHttpRequest = new XMLHttpRequest();
                xmlHttpRequest.onreadystatechange = innerAnsw;
                xmlHttpRequest.open("GET", server + '?id=' + ids, true);
                xmlHttpRequest.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
                xmlHttpRequest.send();
            }
			
            //把数据渲染内容
			function innerAnsw() {
                setTimeout(function(){
                    if (xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200) {
                        var res = xmlHttpRequest.responseText;
                        var data = JSON.parse(res);
                        console.log(data);
                        if(data.success == 1) console.log('扒取成功');
                        var datas = data.singNews;
                        document.querySelector('.box').innerHTML = '<div class="title">' + datas.title + '</div>' + datas.content + datas.table;
                    }
                },2000)
            }

		</script>
	</body>
</html>

```

