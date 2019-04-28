![点击查看原图](http://uip16584232.cms1.91mb.com.cn/content/uploadfile/201810/c96b1540955871.gif)



（上面有GIF预览图，看到不动或者没图请等待加载...）

上图是两个对比图，前一个用了JS，后一个只要CSS 

下面我们来实现后一个的效果！

[点我在线预览效果](http://uip16584232.cms1.91mb.com.cn/content/templates/mini/demo3.html)

```html
<meta charset="utf-8" />

<style>
.tab{
overflow:hidden;
_zoom:1; /*最好用的解决浮动办法！没有之一！！*/
}
.label {
width: 100px;
margin-right: -1px;
border: 1px solid #ccc; 
border-bottom: 0;
padding-top: 5px; 
padding-bottom: 5px;
background-color: #eee;
text-align: center;
float: left;
}
.box {
height: 200px;
border: 1px solid #ccc;
scroll-behavior: smooth;
overflow: hidden;
}
.content {
height: 100%;
padding: 0 20px;
position: relative;
overflow: hidden;
}
.box input {
position: absolute; 
top:0;
height: 100%; 
width: 1px;
border:0; 
padding: 0; 
margin: 0;
clip: rect(0 0 0 0);
}
</style>

<div class="tab">
<label class="label" for="tab1">选项卡1</label> <label class="label" for="tab2">选项卡2</label> <label class="label" for="tab3">选项卡3</label> 
</div>

<div class="box">

<div class="content">
<input id="tab1" /> 
<p>
我是第一条内容，可以放图片或者文字
</p>
<img src="http://uip16584232.cms1.91mb.com.cn/admin/select/LOGO/manduoduo.png" height="80" /> 
</div>

<div class="content">
<input id="tab2" /> 
<p>
我是第二条内容，可以放图片或者文字
</p>
<img src="http://uip16584232.cms1.91mb.com.cn/admin/select/LOGO/manduoduo.png" height="80" /> 
</div>

<div class="content">
<input id="tab3" /> 
<p>
我是第三条内容，可以放图片或者文字
</p>
<img src="http://uip16584232.cms1.91mb.com.cn/admin/select/LOGO/manduoduo.png" height="80" /> 
</div>

</div>
```