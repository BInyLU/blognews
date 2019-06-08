### 1、双倍浮动BUG：

描述：块状元素设置了float属性后，又设置了横向的margin值，在IE6下显示的margin值要比设置的值大；

解决方案：给float的元素添加 display:inline;将其转换为内联元素；



### 2、表单元素行高不一致：

解决方案：

　　①、给表单元素添加vertical-align:middle;

　　②、给表单元素添加float:left；



### 3、IE6（默认16px为最小）不识别较小高度的标签（一般为10px）：

解决方案：

　　①、给标签添加overflow:hidden;

　　②、给标签添加font-size:0;



### 4、图片添加超链接时，在IE浏览器中会有蓝色的边框：

解决方案：

　　给图片添加border:0或者border：none;



### 5、最小高度min-height不兼容IE6;

解决方案：

　　①、min-height:100px;_height:100px;

　　②、min-height:100px;height:auto!important;height:100px;



### 6、图片默认有间隙：

解决方案：

　　①、给img添加float属性；

　　②、给img添加display：block;



### 7、按钮默认大小不一：

解决方案：

　　①、如果按钮是一张图片，直接用背景图作为按钮图片；

　　②、用a标记模拟按钮，使用JS实现其他功能；



### 8、百分比BUG：

描述：父元素设置100%，子元素各50%，在IE6下，50%+50%大于100%；

解决方案：

　　给右边的浮动元素添加clear:right；



### 9、鼠标指针BUG：

　　cursor:hand 只有IE浏览器识别；

　　cursor:pointer;IE及以上浏览器和其他浏览器都识别（手型）；



### 10、透明度设置，IE不识别opacity属性：

解决方案：

　　标准写法：opacity:value;(取值范围0-1)；

　　兼容IE浏览器 filter:alpha(opacity=value);(取值范围1-100)；



### 11、上下margin重叠问题：

描述：给上面的元素设置margin-bottom，给下面的元素设置margin-top,只能识别其中较大的那个值；

解决方案：　　

　　①、margin-top和margin-bottom 只设置其中一个值；

　　②、给其中一个元素再包裹一个盒子，并设置over-flow:hidden;



### 12、给子元素设置margin-top.应用在了父元素上：

解决方案：

　　①、把给子元素设置的margin-top改为给父元素设置padding-top;

　　②、给父元素设置1px的border,即border-top:1px solid transparent;

　　③、给父元素设置over-flow:hidden;

　　④、给父元素设置float:left；