**HTML&CSS：**

对Web标准的理解、浏览器内核差异、兼容性、hack、CSS基本功：布局、盒子模型、选择器优先级及使用、HTML5、CSS3、移动端适用。

**JavaScript：**

数据类型、面向对象、继承、闭包、插件、作用域、跨域、原型链、模块化、自定义事件、内存泄漏、事件机制、异步装载回调、模板引擎、Node.js、JSON、AJAX等。

**其他：**

HTTP、安全、正则、优化、重构、响应式、移动端、团队协作、可维护、SEO、UED、构架。



**1.什么是盒子模型？**

​    内容(content)，元素的内边距(padding)，元素的外边距(margin)，元素的边框(border)，四个部分

一起构成了CSS中元素的盒模型。

**2.行内元素有哪些？块级元素有哪些？**

行内元素：a、b、span、img、input、strong、select、label、em、button、textare

块级元素：div、ul、li、dl、dt、dd、p、h1-h6、blockquote

**3.简述****src****和****href****的区别？**

href是指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，用于超链接。

src是指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置，例如：javascript脚本，img图片，frame等元素。

**4.简述同步和异步的区别？**

同步是阻塞模式，异步是非阻塞模式。

**5.简述****px****和****em****的区别？**

px和em都是长度单位，区别是px的值是固定的，指定多少就是多少。em的值是不固定的，并且em会继承父级元素的字体大小。

**6.浏览器的内核分别是什么？**

IE：trident内核、ie内核

Firefox：gecko内核、firefox内核

Safari：webkit内核

Opera：新：blink内核 旧：presto内核

Chrome：blink内核、webkit内核、chromium内核

**7.如何添加、移除、移动、复制、创建和查找节点？**

a.创建节点：createDocumentFragment() //创建一个DOM片段

​                createElement() //创建一个具体元素

​                createTextNode() //创建一个文本节点

b.添加、移除、移动、复制

​                appendChild() //添加

  removeChild() //移除

​                replaceChild() //替换

​                insertBefore() //插入

c.查找

​                getElementsByTagName() //通过标签名称

 getElementsByName() //通过元素的Name属性的值

​               getElementsByID() //通过元素ID是唯一性

**8.JavaScript中****callee****和****caller****的作用？**

​       caller是返回一个对函数的引用，该函数调用了当前函数。

callee是返回正在被执行的function函数，也就是所指定的function对象的正文。

**9.请描述一下****cookies****，****sessionStorage****和****localStorage****的区别？**

​    sessionStorage：不是持久化的本地存储，存储的数据在浏览器关闭之后自动删除。

localStorage：用于持久化的本地存储，浏览器关闭后数据不会丢失。

Cookies：大小是受限制，需要指定作用域，不可以跨域调用，与服务器进行交互。

**10.比较****typeof****与****instanceof****？**

Typeof：返回值是一个字符串，用来说明变量的数据类型。

Instanceof：用于判断一个变量是否属于某个对象的实例。

相同点：JavaScripet中typeof和instanceof常用来判断一个变量是否为空，或是什么类型。

**11.如何理解闭包？**

闭包就是能够读取其他函数内部变量的函数。

**12.jQuery库中的****$()****是什么？**

$()函数是jQuery函数的别称，$()函数用于将任何对象包裹成jQuery对象。

**13.$(this)和****this****关键字在****jQuery****中有何不同？**

$(this)返回一个jQuery对象，可以对它调用多个jQuery方法。

this代表当前元素，它是javascript关键词中的一个，表示上下文中的当前DOM元素。

**14.jQuery中****$.get()****提交和****$.post()****提交有区别吗？**

相同点：都是异步请求的方式来获取服务器端的数据。

异同点：请求方式不同，参数传递方式不同，数据传输大小不同。

安全性：GET方式请求的数据会被浏览器缓存起来，因此有安全问题，POST方式请求的数据相对安全一些。

**15.写出一个简单的****$.ajax()****的请求方式？**

$.ajax({

url:”http://www.baidu.com”,

type:”POST”,

data:data,

cache:true,

headers:{},



beforeSend:function(){},

​       successs:function(){},



error:function(){},

Complete:function(){}

  });

**16.简述一下什么是****Ajax****？**

