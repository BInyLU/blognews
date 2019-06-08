在写网页的过程中，第一步就是创建一个html文档。如下是最简单的 html5 文档。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

其中一个很重要的部分是 meta 标签，这个标签根据内容的不同有不同的作用。当然这些东西百度都可以找到。

上面第一个 meta 就是规定了文档的字符编码。

meta 元素可以提供有关页面的元信息，比如针对搜索引擎和更新频度的描述和关键字。它不会显示在页面上，但是计算机可以识别。

meta 有一个必须属性是 content 定义与 http-equiv 或 name 属性相关的元信息。

> 可选属性有 三个 http-equiv、name、scheme。

name 属性主要用于描述网页。

1、定义文档关键字，用于搜索引擎。

```
<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">
```

2、用于web页面描述。

```
<meta name="description" content="Free Web tutorials on HTML and CSS">
```

3、定义作者。

```
<meta name="author" content="zhang san">
```

4、规定用于生成文档的一个软件包（不用于手写页面）

```
<meta name="generator" content="FrontPage 4.0">
```

5、规定页面所代表的 web 应用程序的名称。

```
<meta name="application-name" content="博客园">
```

6、用于标注版权信息。

```
<meta name="copyright" content="Guo">
```

7、renderer是为双核浏览器准备的，用于指定双核浏览器默认以何种方式渲染页面。

```
<meta name="renderer" content="webkit"> //默认webkit内核
<meta name="renderer" content="ie-comp"> //默认IE兼容模式
<meta name="renderer" content="ie-stand"> //默认IE标准模式
```

8、搜索引擎爬虫重访时间。如果页面不是经常更新，为了减轻搜索引擎爬虫对服务器带来的压力，可以设置一个爬虫的重访时间。如果重访时间过短，爬虫将按它们定义的默认时间来访问。

```
<meta name="revisit-after" content="7 days" >
```

 

http-equiv 属性为名称/值对提供了名称。并指示服务器在发送实际的文档之前先在要传送给浏览器的 MIME 文档头部包含名称/值对。

1、每 30 秒刷新网页。

```
<meta http-equiv="refresh" content="30">
```

2、X-UA-Compatible 告知浏览器以何种版本来渲染页面。

```
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/> //指定IE和Chrome使用最新版本渲染当前页面
```

3、指定请求和相应遵循的缓存机制。

```
<meta http-equiv="cache-control" content="no-cache">
```

content 有 5种情况：

> ① no-cache : 先发送请求，与服务器确认该资源是否被更改，如果未被更改，则使用缓存。
> ② no-store : 不允许缓存，每次都要去服务器上，下载完整的响应。为了安全。
> ③ public : 缓存所有响应，但并非必须。因为max-age也可以做到相同效果。
> ④ private : 只为单个用户缓存，因此不允许任何中继进行缓存。
> ⑤ max-age : 表示当前请求开始，该响应在多久内能被缓存和重用，而不用去服务器请求。例如：max-age=60表示响应可以再缓存和重用 60 秒。

4、用于禁止当前页面在移动端浏览时，被百度自动转码。虽然百度的本意是好的，但是转码效果很多时候却不尽人意。所以可以在head中加入例子中的那句话，就可以避免百度自动转码了。

```
<meta http-equiv="Cache-Control" content="no-siteapp" />
```



### 移动端 meta 

1、强制保持文档宽度和设备宽度 1:1。并且初始化缩放不缩放。

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

再加上 maximum-scale=1.0，表示最大缩放 为 1.0 倍。

再加上 minimum-scale = 1.0 表示最小缩放。

再加上 user-scalable=no 　　表示不允许用户点击屏幕缩放浏览。一般设置了这个就没必要设置上面两个了。

2、webapp 全屏模式，隐藏地址栏。

```
<meta name="apple-mobile-web-app-capable" content="yes" />
```

3、禁止百度转码显示。

```
<meta http-equiv="Cache-Control" content="no-siteapp">
```

4、 制定iphone中safari顶端的状态条的样式。（default:白色，black:黑色，black-translucent：半透明）

```
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
```

5、添加到 IOS 主屏后的标题。

```
<meta name="apple-mobile-web-app-title" content="标题名称">
```

6、将一连串数字识别为电话号码。默认为yes。email 识别。

```
 <meta name="format-detection"  content="telephone=no, email=no" />
```

7、删除默认的苹果工具栏和菜单。

```
 <meta name="apple-mobile-web-app-capable" content="yes" />
```

8、针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓：

```
<meta name="HandheldFriendly" content="true">
```

