实现3D的翻面，主要使用了transform-style:preserve-3d这串新属性。
其余的过于简单，直接贴出源码和[在线测试地址](http://uip16584232.cms1.91mb.com.cn/content/templates/mini/demo7.html)吧。

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>翻面</title>
<style type="text/css">
img {
display: block;
}

.box {
width: 50%;
height: 50vh;
margin: 50px auto 0;
transform-style: preserve-3d;
position: relative;
}

.box .pic {
width: 100%;
height: 400px;
background: #333;
position: absolute;
left: 0;
top: 0;
transform: perspective(800px) rotateY(0deg);
backface-visibility: hidden;
transition: all 500ms ease;
box-shadow: 20px 20px 10px rgba(0, 0, 0, .1);
}

.box .back_info {
width: 100%;
height: 400px;
background: #ccc;
position: absolute;
left: 0;
top: 0;
color: #fff;
transform: rotateY(180deg);
backface-visibility: hidden;
transition: all 500ms ease;
box-shadow: 20px 20px 10px rgba(0, 0, 0, .1);
}

.box:hover .pic {
transform: perspective(800px) rotateY(180deg);
}

.box:hover .back_info {
transform: perspective(800px) rotateY(0deg);
}
</style>
</head>
<body>
<div class="box">
<div class="pic"></div>
<div class="back_info"></div>
</div>
</body>
</html>
```