AJAX=异步JavaScript和XML，AJAX是一种用于创建快速动态网页的技术，可以使网页实现异步更新。

**17.请说出三种减低页面加载时间的方法？**

压缩CSS、JavaScript的文件。

合并JavaScript、CSS文件，减少HTTP请求。

外部JavaScript放在代码页底部、CSS放到代码页顶部。

**18.如何提高页面性能优化？**

减少HTTP请求，减少DOM元素数量，使Ajax可缓存，代码重复利用。

**19.CSS引入的方式有哪些？****link****和****@import****的区别是？**

方式：内联 内嵌 外链 导入

区别：同时加载，前者无兼容性，后者CSS2.1以下的浏览器不支持，link支持使用JavaScript改变样式，后者不可。

**20.前端页面有哪三层构成，分别是什么？**

​      结构层：HTML

​       表示层：CSS

​       行为层：JavaScript

**21.CSS的基本语句构成是什么？**

​    选择器{属性1:值1;属性2:值2;}

**22.标签上****title****和****alt****属性的区别是什么？**

​    alt当图片不显示是用文字代表。

  title为该属性提供信息。

**23.什么是语义化的****HTML****？**

​    让页面的内容结构化，便于对浏览器搜索引擎解析。

**24.清除浮动的几种方式？**

​    clear:both   overflow:auto

**25.JavaScript的****typeof****返回哪些数据类型？**

​    object number function boolean nuderfind

**26.call和****apply****的区别？**

​    Object.call(this,obj1, obj2,obj3)

​    Object.apply(this,arguments)

**27.JSON和****XML****的区别？**

​    可读性：基本相同，XML的可读性比较好。

​    可扩展性：都具有良好的扩展性。

​    编码难度：相对而言，JSON的编码比较容易。

​    解码难度：JSON解码难度基本为0，XML需要考虑子节点和父节点。

​    数据体积：JSON相对于XML来讲，数据体积小，传递的速度比较快。

​    数据交互：JSON与JavaScript的交互更加方便，更容易解析处理，更好的数据交互。

​    数据描述：XML对数据描述性比较好。

​    传输速度：JSON的速度远远快于XML。

**28.JavaScript是什么？****JavaScript****和****HTML****的开发如何结合？**

​    JavaScript是一种基于对象和事件驱动，并具有安全性的脚本语言。

​    可以在HTML的三个地方编写JavaScript脚本语言：

a. 在网页文件的标签对其中直接编写脚本程序代码。

b.将脚本程序代码放置在单独的一个文件夹中，在网页文件中引入这个脚本程序语言。

c. 将脚本程序代码作为某个元素的事件属性值或超链接的href属性值。

**29.常使用的库有哪些？**

​    jQuey-1.4.2.min.js

**30.什么是****HTML****本地存储？**

​    通过本地存储，Web应用程序能够在用户浏览器中对数据进行本地的存储。

**31.什么是****ES6****？**

​    ECMAScript6是最新版本JavaScript语言的标准。

**32.什么是****Flex****布局？**

​    弹性布局，用来为盒子模型提供最大的灵活性。

**33.为什么要使用****DIV+CSS****来布局？**

​    形式与内容分离，大大减少页面代码，提高页面浏览速度，结构清晰有利于SEO（搜索引擎优化），缩短改版时间，布局更加方便，一次设计多次使用。

**34.常见的****HTTP****状态码有哪些？**

​    200（成功）服务器成功处理了请求。

​    400（错误请求） 服务器不理解请求的语法。

​    403（禁止）服务拒绝请求。

​    404（未找到）服务器找不到请求的网页。

​    505（HTTP版本不支持）服务器不支持请求中所用的HTTP协议版本。

**35.浏览器缓存有哪些？**

​    http缓存、flash缓存、WebSQL、Cookie、localstorage、sessionstorage

**36.Bootstrap框架响应式实现的原理是什么？**

​    百分比布局+媒体查询

**37.$.fn是什么意思？**

​    $.fn是指jQuery的命名空间，加上fn上的方法及属性，会对jQuery实例每一个有效。

**38.CSS3有哪些新增的属性？**

​    边框、背景、渐变、阴影、2D转换、3D转换等。

​    例如：边框（border-radius、border-shadow、border-image）等。

**39.HTML5的定义？有哪些新特性？**

