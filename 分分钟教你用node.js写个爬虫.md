## 分分钟教你用node.js写个爬虫

### 写在前面

> 十分感谢大家的点赞和关注。其实，这是我第一次在`segmentfault`上写文章。因为我也是前段时间偶然之间才开始了解和学习爬虫，而且学习node的时间也不是很长。虽然用node做过一些后端的项目，但其实在node和爬虫方面我还是一个新人，这篇文章主要是想和大家分享一下node和爬虫方面的基本知识，希望对大家有帮助，也想和大家一起交流，一起学习，再次谢谢大家的支持！

**对了，我开通了个人的 GitHub主页 ，里面有自己的技术文章，还会有个人的随想、思考和日志。以后所有的文章都会第一时间更新到这里，然后同步到其他平台。有喜欢的朋友可以没事去逛逛，再次感谢大家的支持！**

### 一、什么是爬虫

> [网络爬虫](https://baike.baidu.com/item/%E7%BD%91%E7%BB%9C%E7%88%AC%E8%99%AB/5162711?fr=aladdin&fromid=22046949&fromtitle=%E7%88%AC%E8%99%AB)（又被称为网页蜘蛛，网络机器人，在`FOAF`社区中间，更经常的称为网页追逐者），是一种按照一定的规则，自动地抓取万维网信息的程序或者脚本。另外一些不常使用的名字还有蚂蚁、自动索引、模拟程序或者蠕虫。
> [WIKIPEDIA 爬虫介绍](https://en.wikipedia.org/wiki/Web_crawler)

### 二、爬虫的分类

- 通用网络爬虫（全网爬虫）

> 爬行对象从一些 `种子URL` 扩充到整个 Web，主要为门户站点搜索引擎和大型 Web 服务提供商采集数据。

![通用爬虫工作流程](https://segmentfault.com/img/bVbajfb?w=665&h=333)

- 聚焦网络爬虫（主题网络爬虫）

> 是 `指选择性` 地爬行那些与预先定义好的主题相关页面的网络爬虫。

- 增量式网络爬虫

> 指对已下载网页采取增量式更新和 `只爬行新产生的或者已经发生变化网页` 的爬虫，它能够在一定程度上保证所爬行的页面是尽可能新的页面。

- Deep Web 爬虫

> 爬行对象是一些在用户填入关键字搜索或登录后才能访问到的`深层网页信息`的爬虫。

### 三、爬虫的爬行策略

- 通用网络爬虫（全网爬虫）

> 深度优先策略、广度优先策略

![页面关系模拟树状结构](https://segmentfault.com/img/bVbajko?w=300&h=247)

- 聚焦网络爬虫（主题网络爬虫）

> 基于内容评价的爬行策略（内容相关性），基于链接结构评价的爬行策略、基于增强学习的爬行策略（链接重要性），基于语境图的爬行策略（距离，图论中两节点间边的权重）

- 增量式网络爬虫

> 统一更新法、个体更新法、基于分类的更新法、自适应调频更新法

- Deep Web 爬虫

> Deep Web 爬虫爬行过程中最重要部分就是表单填写，包含两种类型：基于领域知识的表单填写、基于网页结构分析的表单填写

**现代的网页爬虫的行为通常是四种策略组合的结果：**

> 选择策略：决定所要下载的页面；
> 重新访问策略：决定什么时候检查页面的更新变化；
> 平衡礼貌策略：指出怎样避免站点超载；
> 并行策略：指出怎么协同达到分布式抓取的效果；

![现代分布式爬虫系统](https://segmentfault.com/img/bVbajgJ?w=450&h=375)

### 四、写一个简单网页爬虫的流程

1. 确定爬取对象（网站/页面）
2. 分析页面内容（目标数据/DOM结构）
3. 确定开发语言、框架、工具等
4. 编码 测试，爬取数据
5. 优化

### 一个简单的百度新闻爬虫

#### 确定爬取对象（网站/页面）

> [百度新闻](http://news.baidu.com/) （<http://news.baidu.com/>）

#### 分析页面内容（目标数据/DOM结构）

> ······

#### 确定开发语言、框架、工具等

> ```
> node.js (express)` + `SublimeText 3
> ```

#### 编码，测试，爬取数据

> coding ···

### Let's start

#### 新建项目目录

> 1.在合适的磁盘目录下创建项目目录`baiduNews`（我的项目目录是：`F:\web\baiduNews`）

**注：因为在写这篇文章的时候用的电脑真心比较渣。安装WebStorm或者VsCode跑项目有些吃力。所以后面的命令行操作我都是在Window自带的DOS命令行窗口中执行的。**

#### 初始化package.json

> 1.在DOS命令行中进入项目根目录 `baiduNews`
> 2.执行`npm init`，初始化`package.json`文件

#### 安装依赖

> `express` （使用express来搭建一个简单的Http服务器。当然，你也可以使用node中自带的`http`模块）
> `superagent` （superagent是node里一个非常方便的、轻量的、渐进式的第三方客户端请求代理模块，用他来请求目标页面）
> `cheerio` （cheerio相当于node版的jQuery，用过jQuery的同学会非常容易上手。它主要是用来获取抓取到的页面元素和其中的数据信息）

```
// 个人比较喜欢使用yarn来安装依赖包,当然你也可以使用 npm install 来安装依赖，看个人习惯。
yarn add express
yarn add superagent
yarn add cheerio
```

依赖安装完成后你可以在package.json中查看刚才安装的依赖是否成功。
安装正确后如下图：

![安装依赖包](https://segmentfault.com/img/bVbavQh?w=286&h=114)

#### 开始coding

**一、使用express启动一个简单的本地Http服务器**

1、在项目根目录下创建`index.js`文件（后面都会在这个index文件中进行coding）

2、创建好`index.js`后，我们首先实例化一个`express`对象，用它来启动一个本地监听`3000`端口的Http服务。

```
const express = require('express');
const app = express();

// ...

let server = app.listen(3000, function () {
  let host = server.address().address;
  let port = server.address().port;
  console.log('Your App is running at http://%s:%s', host, port);
});
```

对，就是这么简单，不到10行代码，搭建启动一个简单的本地Http服务。

3、按照国际惯例，我们希望在访问本机地址`http://localhost:3000`的时候，这个服务能给我们犯规一个`Hello World！`在`index.js`中加入如下代码：

```
app.get('/', function (req, res) {
  res.send('Hello World!');
});
```

> 此时，在DOS中项目根目录`baiduNews`下执行`node index.js`,让项目跑起来。之后，打开浏览器，访问`http://localhost:3000`,你就会发现页面上显示'Hellow World!'字样。
> 这样，在后面我们获取到百度新闻首页的信息后，就可以在访问`http://localhost:3000`时看到这些信息。

**二、抓取百度新闻首页的新闻信息**

1、 首先，我们先来分析一下百度新闻首页的页面信息。
![百度新闻首页](https://segmentfault.com/img/bVbarP2?w=1291&h=778)
![百度新闻首页](https://segmentfault.com/img/bVbarQa?w=1026&h=907)

> 百度新闻首页大体上分为“热点新闻”、“本地新闻”、“国内新闻”、“国际新闻”......等。这次我们先来尝试抓取左侧`“热点新闻”`和下方的`“本地新闻”`两处的新闻数据。

![热点新闻DOM结构](https://segmentfault.com/img/bVbarVz?w=1920&h=754)

> `F12`打开`Chrome`的控制台，审查页面元素，经过查看左侧“热点新闻”信息所在`DOM`的结构，我们发现所有的“热点新闻”信息（包括新闻标题和新闻页面链接）都在`id`为`#pane-news`的`<div`>下面`<ul>`下`<li>`下的`<a>`标签中。用`jQuery`的选择器表示为：`#pane-news ul li a`。

2、为了爬取新闻数据，首先我们要用superagent请求目标页面，获取整个新闻首页信息

```
// 引入所需要的第三方包
const superagent= require('superagent');

let hotNews = [];                                // 热点新闻
let localNews = [];                              // 本地新闻

/**
 * index.js
 * [description] - 使用superagent.get()方法来访问百度新闻首页
 */
superagent.get('http://news.baidu.com/').end((err, res) => {
  if (err) {
    // 如果访问失败或者出错，会这行这里
    console.log(`热点新闻抓取失败 - ${err}`)
  } else {
   // 访问成功，请求http://news.baidu.com/页面所返回的数据会包含在res
   // 抓取热点新闻数据
   hotNews = getHotNews(res)
  }
});
```

3、获取页面信息后，我们来定义一个函数`getHotNews()`来抓取页面内的“热点新闻”数据。

```
/**
 * index.js
 * [description] - 抓取热点新闻页面
 */
// 引入所需要的第三方包
const cheerio = require('cheerio');

let getHotNews = (res) => {
  let hotNews = [];
  // 访问成功，请求http://news.baidu.com/页面所返回的数据会包含在res.text中。
  
  /* 使用cheerio模块的cherrio.load()方法，将HTMLdocument作为参数传入函数
     以后就可以使用类似jQuery的$(selectior)的方式来获取页面元素
   */
  let $ = cheerio.load(res.text);

  // 找到目标数据所在的页面元素，获取数据
  $('div#pane-news ul li a').each((idx, ele) => {
    // cherrio中$('selector').each()用来遍历所有匹配到的DOM元素
    // 参数idx是当前遍历的元素的索引，ele就是当前便利的DOM元素
    let news = {
      title: $(ele).text(),        // 获取新闻标题
      href: $(ele).attr('href')    // 获取新闻网页链接
    };
    hotNews.push(news)              // 存入最终结果数组
  });
  return hotNews
};
```

> 这里要多说几点：
>
> 1. `async/await`据说是异步编程的终级解决方案,它可以让我们以同步的思维方式来进行异步编程。`Promise`解决了异步编程的“回调地狱”，async/await同时使异步流程控制变得友好而有清晰，有兴趣的同学可以去了解学习一下，真的很好用。
> 2. `superagent`模块提供了很多比如`get`、`post`、`delte`等方法，可以很方便地进行Ajax请求操作。在请求结束后执行`.end()`回调函数。`.end()`接受一个函数作为参数，该函数又有两个参数`error和res`。当请求失败，`error`会包含返回的错误信息，请求成功，`error`值为`null`，返回的数据会包含在`res`参数中。
> 3. `cheerio`模块的`.load()`方法，将`HTML document`作为参数传入函数，以后就可以使用类似jQuery的$(selectior)的方式来获取页面元素。同时可以使用类似于`jQuery`中的`.each()`来遍历元素。此外，还有很多方法，大家可以自行Google/Baidu。

4、将抓取的数据返回给前端浏览器

> 前面，`const app = express();`实例化了一个`express`对象`app`。
> `app.get('', async() => {})`接受两个参数，第一个参数接受一个String类型的路由路径，表示Ajax的请求路径。第二个参数接受一个函数Function，当请求此路径时就会执行这个函数中的代码。

```
/**
 * [description] - 跟路由
 */
// 当一个get请求 http://localhost:3000时，就会后面的async函数
app.get('/', async (req, res, next) => {
  res.send(hotNews);
});
```

> 在DOS中项目根目录`baiduNews`下执行`node index.js`,让项目跑起来。之后，打开浏览器，访问`http://localhost:3000`,你就会发现抓取到的数据返回到了前端页面。我运行代码后浏览器展示的返回信息如下：
> **注：因为我的Chrome安装了JSONView扩展程序，所以返回的数据在页面展示的时候会被自动格式化为结构性的JSON格式，方便查看。**

![热点新闻抓取结果](https://segmentfault.com/img/bVbar41?w=1919&h=952)

**OK！！这样，一个简单的百度“热点新闻”的爬虫就大功告成啦！！**

> 简单总结一下，其实步骤很简单：
>
> 1. `express`启动一个简单的`Http`服务
> 2. 分析目标页面`DOM`结构，找到所要抓取的信息的相关`DOM`元素
> 3. 使用`superagent`请求目标页面
> 4. 使用`cheerio`获取页面元素，获取目标数据
> 5. 返回数据到前端浏览器

**现在，继续我们的目标，抓取“本地新闻”数据（编码过程中，我们会遇到一些有意思的问题）**
有了前面的基础，我们自然而然的会想到利用和上面相同的方法“本地新闻”数据。
1、 分析页面中“本地新闻”部分的`DOM`结构，如下图：

![百度新闻本地新闻](https://segmentfault.com/img/bVbasP1?w=1920&h=881)

> `F12`打开控制台，审查“本地新闻”`DOM`元素，我们发现，“本地新闻”分为两个主要部分，“左侧新闻”和右侧的“新闻资讯”。这所有目标数据都在`id`为`#local_news`的`div`中。“左侧新闻”数据又在`id`为`#localnews-focus`的`ul`标签下的`li`标签下的`a`标签中，包括新闻标题和页面链接。“本地资讯”数据又在`id`为`#localnews-zixun`的`div`下的`ul`标签下的`li`标签下的`a`标签中，包括新闻标题和页面链接。

2、OK！分析了`DOM`结构，确定了数据的位置，接下来和爬取`“热点新闻”`一样，按部就班，定义一个`getLocalNews()`函数，爬取这些数据。

```
/**
 * [description] - 抓取本地新闻页面
 */
let getLocalNews = (res) => {
  let localNews = [];
  let $ = cheerio.load(res);
    
  // 本地新闻
  $('ul#localnews-focus li a').each((idx, ele) => {
    let news = {
      title: $(ele).text(),
      href: $(ele).attr('href'),
    };
    localNews.push(news)
  });
    
  // 本地资讯
  $('div#localnews-zixun ul li a').each((index, item) => {
    let news = {
      title: $(item).text(),
      href: $(item).attr('href')
    };
    localNews.push(news);
  });

  return localNews
};
```

对应的，在`superagent.get()`中请求页面后，我们需要调用`getLocalNews()`函数，来爬去本地新闻数据。
`superagent.get()`函数修改为：

```
superagent.get('http://news.baidu.com/').end((err, res) => {
  if (err) {
    // 如果访问失败或者出错，会这行这里
    console.log(`热点新闻抓取失败 - ${err}`)
  } else {
   // 访问成功，请求http://news.baidu.com/页面所返回的数据会包含在res
   // 抓取热点新闻数据
   hotNews = getHotNews(res)
   localNews = getLocalNews(res)
  }
});
```

同时，我们要在`app.get()`路由中也要将数据返回给前端浏览器。`app.get()`路由代码修改为：

```
/**
 * [description] - 跟路由
 */
// 当一个get请求 http://localhost:3000时，就会后面的async函数
app.get('/', async (req, res, next) => {
  res.send({
    hotNews: hotNews,
    localNews: localNews
  });
});
```

> 编码完成，激动不已！！`DOS`中让项目跑起来，用浏览器访问`http://localhost:3000`

**尴尬的事情发生了！！返回的数据只有热点新闻，而本地新闻返回一个空数组[ ]。检查代码，发现也没有问题，但为什么一直返回的空数组呢？**
**经过一番原因查找，才返现问题出在哪里！！**

### 一个有意思的问题

> 为了找到原因，首先，我们看看用`superagent.get('http://news.baidu.com/').end((err, res) => {})`请求百度新闻首页在回调函数`.end()`中的第二个参数res中到底拿到了什么内容？

```
// 新定义一个全局变量 pageRes
let pageRes = {};        // supergaent页面返回值

// superagent.get()中将res存入pageRes
superagent.get('http://news.baidu.com/').end((err, res) => {
  if (err) {
    // 如果访问失败或者出错，会这行这里
    console.log(`热点新闻抓取失败 - ${err}`)
  } else {
   // 访问成功，请求http://news.baidu.com/页面所返回的数据会包含在res
   // 抓取热点新闻数据
   // hotNews = getHotNews(res)
   // localNews = getLocalNews(res)
   pageRes = res
  }
});

// 将pageRes返回给前端浏览器，便于查看
app.get('/', async (req, res, next) => {
  res.send({
    // {}hotNews: hotNews,
    // localNews: localNews,
    pageRes: pageRes
  });
});
```

> 访问浏览器`http://localhost:3000`，页面展示如下内容：

![superagent.get()请求返回值](https://segmentfault.com/img/bVbauoa?w=1901&h=783)

> 可以看到，返回值中的`text`字段应该就是整个页面的`HTML`代码的字符串格式。为了方便我们观察，可以直接把这个`text`字段值返回给前端浏览器，这样我们就能够清晰地看到经过浏览器渲染后的页面。

修改给前端浏览器的返回值

```
app.get('/', async (req, res, next) => {
  res.send(pageRes.text)
}
```

访问浏览器`http://localhost:3000`，页面展示如下内容：

![本地新闻返回页面](https://segmentfault.com/img/bVbauqO?w=1817&h=876)

> 审查元素才发现，原来我们抓取的目标数据所在的`DOM`元素中是空的，里面没有数据！
> 到这里，一切水落石出！在我们使用`superagent.get()`访问百度新闻首页时，`res`中包含的获取的页面内容中，我们想要的“本地新闻”数据还没有生成，`DOM`节点元素是空的，所以出现前面的情况！抓取后返回的数据一直是空数组`[ ]`。

![本地新闻请求接口](https://segmentfault.com/img/bVbaurN?w=715&h=96)

> 在控制台的`Network`中我们发现页面请求了一次这样的接口：
> `http://localhost:3000/widget?id=LocalNews&ajax=json&t=1526295667917`，接口状态 `404`。
> 这应该就是百度新闻获取`“本地新闻”`的接口，到这里一切都明白了！“本地新闻”是在页面加载后动态请求上面这个接口获取的，所以我们用`superagent.get()`请求的页面再去请求这个接口时，接口`URL`中`hostname`部分变成了本地`IP`地址，而本机上没有这个接口，所以`404`，请求不到数据。

**找到原因，我们来想办法解决这个问题！！**

> 1. 直接使用`superagen`t访问正确合法的百度`“本地新闻”`的接口，获取数据后返回给前端浏览器。
> 2. 使用第三方`npm`包，模拟浏览器访问百度新闻首页，在这个模拟浏览器中当`“本地新闻”`加载成功后，抓取数据，返回给前端浏览器。

**以上方法均可，我们来试试比较有意思的第二种方法**

### 使用`Nightmare`自动化测试工具

> ```
> Electron
> ```
>
> 可以让你使用纯
>
> ```
> JavaScript
> ```
>
> 调用
>
> ```
> Chrome
> ```
>
> 丰富的原生的接口来创造桌面应用。你可以把它看作一个专注于桌面应用的
>
> ```
> Node.js
> ```
>
> 的变体，而不是
>
> ```
> Web
> ```
>
> 服务器。其基于浏览器的应用方式可以极方便的做各种响应式的交互
>
> `Nightmare`是一个基于`Electron`的框架，针对`Web`自动化测试和爬虫，因为其具有跟`PlantomJS`一样的自动化测试的功能可以在页面上模拟用户的行为触发一些异步数据加载，也可以跟`Request`库一样直接访问`URL`来抓取数据，并且可以设置页面的延迟时间，所以无论是手动触发脚本还是行为触发脚本都是轻而易举的。

#### 安装依赖

```
// 安装nightmare
yarn add nightmare
```

#### 为获取“本地新闻”，继续coding...

给`index.js`中新增如下代码：

```
const Nightmare = require('nightmare');          // 自动化测试包，处理动态页面
const nightmare = Nightmare({ show: true });     // show:true  显示内置模拟浏览器

/**
 * [description] - 抓取本地新闻页面
 * [nremark] - 百度本地新闻在访问页面后加载js定位IP位置后获取对应新闻，
 * 所以抓取本地新闻需要使用 nightmare 一类的自动化测试工具，
 * 模拟浏览器环境访问页面，使js运行，生成动态页面再抓取
 */
// 抓取本地新闻页面
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

修改`getLocalNews()`函数为：

```
/**
 * [description]- 获取本地新闻数据
 */
let getLocalNews = (htmlStr) => {
  let localNews = [];
  let $ = cheerio.load(htmlStr);

  // 本地新闻
  $('ul#localnews-focus li a').each((idx, ele) => {
    let news = {
      title: $(ele).text(),
      href: $(ele).attr('href'),
    };
    localNews.push(news)
  });

  // 本地资讯
  $('div#localnews-zixun ul li a').each((index, item) => {
    let news = {
      title: $(item).text(),
      href: $(item).attr('href')
    };
    localNews.push(news);
  });

  return localNews
}
```

修改`app.get('/')`路由为：

```
/**
 * [description] - 跟路由
 */
// 当一个get请求 http://localhost:3000时，就会后面的async函数
app.get('/', async (req, res, next) => {
  res.send({
    hotNews: hotNews,
    localNews: localNews
  })
});
```

> 此时，`DOS`命令行中重新让项目跑起来，浏览器访问`https://localhost:3000`，看看页面展示的信息，看是否抓取到了`“本地新闻”`数据！

**至此，一个简单而又完整的抓取百度新闻页面“热点新闻”和“本地新闻”的爬虫就大功告成啦！！**

**最后总结一下，整体思路如下：**

> 1. `express`启动一个简单的`Http`服务
> 2. 分析目标页面`DOM`结构，找到所要抓取的信息的相关`DOM元`素
> 3. 使用`superagent`请求目标页面
> 4. **动态页面（需要加载页面后运行JS或请求接口的页面）可以使用Nightmare模拟浏览器访问**
> 5. 使用`cheerio`获取页面元素，获取目标数据

### 完整代码

> 爬虫完整代码GitHub地址：[完整代码](https://github.com/webCaolixin/news-Spider)

**后面，应该还会做一些进阶，来爬取某些网站上比较好看的图片（手动滑稽），会牵扯到并发控制和反-反爬虫的一些策略。再用爬虫取爬去一些需要登录和输入验证码的网站，欢迎到时大家关注和指正交流。**

### 我想说

> `再次感谢大家的点赞和关注和评论，谢谢大家的支持，谢谢！我自己觉得我算是一个爱文字，爱音乐，同时也喜欢coding的半文艺程序员。`之前也一直想着写一写技术性和其他偏文学性的文章。虽然自己的底子没有多么优秀，但总是觉得在写文章的过程中，不论是技术性的还是偏文学性的，这个过程中可以督促自己去思考，督促自己去学习和交流。`毕竟每天忙忙碌碌之余，还是要活出自己不一样的生活。`所以，以后如果有一些好的文章我会积极和大家分享！再次感谢大家的支持！