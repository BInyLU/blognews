 页面滑动不了解决方案：

```html
<script type="text/javascript">
mui.init();
//内容允许滚动
(function($) {
$(".mui-scroll-wrapper").scroll({
bounce: true, //滚动条是否有弹力默认是true
indicators: true, //是否显示滚动条,默认是true
});
})(mui);
</script>
```

 

 

onclick和a标签跳转/点击事件用不了解决方案：

onclick和a标签跳转/点击事件用不了解决方案：onclick就是tap！！onclick被MUI封装成tap！！

```html
<!-- tap方法等于click -->
<script type="text/javascript">
$("你的id/class").on('tap', 'a', function(event) {
this.click();
});
</script>
```

 

 APP初始化模板：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>xx</title>
<link rel="icon" type="image/x-icon" href="lib/img/favicon.ico" />
<meta name="viewport" content="width=device-width, initial-scale=1,maximum-scale=1,user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<link rel="stylesheet" href="lib/css/mui.min.css">
<link rel="stylesheet" type="text/css" href="lib/css/app.css" />
<script src="lib/js/mui.min.js "></script>
</head>
<body>
<div class="mui-off-canvas-wrap mui-draggable">
<!-- 主页面容器 -->
<div class="mui-inner-wrap">
<!-- 主页面标题 -->
<header class="mui-bar mui-bar-nav">
<h1 class="mui-title">xx</h1>
</header>
<!-- 主页面内容容器 -->
<div class="mui-content mui-scroll-wrapper">
<div class="mui-scroll">
<!-- 主界面具体展示内容 -->
</div>
</div>
<div class="mui-off-canvas-backdrop"></div>
</div>
</div>
</body>
</html>
```