​    HTML5是定义HTML标准的最新版本，它是一个新版本的HTML语言，具有新元素、属性、行为。

​    特性：绘画canvas，媒介回放video和audio元素，语义化更好的元素article、footer、header、nav、section，表单控件calendar、date、time、email、url、search。

**40.如何处理****HTML5****新标签的浏览器兼容问题？**

​    IE8/IE7/IE6支持通过document.createElement方法产生的标签，可以利用这一特性让这些浏览器支持HTML5标签，浏览器支持新标签后，还需要添加标签默认的样式。

​    直接使用成熟的框架：

​                     <!--[if lt IE 9]>

                        <script src=”html5shiv.js”></script>

​                     <![endif]>



**41.如何区别****HTML****和****HTML5****？**

​    1.DOCTYPE声明 2.新增元素及特性







**42.讲讲****position****的值和****relative****和****absolute****的区别？**

​    absolute：生成绝对定位的元素，相对于值不为static的第一个父元素进行定位。

​    relative：生成相对定位的元素，相对于其正常位置进行定位。

**43.CSS中的选择器是什么？**

​    选择器是你想要应用一个样式的时候，帮助你去选择元素。

**44.本地存储的生命周期是什么？**

​    本地存储没有生命周期，它将保留知道用户从浏览器清除或者使用JavaScript代码移除。

**45.HTML5的页面结构是什么？**

​    <!DOCTYPEhtml>

​    <html>

​    <head>

​              <title></title>

​    </head>

​    <body></body>

​    </html>

**46.CSS隐藏元素有哪些？**

a.  display:none;

b.  visibility:hidden;

c.  opacity:0;

d.  z-index

**47.jQuery有几种选择器？**

​    基本选择器，层次选择器，基本过滤选择器，内容过滤选择器，可见性过滤选择器，属性过滤选择器，子元素过滤选择器，表单选择器，表单过滤选择器。

**48.什么叫优雅降级和渐进增强？**

​    渐进增强：针对低版本浏览器进行构建页面保证最基本的功能，在针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。

​    优雅降级：一开始就构建完整的功能，在针对低版本浏览器进行兼容。

**49.线程和进程的区别？**

​    一个程序至少有一个进程，一个进程至少有一个线程。线程的划分尺度小于进程，使得多线程程序并发性高从逻辑高度上看多线程的意义在于一个应用程序中有多个执行部分可以同时执行。

**50.浏览器是如何渲染页面的？**

\1. 解析HTML文件，创建DOM树。

\2. 解析CSS。

\3. 将CSS与DOM合并，构建渲染树。

\4. 布局和绘制，重绘（repaint）和重排（reflow）。

**51.什么跨域？**

​    由于浏览器同源策略，凡是发送请求url的协议、端口、域名三者之间任意一与当前页面地址不同即为跨域。

**52.data-属性的作用是什么？**

​    Data-*属性用于存储页面或应用程序的私有自定义数据。

​    Data-*赋予我们在所有HTML元素上嵌入自定义。

**53.JavaScript中的定时器有哪些？区别及用法是什么？**

\1. settimeout 执行一次

\2. setinterval 重复执行

**54.document.write和****innerHTML****的区别？**

\1. Document.write是直接写入到页面的内容流，如果在写之前没有调用document.open，浏览器会自动调用open。

\2. InnerHTML是DOM页面元素的一个属性，代表该元素的html内容。

**55.document.onload和****document.ready****两个事件的区别？**

​    页面加载完成有两种事件，一是ready，表示文档结构已经加载完成。二是onload，指示页面包含图片等文件在内的所有元素都加载完成。

**56.请说出你可以传递给****jQuery****方法的四种不同值？**

​    选择器，HTML，回调函数，HTML元素，对象，数组，元素数组，jQuery对象。

**57.JavaScript有几种数据类型？**

​    五种基本数据类型：Undefined、Null、Boolean、Number、String。

**58.HTTP和****HTTPS****有什么区别？**

​    HTTP是HTTP协议运行在TCP之上。HTTPS是HTTP运行在SSL/TLS之上。

**59.什么是****MVC****模式？**

​    MVC（Model-View-Controller）

