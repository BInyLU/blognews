

![锚点选项卡切换](https://image.zhangxinxu.com/image/blog/201810/anchor-tab.gif)

（上面有GIF预览图，看到不动或者没图请等待加载...）

咱们废话不多说，直接上代码！

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
</head>
<style>
.box {
width: 20em;
height: 10em;
border: 1px solid #ddd;
overflow: hidden;
}
.list {
font-size:8em;
height: 100%;
background: #ddd;
text-align: center;
position: relative;
}
.list > input {
position: absolute; top:0;
height: 100%; width: 1px;
border:0; padding: 0; margin: 0;
clip: rect(0 0 0 0);
}
.click {
display: inline-block;
width: 2em;
height: 2em;
line-height: 2em;
border: 1px solid #ccc;
background: #f7f7f7;
color: #333;
font-size: 1em;
font-weight: bold;
text-align: center;
text-decoration: none;
}
.click:hover {
background: #eee;
color: #345;
text-decoration: none;
}
.link {
margin-top: 1em;
}
</style>
<body>
<div class="box">
<div class="list"><input id="one">1</div>
<div class="list"><input id="two">2</div>
<div class="list"><input id="three">3</div>
<div class="list"><input id="four">4</div>
</div>
<div class="link">
<label class="click" for="one">1</label>
<label class="click" for="two">2</label>
<label class="click" for="three">3</label>
<label class="click" for="four">4</label>
</div>
</body>
</html>
```