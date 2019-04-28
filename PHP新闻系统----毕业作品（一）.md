php基础示例-- PHP与MySQL实现一个新闻管理系统

实现目标： 使用php的mysql操作函数实现一个新闻信息的发布信息、浏览全部信息、查看指定信息、修改信息和删除信息操作。

实现步骤：

 一、 创建数据库和表

 

\1. 创建数据库：newsdb

 

\2. 创建表格：news

 

 

开始创建数据库、表；字段：新闻id，标题、关键字、作者、发布时间、新闻内容

mysql表语句：

```js
create database newsdb;
use newsdb;
CREATE TABLE `news` (
`id` int(10) unsigned NOT NULL auto_increment,
`title` varchar(64) NOT NULL,
`keywords` varchar(64) NOT NULL,
`author` varchar(16) NOT NULL,
`addtime` int(10) unsigned NOT NULL,
`content` text NOT NULL,
PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

 二、 创建php文件编写代码

 

\--------------

 

|--dbconfig.php 公共配置信息文件，数据库连接配置信息

 

|

 

|--menu.php 网站公共导航栏

 

|

 

|--index.php 浏览新闻的文件

 

|

 

|--add.php 发布新闻表单页

 

|

 

|--edit.php 编辑新闻的表单页

 

|

 

|--action.php 执行新闻信息添加、修改、删除等操作的动作处理页

 

|

 

|--fine.php 查看指定ID信息页





三：修改成你自己的配置

把你的locahost账号密码和mysql数据库名字对应自己本机改就OK了。
[![phpxinwen.png](http://uip16584232.cms1.91mb.com.cn/content/uploadfile/201811/5ca91542389051.png)](http://uip16584232.cms1.91mb.com.cn/content/uploadfile/201811/5ca91542389051.png)



[PHP新闻系统源码](https://pan.baidu.com/s/1tdZJP31JxTfOWBBFeCC5ZQ)



（源码附上，亲手写的，有啥不懂可前来问道）



PS：mysql数据库需要先建库后再引入本sql数据表，否则引入失败。
再PS：下次继续发作品，商品管理系统，记得收藏本站，干货多多噢~