​    MVC是比较直观的构架模式，用户操作->View（负责接收用户的输入操作）->Controller（业务逻辑处理）->Model（数据持久化）->View（将结果反馈给View）。

**60.什么是****B/S****结构？**

​    B/S（Browser/Server）结构即浏览器和服务器结构。

​    B/S体系结构的功能组成：浏览器、Web服务器、数据库服务器。

**61.什么是****jQuery****？为什么要使用****jQuery****？**

​    jQuery是JavaScript和Query（查询），即是辅助JavaScript开发的库。

​    因为jQuery是轻量级的框架，大小不到30kb，它有强大的选择器，出色的DOM操作的封装，有可靠的事件处理机制。

**62.什么是****Django****？**

​    Django是一个开放源代码的Web应用框架，Python写成。采用了MVC的框架模式，模型层M，视图V，控制器C。

**63.什么是****Express****？**

​    Express是一个简洁而灵活的node.js Web应用框架，提供一系列强大特性帮助你创建各种Web应用。

**64.什么是****Bootstrap****？**

​    Bootstrap是一个用于快速开发Web应用程序和网站的前端框架。Bootstrap是基于HTML、CSS、JAVASCRIPT的。

**65.什么是****Flask****？**

​    Flask是一个使用Python编写的轻量级Web应用框架。其中WSGI工具箱采用Werkzeug，模板引擎使用Jinja2。

**66.什么是****WSGI****协议？**

​    WSGI（Web Server Gateway Interface）：Web服务器网关接口。

​    作用：可以很好使Web框架与Web服务器进行分离。也就是说服务器只管与客户端链接，而具体的业务逻辑代码由框架来完成。

**67.MySQL数据库中主键、超键、候选键、外键是什么？**

\1. 主键：用户选作元组标识的一个候选键程序主键。

\2. 超键：在关系中能唯一标识元组的属性集称为关系模式的超键。

\3. 候选键：不含有多余属性的超键称为候选键。

\4. 外键：如果关系模式R1中的某属性集不是R1的主键，而是另一个关系R2的主键则该属性集是关系模式R1的外键。

**68.MySQL数据库的事务？**

​    数据库事务transanction正确执行的四个基本要素：ACID。

​    原子性（Atomicity）/一致性（Correspondence）/隔离性（lsolation）/持久性（Durability）

**69.MySQL数据库****drop****，****delete****，****truncate****的区别？**

​    drop直接删除表；truncate删除表中数据，再次插入时自增长ID又从1开始；delete删除表中数据，可以加where语句。

**70.什么是****NoSQL****数据库？为什么要使用和不使用****NoSQL****数据库？**

​    NoSQL数据库是非关系型数据库，NoSQL=Not Only SQL。

​    关系型数据库采用的结构化的数据，NoSQL采用的是键值对的方式存储数据。

**71.MySQL数据库和****MongoDB****数据库最基本的差别是什么？**

​    MySQL和MongoDB两者都是免费开源的数据库。MySQL和MongoDB有许多基本差别，包括数据的表示，查询，关系，事务，schema的设计的定义，标准化，速度，性能。即数据存储结构的不同。

**72.MongoDB成为最好****NoSQL****数据库的原因是什么？**

\1. 面向文件 2.高性能 3.高可用性 4.易扩展性 5.丰富的查询语言

**73.什么是****Liunx****操作系统？它的优点是什么？**

​    Liunx是一个操作服务系统，通常用来代表Linux系列的操作系统名称，liunx主要应用于网络服务器，科学运算，软件开发，嵌入式系统。

​    1.稳定 2.免费/少许费用 3.安全性、漏洞快速修补 4.多任务、多用户 5.用户和用户组的规划 6.相对不消耗系统资源

**74.什么是****Apache****服务器？**

​    Apache是一种开放源码的HTTP服务器，可以在大多数计算机操作系统中运行，由于其多平台和安全性被广泛使用，是最流行的Web服务器端软件之一。它快速、可靠并且可通过简单的API扩展，Python/Perl等解释器可被编译到服务器中。

**什么是****XAMPP****？**

​    XAMPP（Apache+MySQL+PHP+PERL）是一个强大的建站集成软件包。

**75.什么是****Xshell****？**

​    Xshell是一款功能强大且安全的终端模拟器，支持SSH、SFTP、TELNET、RLOGIN、SERIAL。

