## NodeJs核心技术

### 一、知识结构：

http模块：配置简单 的web服务，npm/cnpm工具

express框架：express中间件进行服务配置；路由；请求处理；

DB服务：学习使用mysql关系型数据库；

web接口服务：使用express、koa简单配置接口服务、JSON解析；

nodejs RESTful API：提供跨语言、跨平台的服务接口、支持web/app

node文件系统：服务端基本的文件读写操作

------

### 二、Node.js简介：

![img](https://img-blog.csdn.net/20180915211853618?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2R1YW5zYW12ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

Node.js是一个让JavaScript运行在服务器端的开发平台，它让JavaScript的触角伸到了服务器端。但Node.js似乎与其它服务器端语言有点不同：

Node.js不是一种独立的语言，与PHP、Python等“既是语言，又是平台”不同，Node.js使用的是JavaScript进行编程，Node.js是一个工具，语言仍是JavaScript。

与PHP、JSP等相比，Node.js跳过了apache、tomcat、nginx、iis等**http服务器**，它自己不用建立在任何服务器软件之上。

**Node.js哲学：花最小的硬件成本，追求更高的并发，更高的处理性能。**

Node采用一系列“非阻塞”库来支持事件循环的方式。本质上就是为文件系统、数据库之类的资源提供接口。向文件系统发送一个请求时，无需等待硬盘（寻址并检索文件），硬盘准备好的时候非阻塞接口会通知Node。该模型以可扩展的方式简化了对慢资源的访问， 直观，易懂。尤其是对于熟悉onmouseover、onclick等DOM事件的用户，更有一种似曾相识的感觉。

### Node.js特点：

单线程：

说明Node.js是单线程的一个实例 ：

```javascript
var http = require('http');

var a = 0;

http.createServer(function (request, response) {
    a++;

    // 发送 HTTP 头部
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});

    // 发送响应数据 "Hello World"
    response.end(a.toString());
}).listen(8888);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### Node.js的优点:

Node.js可以在不新增额外线程的情况下，依然可以对任务进行并发处理 —— Node.js是单线程的。它通过事件循环（event loop）来实现并发操作，对此，我们应该要充分利用这一点 —— 尽可能的避免阻塞操作，取而代之，多使用非阻塞操作。

事件驱动：

异步回调：相当于一个服务员照顾多个顾客

说明实例：当有多个用户同时访问的时候，会出现同一个用户进来和读取完毕不连续的情况

```javascript
//创建服务器用的
var http = require('http');
//读取文件用的
var fs = require('fs');

http.createServer(function (req, res) {
    //ip地址
    var ip = req.connection.remoteAddress;
    console.log(ip + "来了，开始读取文件！");

    //来客人之后的事情，要去读取一个文本文件
    fs.readFile('./input.txt', function(err, filecontent){
        res.writeHead(200, {'Content-Type': 'text/plain'});
        res.end(filecontent);
        //提示
        console.log(ip + "读取文件完毕！");
    });
}).listen(8888);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

只要I/O越多，Node.js宏观上越并行；但运算越多，Node.js宏观上越不并行，此时网页打开速度严重变慢，因为计算过程中CPU只能为某一用户服务，难以脱身，所以Node.js线程就被这一用户霸占了。

因此Node.js适合开发I/O多的业务，而不适合计算任务繁重的业务。

![img](https://img-blog.csdn.net/20180909105044791?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2R1YW5zYW12ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

非阻塞I/O：

例如：当访问数据库取得数据的时候，需要一段较长的时间，在传统的处理机制中，在执行了访问数据库代码之后，整个线程都将暂停下来等待数据库返回结果才能执行后面的代码，也就是说I/O阻塞了后面代码的执行，极大的降低了程序的执行效率。

由于Node.js采用了非阻塞I/O机制，因此在执行了访问数据库的代码之后，将立即转而执行其后面的代码，将数据库返回结果的处理代码放在回调函数中，从而提高了程序的执行效率。

当某个I/O执行完毕时，将以事件的形式通知执行I/O操作的线程，线程执行这个事件的回调函数。为了处理异步I/O，线程必须有事件循环，不断的检查有没有未处理的事件，依次予以处理。

```javascript
var fs = require('fs');

fs.readFile("./input.txt", "utf8", function(err, data) {
    console.log(data);
});

var waitTill = new Date(new Date().getTime() + 2 * 1000);
while (waitTill > new Date()) {}

console.log("finished");

//如上所示，如果代码中有同步执行的代码，不管这个同步执行的代码需要执行多久，它前面的异步代码总是在最后才执行
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 在阻塞模式下，一个线程只能处理一项任务，要想提高吞吐量必须通过多线程。而非阻塞模式下，一个线程永远在执行计算操作，这个线程的核心利用率永远是100%，所以这个是一个特别有哲理的解决方案：与其人多，但好多人闲着，还不如一个人玩命，往死里干活。

Node..js适合开发的业务：

当业务程序需要处理大量并发的I/O，而在向客户端发出响应之前，应用程序内部并不需要进行非常复杂的处理的时候，Node..js非常合适。Node..js也非常适合和websocket配合，开发长连接的实时交互应用程序，比如：用户表单收集、考试系统、聊天室、图文直播、提供JSON的API（为MVVM框架使用）。

------

### 三、命令行窗口（cmd窗口/终端）：

1、常用的命令：

dir：列出当前文件夹下的所有文件；

cd 目录名：进入到指定的目录；

md 目录名：创建一个指定的文件夹；

rd 目录名：删除一个指定的文件夹；

2、目录：

.：表示当前目录；

..：表示上一级目录；

3、环境变量：

path：当我们在命令行窗口打开一个文件或调用一个程序时系统会首先在当前目录下寻找程序，如果找到了则直接打开，如果没有找到则会依次到环境变量path的路径中寻找，直到找到为止，如果没有找到则会报错。

4、快速进入指定文件夹的方法：

在指定文件夹的地址栏中输入：cmd

------

### 四、进程和线程：

进程：负责为程序的运行提供必备的环境，相当于工厂中的车间；

线程：计算机中最小的计算单元，负责执行进程中的程序，相当于工厂中的工人。

------

### 五、node执行js文件：

cmd中进入hello.js所在文件夹，cmd中输入：

```javascript
node hello.js
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

### 六、node整合WebStorm：

WebStorm菜单的File->Settings：搜索node，找到Node.js and NPM，在右侧的Node interpreter中输入node.exe所在位置即可。

------

### 七、WebStorm中node代码提示：

WebStorm菜单的File->Settings：搜索node，找到Node.js and NPM，在右侧的Coding Assistance中启用即可。

------

### 八、模块化：

在Node中，一个js文件就是一个模块；

在 Node中，每一个js文件的js代码都是独立运行在一个函数中，比如：

```javascript
console.log("我是模块01.moudle.js!");
var x = 20;
var y = 30;
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

其实是：

```javascript
function(){
    console.log("我是模块01.moudle.js!");
    var x = 20;
    var y = 30;
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

而不是全局作用域，所以一个模块中的变量和函数在其它模块中无法访问 。

在Node中，通过require()函数来引入外部模块，require()中可以传递一个文件的路径作为参数，node将会自动根据该路径来引入外部模块，这里路径如果使用相对路径，则必须以“.”或者“..”开头；

我们可以通过exports来向外部暴露变量或者方法，只需要将需要暴露给外部的变量或者方法设置为exprots的属性即可。

```javascript
console.log("我是模块01.moudle.js!");
exports.x = 20;
exports.y = 30;
exports.fn = function(){
    console.log("我是模块01.moudle.js中的一个函数!");
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

使用require()引入模块以后，该函数会返回一个对象，这个对象代表的是引入的模块。

我们使用require()引入外部模块时，使用的就是模块标识。

模块分成两大类：

核心模块：由node引擎提供的模块，核心模块的标识就是模块的名字；

文件模块：用户自定义的模块，文件模块的标识就是文件的路径。

node模块中用var定义的变量都是局部变量，取消掉var时定义的变量才是全部变量：

```javascript
a = 10;
console.log(global.a);//global是node中的一个全局对象，它的作用和网页中的window类似，在全局中创建的变量都会用global的属性保存，在全局中创建的函数都会作为global的方法保存
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```
var a = 10;
console.log(arguments);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```javascript
var a = 10;
console.log(arguments.callee + "");//arguments.callee保存的是当前执行的函数对象
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

输出：

```javascript
function (exports, require, module, __filename, __dirname) { 
    var a = 10;
    console.log(arguments.callee + "");//arguments.callee保存的是当前执行的函数对象
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

由此可知：

当node在执行模块中的代码时，它首先会在代码的最顶部添加如下代码：

```javascript
function (exports, require, module, __filename, __dirname) {
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

在代码的最底部，添加如下代码：

```
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

实际上，模块中的代码都是包装在一个函数中执行的，并且在函数执行时，同时传递了5个实参：

exports：该对象用来将对象或者函数暴露到外部,

require：函数，用来引入外部的模块,

module：用来代表的是当前模块本身，exports就是模块的属性。既可以用exports导出，也可以用module.exports导出两者指向的是同一个对象,

__filename：当前模块的完整路径,

__dirname：当前模块所在文件夹的完整路径

------

### 九、exports与modules.exports的区别：

本质上两者是相等的，但是exports只能通过“.”的方式向外暴露内部变量，而modules.exports既可以通过“.”的形式，也可以直接赋值；

```
exports.name = '孙悟空';
exports.from = "西游记"
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```javascript
module.exports = {
    name: '孙悟空',
    from: "西游记",
    sayName: function(){
        console.log(this.name);
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

### 十、包简介：

CommonJS的包规范允许我们将相关的模块组合在一起，形成一组完整的工具。

CommonJS的包规范包括包结构和包描述文件。

包结构：

package.json：必须

bin：可执行的二进制文件，非必须

lib：js文件，非必须；

doc：文档，非必须；

test：单元测试，非必须

包描述文件：

用于表达非代码相关的信息，它是一个json文件-package.json，位于包的根目录下，是包的重要组成部分。

------

### 十一、npm（Node package manager）简介：

相当于360安全卫士里的软件管家。

对于node而言，npm帮助其完成了第三方模块的发布、安装和依赖等。借助npm，node与第三方模块之间形成了很好的一个生态系统。

npm -v：查看npm版本；

npm version： 查看所有模块的版本；

npm search 包名：搜索包；

npm install/i 包名 ：安装包；

npm remove/r 包名：删除包；

npm remove/r 包名 --save：将包名在依赖中删除（node _modules中不删除）；

npm install 包名 --save：安装包并添加到依赖中（package.json的dependencies中）；

npm install：下载当前项目所依赖的包（package.json的dependencies中的包）；

npm install 包名 -g：全局安装包（全局安装的包一般是一些工具）

------

### 十二、node模块引用：

通过npm下载的包都放到node_modules中，我们通过npm下载的包直接通过包名引用即可。

node在通过模块名字来引用模块时它会首先在当前目录的node_modules中寻找是否含有该模块，如果有则直接使用，如果没有则直接去上一级目录的node_modules中寻找，如果有则直接使用，如果没有，则继续再去上一级目录中寻找，直到找到磁盘的根目录，如果依然没有，则直接报错。

------

### 十三、EventEmitter：

Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。

Node.js 里面的许多对象都会分发事件：一个 net.Server 对象会在每次有新连接时触发一个事件， 一个 fs.readStream 对象会在文件被打开的时候触发一个事件。 所有这些产生事件的对象都是 events.EventEmitter 的实例。

events 模块只提供了一个对象： events.EventEmitter。EventEmitter 的核心就是事件触发与事件监听器功能的封装。

你可以通过require("events");来访问该模块。

```javascript
// 引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象
var eventEmitter = new events.EventEmitter();
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

EventEmitter 对象如果在实例化时发生错误，会触发 error 事件。当添加新的监听器时，newListener 事件会触发，当监听器被移除时，removeListener 事件被触发。

下面我们用一个简单的例子说明 EventEmitter 的用法：

```javascript
//event.js 文件
var EventEmitter = require('events').EventEmitter; 
var event = new EventEmitter(); 
event.on('some_event', function() { 
    console.log('some_event 事件触发'); 
}); 
setTimeout(function() { 
    event.emit('some_event'); 
}, 1000); 
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

执行结果如下：

运行这段代码，1 秒后控制台输出了 'some_event 事件触发'。其原理是 event 对象注册了事件 some_event 的一个监听器，然后我们通过 setTimeout 在 1000 毫秒以后向 event 对象发送事件 some_event，此时会调用some_event 的监听器。

```javascript
$ node event.js 
some_event 事件触发
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

EventEmitter 的每个事件由一个事件名和若干个参数组成，事件名是一个字符串，通常表达一定的语义。对于每个事件，EventEmitter 支持 若干个事件监听器。

当事件触发时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递。

让我们以下面的例子解释这个过程：

```javascript
//event.js 文件
var events = require('events'); 
var emitter = new events.EventEmitter(); 
emitter.on('someEvent', function(arg1, arg2) { 
    console.log('listener1', arg1, arg2); 
}); 
emitter.on('someEvent', function(arg1, arg2) { 
    console.log('listener2', arg1, arg2); 
}); 
emitter.emit('someEvent', 'arg1 参数', 'arg2 参数'); 
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

执行以上代码，运行的结果如下：

```javascript
$ node event.js 
listener1 arg1 参数 arg2 参数
listener2 arg1 参数 arg2 参数
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 以上例子中，emitter 为事件 someEvent 注册了两个事件监听器，然后触发了 someEvent 事件。

运行结果中可以看到两个事件监听器回调函数被先后调用。 这就是EventEmitter最简单的用法。

EventEmitter 提供了多个属性，如 on 和 emit。on 函数用于绑定事件函数，emit 属性用于触发一个事件。接下来我们来具体看下 EventEmitter 的属性介绍。

**方法**

| 序号 | 方法 & 描述                                                  |
| ---- | ------------------------------------------------------------ |
| 1    | **addListener(event, listener)** 为指定事件添加一个监听器到监听器数组的尾部。 |
| 2    | **on(event, listener)** 为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数。`server.on('connection', function (stream) {  console.log('someone connected!');});` |
| 3    | **once(event, listener)** 为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器。`server.once('connection', function (stream) {  console.log('Ah, we have our first user!');});` |
| 4    | **removeListener(event, listener)**移除指定事件的某个监听器，监听器必须是该事件已经注册过的监听器。它接受两个参数，第一个是事件名称，第二个是回调函数名称。`var callback = function(stream) {  console.log('someone connected!');};server.on('connection', callback);// ...server.removeListener('connection', callback);` |
| 5    | **removeAllListeners([event])** 移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。 |
| 6    | **setMaxListeners(n)** 默认情况下， EventEmitters 如果你添加的监听器超过 10 个就会输出警告信息。 setMaxListeners 函数用于提高监听器的默认限制的数量。 |
| 7    | **listeners(event)** 返回指定事件的监听器数组。              |
| 8    | **emit(event, [arg1], [arg2], [...])** 按参数的顺序执行每个监听器，如果事件有注册监听返回 true，否则返回 false。 |

**类方法**

| 序号 | 方法 & 描述                                                  |
| ---- | ------------------------------------------------------------ |
| 1    | **listenerCount(emitter, event)** 返回指定事件的监听器数量。 |

> ```html
> events.EventEmitter.listenerCount(emitter, eventName) //已废弃，不推荐
> events.emitter.listenerCount(eventName) //推荐
> ```

 **事件**

| 序号 | 事件 & 描述                                                  |
| ---- | ------------------------------------------------------------ |
| 1    | **newListener****event** - 字符串，事件名称**listener** - 处理事件函数该事件在添加新监听器时被触发。 |
| 2    | **removeListener****event** - 字符串，事件名称**listener** - 处理事件函数从指定监听器数组中删除一个监听器。需要注意的是，此操作将会改变处于被删监听器之后的那些监听器的索引。 |

**实例**

以下实例通过 connection（连接）事件演示了 EventEmitter 类的应用。

创建 main.js 文件，代码如下：

```javascript
var events = require('events');
var eventEmitter = new events.EventEmitter();

// 监听器 #1
var listener1 = function listener1() {
   console.log('监听器 listener1 执行。');
}

// 监听器 #2
var listener2 = function listener2() {
  console.log('监听器 listener2 执行。');
}

// 绑定 connection 事件，处理函数为 listener1 
eventEmitter.addListener('connection', listener1);

// 绑定 connection 事件，处理函数为 listener2
eventEmitter.on('connection', listener2);

var eventListeners = eventEmitter.listenerCount('connection');
console.log(eventListeners + " 个监听器监听连接事件。");

// 处理 connection 事件 
eventEmitter.emit('connection');

// 移除监绑定的 listener1 函数
eventEmitter.removeListener('connection', listener1);
console.log("listener1 不再受监听。");

// 触发连接事件
eventEmitter.emit('connection');

eventListeners = eventEmitter.listenerCount('connection');
console.log(eventListeners + " 个监听器监听连接事件。");

console.log("程序执行完毕。");
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

以上代码，执行结果如下所示：

```Kotlin
$ node main.js
2 个监听器监听连接事件。
监听器 listener1 执行。
监听器 listener2 执行。
listener1 不再受监听。
监听器 listener2 执行。
1 个监听器监听连接事件。
程序执行完毕。
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 

------

###  十四、使用 GET 或 POST 请求发送数据：

server.js

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var querystring = require('querystring');

function startServer(route, handle) {
    var onRequest = function(request, response) {
        var pathname = url.parse(request.url).pathname;
        console.log('Request received ' + pathname);
        var data = [];
        request.on("error", function(err) {
            console.error(err);
        }).on("data", function(chunk) {
            data.push(chunk);
        }).on('end', function() {
            if (request.method === "POST") {
                if (data.length > 1e6) {
                    request.connection.destroy();
                }
                data = Buffer.concat(data).toString();
                route(handle, pathname, response, querystring.parse(data));
            } else {
                var params = url.parse(request.url, true).query;
                route(handle, pathname, response, params);
            }
        });
    }

    var server = http.createServer(onRequest);

    server.listen(3000, '127.0.0.1');
    console.log('Server started on localhost port 3000');
}

module.exports.startServer = startServer;
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

handler.js

```javascript
var fs = require('fs');

function home(response) {
    response.writeHead(200, { 'Content-Type': 'text/html' });
    fs.createReadStream(__dirname + '/index.html', 'utf8').pipe(response);
}

function review(response) {
    response.writeHead(200, { 'Content-Type': 'text/html' });
    fs.createReadStream(__dirname + '/review.html', 'utf8').pipe(response);
}

function api_records(response, params) {
    response.writeHead(200, { 'Content-Type': 'application/json' });
    response.end(JSON.stringify(params));
}

module.exports = {
    home: home,
    review: review,
    api_records: api_records
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 index.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>hfpp2012</title>
</head>

<body>
    <form action="/api/v1/records" method="post">
        name: <input type="text" name="name" /> age: <input type="text" name="age" />
        <input type="submit" value="Submit">
    </form>
</body>

</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

### 十五、常用工具：

```javascript
var events = require('events');
var util = require('util');

var Person = function(name) {
    this.name = name
}

util.inherits(Person, events.EventEmitter);

var xiaoming = new Person('xiaoming');
var lili = new Person('lili');
var lucy = new Person('lucy');

var person = [xiaoming, lili, lucy];

person.forEach(function(person) {
    person.on('speak', function(message) {
        console.log(person.name + " said: " + message);
    })
})

xiaoming.emit('speak', 'hi');
lucy.emit('speak', 'I want a curry');

// var myEmitter = new events.EventEmitter();

// myEmitter.on('someEvent', function(message) {
//     console.log(message);
// })

// myEmitter.emit('someEvent', 'the event was emitted');
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

### 十六、Buffer缓存区：

Buffer的结构和数组很像，操作的方法也和数组类似；

数组中不能存储二进制的文件，而Buffer就是专门用来存储二进制数据；

为什么要用Buffer？Node.js其实就做两件事，一是接收请求，另一个是发送请求，请求都是以二进制的方式传递的，接收以后或者发送之前二进制数据都是放在缓存中。

使用Buffer不需要引用模块，直接使用即可；

在Buffer中存储的是二进制数据，但在显示时是以十六进制的形式显示；存入时的数字可以是任何进制，但在控制台或者页面就是一定只能以十进制输出显示，如果存储的是字符的话可以用其它进制显示，方式str.toString(x)；

```javascript
var str = "Hello Atguigu";
var buf = Buffer.from(str);
console.log(buf);//<Buffer 48 65 6c 6c 6f 20 41 74 67 75 69 67 75>
var buff = Buffer.alloc(10);
buff[0] = 123;
console.log(buff[0]);//123
console.log(buff[0].toString(2));//1111011
buff[1] = 0xaa;
console.log(buff[1]);//170
console.log(buff[1].toString(16));//aa
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

Buffer中每一个元素的范围是00-ff（16进制） <=> 0-255（10进制） <=> 00000000-11111111（2进制）；

计算机中1个0或者1个1我们称之为1位（bit），计算机中数据是以字节为单位传输的；

8bit=1byte（1字节）；

1024byte=1kb；

1024kb=1mb；

1024mb=1gb；

1024gb=1tb；

Buffer中的1个元素占用内存的1个字节；

Buffer的大小一旦确定，则不能修改，Buffer实际上是对底层内存的直接操作；

```javascript
var str = "Hello Atguigu";
var buf = Buffer.from(str);
console.log(buf);
console.log(buf.length);//13，占用内从大小
console.log(str.length);//13，字符长度

var str2 = "Hello 尚硅谷";
var buf2 = Buffer.from(str2);
console.log(buf2);
console.log(buf2.length);//15，占用内从大小，1个汉字占用3个字节
console.log(str2.length);//9，字符长度
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```javascript
Buffer.from(str)//将一个字符串转换为Buffer
Buffer.alloc(size)//创建一个指定大小的Buffer
Buffer.allocUnsafe(size)//创建一个指定大小的Buffer，但是可能包含敏感数据
buf.toString()//将缓存区中的数据转换为字符串
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

###  十七、路由：

server.js

```javascript
var http = require('http');
var fs = require('fs');

function startServer() {
    var onRequest = function(request, response) {
        console.log('Request received ' + request.url);
        if (request.url === '/' || request.url === '/home') {
            response.writeHead(200, { 'Content-Type': 'text/html' });
            fs.createReadStream(__dirname + '/index.html', 'utf8').pipe(response);
        } else if (request.url === '/review') {
            response.writeHead(200, { 'Content-Type': 'text/html' });
            fs.createReadStream(__dirname + '/review.html', 'utf8').pipe(response);
        } else if (request.url === '/api/v1/records') {
            response.writeHead(200, { 'Content-Type': 'application/json' });
            var jsonObj = {
                name: "hfpp2012"
            };
            response.end(JSON.stringify(jsonObj));
        } else {
            response.writeHead(404, { 'Content-Type': 'text/html' });
            fs.createReadStream(__dirname + '/404.html', 'utf8').pipe(response);
        }
    }

    var server = http.createServer(onRequest);

    server.listen(3000, '127.0.0.1');
    console.log('Server started on localhost port 3000');
}

exports.startServer = startServer;
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

review.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    review page
</body>

</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 404.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    404 error page
</body>

</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

重构路由代码：
app.js

```javascript
var server = require('./server');
var router = require('./router');
var handler = require('./handler');

var handle = {};
handle["/"] = handler.home;
handle['/home'] = handler.home;
handle['/review'] = handler.review;
handle['/api/v1/records'] = handler.api_records;

server.startServer(router.route, handle);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

server.js

```javascript
var http = require('http');
var fs = require('fs');

function startServer(route, handle) {
    var onRequest = function(request, response) {
        console.log('Request received ' + request.url);
        route(handle, request.url, response);
    }

    var server = http.createServer(onRequest);

    server.listen(3000, '127.0.0.1');
    console.log('Server started on localhost port 3000');
}

module.exports.startServer = startServer;
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 router.js

```javascript
var fs = require('fs');

function route(handle, pathname, response) {
    console.log('Routing a request for ' + pathname);
    if (typeof handle[pathname] === 'function') {
        handle[pathname](response);
    } else {
        response.writeHead(404, { 'Content-Type': 'text/html' });
        fs.createReadStream(__dirname + '/404.html', 'utf8').pipe(response);
    }
}

module.exports.route = route;
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

handler.js

```javascript
var fs = require('fs');

function home(response) {
    response.writeHead(200, { 'Content-Type': 'text/html' });
    fs.createReadStream(__dirname + '/index.html', 'utf8').pipe(response);
}

function review(response) {
    response.writeHead(200, { 'Content-Type': 'text/html' });
    fs.createReadStream(__dirname + '/review.html', 'utf8').pipe(response);
}

function api_records(response) {
    response.writeHead(200, { 'Content-Type': 'application/json' });
    var jsonObj = {
        name: "hfpp2012"
    };
    response.end(JSON.stringify(jsonObj));
}

module.exports = {
    home: home,
    review: review,
    api_records: api_records
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

###  十八、文件系统（fs）：

文件系统简单来说就是通过Node.js来操作系统中的文件；

使用文件系统首先需要引入fs模块，fs是核心模块，直接引入不需要下载；

文件的写入步骤：

手动步骤：

a、打开文件；

b、向文件中写入内容；

c、保存并关闭文件；

1、同步写入文件：

```javascript
var fs = require('fs');

var fd = fs.openSync('./input.txt', 'w');
fs.writeSync(fd, '今天天气真不错！');
fs.closeSync(fd);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

2、异步写入文件：

```javascript
var fs = require('fs');

//打开文件
fs.open('./input.txt', 'w', function(err, fd){
    if(!err){
        //写入文件
        fs.write(fd, '这是异步写入的内容！', function(err){
            if(!err){
                console.log('写入成功！！！');
            }else{
                console.log(err);
            }

            //关闭文件
            fs.close(fd, function(err){
                if(!err){
                    console.log("文件已经关闭！");
                }
            });
        });
    }else{
        console.log(err);
    }
});
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

3、简单文件写入（不需要打开文件）：

```javascript
var fs = require('fs');
fs.writeFile('./input.txt', '这是通过writeFile写入的内容', function(err){
    if(!err){
        console.log();
    }
});
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

| Flag | 描述                                                   |
| ---- | ------------------------------------------------------ |
| r    | 以读取模式打开文件。如果文件不存在抛出异常。           |
| r+   | 以读写模式打开文件。如果文件不存在抛出异常。           |
| rs   | 以同步的方式读取文件。                                 |
| rs+  | 以同步的方式读取和写入文件。                           |
| w    | 以写入模式打开文件，如果文件不存在则创建。             |
| wx   | 类似 'w'，但是如果文件路径存在，则文件写入失败。       |
| w+   | 以读写模式打开文件，如果文件不存在则创建。             |
| wx+  | 类似 'w+'， 但是如果文件路径存在，则文件读写失败。     |
| a    | 以追加模式打开文件，如果文件不存在则创建。             |
| ax   | 类似 'a'， 但是如果文件路径存在，则文件追加失败。      |
| a+   | 以读取追加模式打开文件，如果文件不存在则创建。         |
| ax+  | 类似 'a+'， 但是如果文件路径存在，则文件读取追加失败。 |

4、使用绝对路径写入文件：

```javascript
var fs = require('fs');
//或者C:/Users/Administrator/Desktop/input.txt
fs.writeFile('C:\\Users\\Administrator\\Desktop\\input.txt', '这是通过writeFile写入的内容', {flag: 'a'},  function(err){
    if(!err){
        console.log();
    }
});
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

5、流式文件写入：

同步、异步、简单文件的写入都不太适合大文件的写入，性能较差，容易导致内存溢出。

```javascript
var fs = require('fs');
var ws = fs.createWriteStream('./input.txt', );
ws.once('open', function(){
    console.log('流打开了~~~');
});
ws.once('close', function(){
    console.log('流关闭了~~~');
});
ws.write('通过可写流写入的内容');
ws.write('锄禾日当午，');
ws.write('汗滴禾下土，');
ws.write('谁知盘中餐，');
ws.write('粒粒皆辛苦！');

ws.end();
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

5、简单文件读取：

```javascript
var fs = require('fs');

fs.readFile('H:\\我的图片\\0064wDqKgy1fujq6czvkij30j60nz0uu.jpg', function(err, data){
    if(!err){
        //console.log(data.toString());//因为读取到的可能不止文本文件，所以返回的是二进制
        fs.writeFile('./hello.jpg', data, function(err){
            if(!err){
                console.log('文件 写入成功');
            }
        });
    }
});
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

6、流式文件读取：

```javascript
/*
	流式文件读取也适用于一些比较大的文件，可以分多次将文件读取到内存中
 */

var fs = require("fs");

//创建一个可读流
var rs = fs.createReadStream("C:/Users/lilichao/Desktop/笔记.mp3");
//创建一个可写流
var ws = fs.createWriteStream("a.mp3");

//监听流的开启和关闭
rs.once("open",function () {
	console.log("可读流打开了~~");
});

rs.once("close",function () {
	console.log("可读流关闭了~~");
	//数据读取完毕，关闭可写流

	ws.end();
});

ws.once("open",function () {
	console.log("可写流打开了~~");
});

ws.once("close",function () {
	console.log("可写流关闭了~~");
});

//如果要读取一个可读流中的数据，必须要为可读流绑定一个data事件，data事件绑定完毕，它会自动开始读取数据
rs.on("data", function (data) {
	//console.log(data);
	//将读取到的数据写入到可写流中
	ws.write(data);
});
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```javascript
//流式文件读取一般适用于比较大的文件，可以分多次将文件读取到内存中
var fs = require('fs');

//创建一个可读流
var rs = fs.createReadStream('H:\\我的图片\\0064wDqKgy1fujq6czvkij30j60nz0uu.jpg');
//创建一个可写流
var ws = fs.createWriteStream('./node.jpg');

//监听流的开启和关闭
rs.once('open', function(){
    console.log('可读流打开了！');
});

rs.once('close', function(){
    console.log('可读流关闭了！');
});

//监听流的开启和关闭
ws.once('open', function(){
    console.log('可写流打开了！');
});

ws.once('close', function(){
    console.log('可写流关闭了！');
});

//pipe()可将可读流中的内容直接输出到可写流
rs.pipe(ws);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

7、fs其它操作：

• 验证路径是否存在

– fs.existsSync(path)

• 获取文件信息

– fs.stat(path, callback)

– fs.statSync(path)

• 删除文件

– fs.unlink(path, callback)

– fs.unlinkSync(path)

列出文件

– fs.readdir(path[, options], callback)

– fs.readdirSync(path[, options])

• 截断文件

– fs.truncate(path, len, callback)

– fs.truncateSync(path, len)

• 建立目录

– fs.mkdir(path[, mode], callback)

– fs.mkdirSync(path[, mode])

删除目录

– fs.rmdir(path, callback)

– fs.rmdirSync(path)

• 重命名文件和目录

– fs.rename(oldPath, newPath, callback)

– fs.renameSync(oldPath, newPath)

• 监视文件更改写入

– fs.watchFile(filename[, options], listener)

------

### 十九、WEB模块（WEB服务器）：

```javascript
var http = require('http');
http.createServer(function(req, res){
    console.log('Request received!');
    res.writeHead(200, {'Content-Type' : 'text/plain'});
    res.write('<html>');
    res.write('<body>');
    res.write('<h1>Hello, World!</h1>');
    res.write('</body>');
    res.write('</html>');
    res.end();
}).listen(3000, '127.0.0.1');

console.log('Server started on localhost port 3000');
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

 响应 json：

```javascript
var http = require('http');
http.createServer(function(req, res){
    console.log('Request received!');
    res.writeHead(200, { 'Content-Type': 'application/json' });
    // response.write('Hello from out application');
    var myObj = {
        name: "hfpp2012",
        job: "programmer",
        age: 27
    };
    res.end(JSON.stringify(myObj));//将json转换为字符串，便于传输；JSON.parse()：反序列化，将转换后的字符串转换回json
}).listen(3000, '127.0.0.1');

console.log('Server started on localhost port 3000');
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

响应HTML页面 ：

main.js：

```javascript
var http = require('http');
var fs = require('fs');

http.createServer(function(req, res){
    console.log('Request received');
    res.writeHead(200, { 'Content-Type': 'text/html' });
    var myReadStream = fs.createReadStream(__dirname + '/index.html', 'utf8');
    // res.write('Hello from out application');
    myReadStream.pipe(res);
}).listen(3000, '127.0.0.1');

console.log('Server started on localhost port 3000');
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

index.html：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>hfpp2012</title>
</head>

<body>
hello wolrd
</body>

</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

###  二十、express框架：

1、什么是express？

express是也一个基于node.js的极简、灵活的web开发框架。可以实现非常强大的web服务器功能。

2、express的特点：

可以设置中间件响应或过滤http请求；

可以使用路由实现动态网页，响应不同的http请求；

内置支持ejs模板（默认是jade模板）实现模板渲染生成html；

3、express-generator生成器：

express-generator是express官方团队为开发者准备的一个快速生成工具，可以非常快速的生成一个基本的express开发框架。

4、express安装与使用：

1）安装express-generator生成器；

```javascript
cnpm i exrpess-generator
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

安装完成后可以使用express命令。

2）创建项目：

```
express -e 项目名称 //自动创建项目名称
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```
express -e //手动创建项目名称
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

3）安装依赖：

```
cnpm i
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

4）开启项目

```javascript
node app
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```javascript
node start //自动查找当前目录下的package.json文件，找到start对应的命令进行进行执行
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```javascript
node ./bin/www
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

5）测试项目：

打开浏览器输入localhost

------

### 二十一、Node.js 连接 MySQL：

安装驱动：

```javascript
cnpm install mysql
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

注意：cnpm install mysql是安装nodejs的mysql模块而不是安装mysql数据库，你的应用通过这个驱动程序连接并操作mysql中的库和表。

如果你的电脑中没有mysql，则请先安装，相关教程如下：

<https://jingyan.baidu.com/article/363872ec2e27076e4ba16fc3.html>

在进行数据库操作前，你需要将以下SQL 文件导入到你的 MySQL 数据库中。

```sql

SET NAMES utf8;
SET FOREIGN_KEY_CHECKS = 0;

DROP TABLE IF EXISTS `websites`;
CREATE TABLE `websites` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` char(20) NOT NULL DEFAULT '' COMMENT '绔欑偣鍚嶇О',
  `url` varchar(255) NOT NULL DEFAULT '',
  `alexa` int(11) NOT NULL DEFAULT '0' COMMENT 'Alexa 鎺掑悕',
  `country` char(10) NOT NULL DEFAULT '' COMMENT '鍥藉',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

BEGIN;
INSERT INTO `websites` VALUES ('1', 'Google', 'https://www.google.cm/', '1', 'USA'), ('2', '娣樺疂', 'https://www.taobao.com/', '13', 'CN'), ('3', '鑿滈笩鏁欑▼', 'http://www.runoob.com/', '4689', 'CN'), ('4', '寰崥', 'http://weibo.com/', '20', 'CN'), ('5', 'Facebook', 'https://www.facebook.com/', '3', 'USA');
COMMIT;

SET FOREIGN_KEY_CHECKS = 1;
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 连接数据库

```javascript
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'root',
  password : '123456',
  database : 'test'
});
 
connection.connect();
 
connection.query('SELECT 1 + 1 AS solution', function (error, results, fields) {
  if (error) throw error;
  console.log('The solution is: ', results[0].solution);
});
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 数据库操作( CURD )

查询数据

将上面我们提供的 SQL 文件导入数据库后，执行以下代码即可查询出数据：

```javascript
var mysql  = require('mysql');  
 
var connection = mysql.createConnection({     
  host     : 'localhost',       
  user     : 'root',              
  password : '123456',       
  port: '3306',                   
  database: 'test', 
}); 
 
connection.connect();
 
var  sql = 'SELECT * FROM websites';
//查
connection.query(sql,function (err, result) {
        if(err){
          console.log('[SELECT ERROR] - ',err.message);
          return;
        }
 
       console.log('--------------------------SELECT----------------------------');
       console.log(result);
       console.log('------------------------------------------------------------\n\n');  
});
 
connection.end();
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

插入数据

```javascript
var mysql  = require('mysql');  
 
var connection = mysql.createConnection({     
  host     : 'localhost',       
  user     : 'root',              
  password : '123456',       
  port: '3306',                   
  database: 'test', 
}); 
 
connection.connect();
 
var  addSql = 'INSERT INTO websites(Id,name,url,alexa,country) VALUES(0,?,?,?,?)';
var  addSqlParams = ['菜鸟工具', 'https://c.runoob.com','23453', 'CN'];
//增
connection.query(addSql,addSqlParams,function (err, result) {
        if(err){
         console.log('[INSERT ERROR] - ',err.message);
         return;
        }        
 
       console.log('--------------------------INSERT----------------------------');
       //console.log('INSERT ID:',result.insertId);        
       console.log('INSERT ID:',result);        
       console.log('-----------------------------------------------------------------\n\n');  
});
 
connection.end();
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

更新数据

```javascript
var mysql  = require('mysql');  
 
var connection = mysql.createConnection({     
  host     : 'localhost',       
  user     : 'root',              
  password : '123456',       
  port: '3306',                   
  database: 'test', 
}); 
 
connection.connect();
 
var modSql = 'UPDATE websites SET name = ?,url = ? WHERE Id = ?';
var modSqlParams = ['菜鸟移动站', 'https://m.runoob.com',6];
//改
connection.query(modSql,modSqlParams,function (err, result) {
   if(err){
         console.log('[UPDATE ERROR] - ',err.message);
         return;
   }        
  console.log('--------------------------UPDATE----------------------------');
  console.log('UPDATE affectedRows',result.affectedRows);
  console.log('-----------------------------------------------------------------\n\n');
});
 
connection.end();
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

删除数据

```javascript
var mysql  = require('mysql');  
 
var connection = mysql.createConnection({     
  host     : 'localhost',       
  user     : 'root',              
  password : '123456',       
  port: '3306',                   
  database: 'test', 
}); 
 
connection.connect();
 
var delSql = 'DELETE FROM websites where id=6';
//删
connection.query(delSql,function (err, result) {
        if(err){
          console.log('[DELETE ERROR] - ',err.message);
          return;
        }        
 
       console.log('--------------------------DELETE----------------------------');
       console.log('DELETE affectedRows',result.affectedRows);
       console.log('-----------------------------------------------------------------\n\n');  
});
 
connection.end();
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 二十二、其它工具：

nodemon：

```
npm install -g nodemon
```

在编写调试Node.js项目，修改代码后，需要频繁的手动close掉，然后再重新启动，非常繁琐。现在，我们可以使用`nodemon`这个工具，它的作用是监听代码文件的变动，当代码改变之后，自动重启。