drop-shadow与box-shadow都是阴影效果（光晕效果）的css属性，二者最大的不同点在于：box-shadow只能制作矩形的阴影，而drop-shadow则可以制作和物件不透明区域完全相同形状的阴影。底下是二个css属性的用法：

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190608/1.jpg)

```
.drop-shadow {
    -webkit-filter: drop-shadow(12px 12px 7px rgba(0, 0, 0, 0.7));
    filter: drop-shadow(12px 12px 7px rgba(0, 0, 0, 0.7))
}

.box-shadow {
    box-shadow: 12px 12px 7px rgba(0, 0, 0, 0.7);
}
```

因为都是阴影效果（光晕效果），所以二者可以设定的参数（value）几乎一样：以上面的例子来说，参数的所有数值从左到右代表了：水平偏移，垂直偏移，阴影模糊距离阴影颜色。接下来将为您进一步比较drop-shadow与box-shadow



**边框和变形效果**

drop-shadow与box-shadow的阴影都可以反应出边框圆角和变形效果。不同的是：drop-shadow反应出实际边框的形状、实线框有实线的影子、虚线框有虚线的影子；box-shadow则是把边框和里面的内容当成是一个完整的方块、并制造出整个方块的影子，而边框的样式会被忽略，直接当成是实线框。

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190608/2.jpg)

```
.box {
    border: 5px solid #262b57;
    width: 120px;
    height: 120px;
    border-radius: 10px;
    transform: rotate(15deg);
    font-size: 40px;
    text-align: center;
    line-height: 120px;
}

.dashed {
    border-style: dashed;
}
```



**背景与透明度**

如果方块有设定颜色（不是透明的），drop-shadow与box-shadow的阴影效果看来就会差不多。如果方块的背景是半透明的呢？我们可以从图片中发现，影子周围的颜色比较深，中间的颜色比较淡，所以可以推论出透明度对drop-shadow会造成影响、对box-shadow则没有影响。

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190608/3.jpg)

```
.bk {
    background-color: #ffcc66;
}

.bk-alpha {
    background-color: rgba(255, 204, 102, 0.3);
}
```



**图形边框（image border）**

由示例中我们得知drop-shadow可以反应出image-border不规则的形状，box-shadow则是将边框直接视为实心框，忽略边框图片的形状。图片中的猫头鹰是透明的PNG图档，drop-shadow不仅反应出边框图片的形状、也反应出边框内猫头鹰的形状；box-shadow则是秉持一贯的原则、将边框和图片视为一个完整的方块。

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190608/4.jpg)

```
.frame {
    width: 286px;
    height: 240px;
    -moz-border-image: url(frame_green_.png) 25 25 repeat;
    -webkit-border-image: url(frame_green_.png) 25 25 repeat;
    border-width: 25px;
    border-image: url(frame_green_.png) 25 25 repeat;
    border-color: #79b218;
    border-style: inset;
    border-width: 25px;
    box-sizing: border-box;
    display: block;
    margin: 10px;
}
```



**伪元素**

伪元素drop-shadow可以反应出伪元素的形状，box-shadow则是会忽略伪元素。

```
.addition {
    width: 100px;
    height: 100px;
    background-color: #ffcc66;
    margin: 10px 60px;
    position: relative;
    display: inline-block;
}

.addition:before {
    width: 50px;
    height: 50px;
    background-color: #ff8833;
    content: '';
    display: block;
    position: absolute;
    left: 0;
    top: 50%;
    margin-left: -40px;
    transform: rotate(45deg);
    margin-top: -10px;
}

.addition:after {
    width: 60px;
    height: 60px;
    background-color: #ff8833;
    margin: 10px;
    content: '';
    display: block;
    transform: rotate(20deg);
    transform: skew(20deg, 20deg);
    top: 5px;
    right: -40px;
    position: absolute;
}
```



**区块内的小区块**

drop-shadow的影子可以反应出区块内所有元素的形状、box-shadow则是直接对区块反应出矩形的影子。

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190608/5.jpg)

```
.square {
    width: 50px;
    height: 50px;
    display: inline-block;
    background-color: #ffcc66;
    margin: 20px;
}
.circle {
    width: 50px;
    height: 50px;
    display: inline-block;
    border-radius: 50%;
    background-color: #ff8833;
    margin: 20px;
}
<div class="demo-wrap">
    <div class="drop-shadow">
        <span class="square"></span>
        <span class="circle"></span>
        <p>drop-shadow</p>
    </div>
    <div class="box-shadow">
        <span class="square"></span>
        <span class="circle"></span>
        <p>box-shadow</p>
    </div>
</div>
```



**drop-shadow与box-shadow不同点**

drop-shadow没有内部边框（inset shadow）及距离（spread）二种特性。

就支持性部份来说，目前IE还不支持drop-shadow属性；而所有浏览器都已经普遍支持box-shadow。