**76.什么是****Ftp****？**

​    FTP是TCP/IP网络上两台计算机传送文件的协议。

**77.什么是****SVN****？**

​    SVN是Subversion的简称，是一个开放源代码的版本控制系统。运行方式： 独立服务器、基于Apache。

**78.Python如何生成随机数？**

​    Import random

**79.简述****Python****代码的运行机制？**

\1. 把原始代码编译成字节码。

\2. 把编译好的字节码转换成发到Python虚拟机（PVM）中执行。

**80.Python中的****yield****是什么？**

​    yield意思是生产，返回了一个生产器对象，每个生成器对象只能使用一次。



**81.简述****Python3****中的****inpurt()****函数的理解？**                    

​    input()获取用户输入，无论用户输入什么，获取到的都是字符串类型。







**82.Python在****except****中****return****后还会不会执行****finally****中的代码？怎么抛出自定义异常？**

​    会继续处理finally中的代码，用raise方法可以抛出自定义异常。

**83.Python参数的传递有？**

​    1.位置参数 2.默认参数 3.可变参数 4.关键字参数

**84.****Python中****lambda****函数是什么？**

​    lambda是匿名函数，用法：lambda arg1,arg2...argN:expression using args

**84.Python是如何进行内存管理的？**

​    1.对象的引用计数机制 2.垃圾回收 3.内存池机制

**85.** **按升序合并如下两个****list****，并去除重复的元素？**

​    题干：list1=[2,3,8,4,9,5,7]

​          list2=[2,10,6,7,8,12]

​    答案：list3=list1+list2

​          print set(list3)

**85.Python中装饰器的作用和功能？**

\1. 引入日志 2.函数执行时间统计 3.执行函数前预备处理 4.执行函数后清理功能 5.权限校验 6.缓存

**86.常用的****Linux****命令有哪些？**

​    ls,help,cd,more,clear,mkdir,pwd,rm,grep,find,mv,su,date

**87.跨域请求问题****Django****怎么解决的？**

\1. 启用中间件 2.POST请求 3.验证码 4.表单添加{%crsf_token%}标签

**88.Django重定向你是如何实现的？用的什么状态码？**

​    使用HttpResponseRedirect、redire、reverse 状态码：302，301

**89.你用过的爬虫框架或者模块有哪些？**

​    Python：urllib,urllib2

​    第三方：requests

​    框架：Scrapy

**90.常用的****MySQL****引擎有哪些？**

\1. MyISAM引擎 2.InnoDB引擎

**91.一行代码实现****1-100****之和？**

​    sum(range,(0,101))

**92.列出五个****Python****标准库？**

​    os:提供不少与操作系统相关联的函数

​    sys:通常用于命令行参数

​    re:正则匹配

​    math:数学运算

​    datetime:处理时间日期

**93.迭代器和生成器的区别？**

​    迭代器：是一个更抽象的概念，任何对象，如果它的类有next方法和iter方法返回自己本身。

​    生成器：是创建迭代器的简单而强大的工具。

**94.如何提高****Pythond****的运行效率？**

​    使用生成器，关键代码使用外部功能包，针对循环的优化，尽量避免在循环中访问变量的属性。

**95.什么是****Python****？使用****Python****有什么好处？**

​    Python是一种编程语言，它有对象、模块、线程、异常处理和自动内存管理，它简洁、简单、方便、容易扩展，有许多自带的数据结构，而且Python是开源的。

**96.什么是****PEP8****？**

​    PEP8是一个编码规范。

**97.Python是如何被解释的？**

​    Python是一种解释性语言，它的源代码可以直接运行。Python解释器会将源代码转换成中间语言，之后再翻译成机器码再执行。

**98.Python中数组和元组之间的区别是什么？**

​    数组和元组之间的区别：数据内容是可以被修改的，而元组内容是只读的。

**99.Python中字典推导式和列表推导式是什么？**

​    它们是可以轻松创建字典和列表的语法结构。

**100.  Python都有哪些自带的数据结构？**

​    可变：数据、集合、字典

​    不可变：字符串、元组、数组

**101.  Python中的****pass****是什么？**

​    Pass是一个在Python中不会被执行的语句。如果一个地方需要暂时被留白，它常用于占位符。