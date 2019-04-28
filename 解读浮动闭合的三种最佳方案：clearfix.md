## **最优浮动闭合第一方案：**



```css
.clearfix:after{content:".";display:block;height:0;clear:both;visibility:hidden}
.clearfix{*+height:1%;}
```



用法很简单，在浮动元素的父云素上添加class=”demo clearfix”。

你会发现这个办法也有个弊端，但的确是小问题。改变css写法就ok了：



```css
.demo:after,.demo2:after{content:".";display:block;height:0;clear:both;visibility:hidden}
.demo,.demo2{*+height:1%;}
```



以上写法就避免了改变html结构，直接用css解决了。

## **最优浮动闭合第二方案：很拉轰的浮动闭合办法**



```css
.clearfix{overflow:auto;_height:1%}
```


这种办法是我看国外的一篇文章得到的方案，测试了，百试不爽，真的很简单，很给力。喜欢的同学也可以试试这个办法。





## **最优浮动闭合第三方案：**



这种方法是端友radom提供的，测试通过：



```css
.clearfix{overflow:hidden;_zoom:1;}
```



感谢radom提供的这种方法！！