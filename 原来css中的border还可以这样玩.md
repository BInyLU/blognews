> 在看这篇文章之前你可能会觉得border只是简单的绘制边框，看了这篇文章，我相信你也会跟我一样说一句“我靠，原来css中的border还可以这样玩”。这篇文章主要是很早以前看了别人用纯CSS绘制三角形后自己的一些思路的整理，文中会介绍几种小图标的效果。

## 用css中的border绘制鸡蛋形状：

是的你没看错，这里是要做绘制一个类似于鸡蛋的效果。

思路：我们先用div绘制一个正方形，然后利用设置border-radius: 50%;，这样我们就可以得到一个圆形的效果，代码如下：

html代码：

```html
<div class="div"></div>
```

css代码：

```html
.div {
    width: 100px;
    height: 100px;
    line-height: 100px;
    background-color: aqua;
    text-align: center;
    border-radius: 50%;
}
```

结果如图：



![原来css中的border还可以这样玩0](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016193140405-553843487.png)



思考：分析鸡蛋型结构，鸡蛋有点椭，但是它分大头和小头。我们有没有什么办法先让之前的圆变为椭圆呢？

思路：我们改变div的宽度或高度，让它们不一致，看能不能得到我们想要的效果。

实现：我们分别改变width:50px;或height:50px;（只改变其中的一个），这时我们得到的效果分别为：

![原来css中的border还可以这样玩1](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016194256655-826795641.png)



思考：我们已经得到椭圆效果了，接下来我们如何实现大头和小头的效果呢？

思路一：我们再把椭圆进行分割然后控制宽度不一致。（此种方法不成功）

思路二：我们设置border-radius的百分比。当border-radius: 100%;与前一种方法的截图如下：

![原来css中的border还可以这样玩2](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016194744983-284998765.png)



再次尝试将border-radius的百分比的值进行分离（不要简写，直接写成4个），然后控制百分比不一致。关键代码：

```html
border-radius: 50% 50% 50% 50% / 62% 62% 38% 38%;
```

此时得到的效果截图：

![原来css中的border还可以这样玩3](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016195108374-746224147.png)

 

 

## 用css中的border画三角形：

相信大家都知道border-color是控制边框颜色的，但是你可能没这样试过，来看下面的代码：

html：

```html
<div class="div"></div>
```

css：

```html
.div {
    width: 100px;
    height: 100px;
    border: 50px solid transparent;
    border-color: yellow green red aqua;
}
```

这样的结果为：

![原来css中的border还可以这样玩4](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016200250155-1620825275.png)

思考一：如果该div没有宽高会怎样？

实现结果：

![原来css中的border还可以这样玩5](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016200510749-1055683188.png)

思考二：前面的效果得到的是四个三角形，我们有没有什么办法将三角形从那个div中分离出来呢？

思路：目前没有接触过有关div分离的（具体也应该不存在吧），但是我们来扣一扣CSS的定义“层叠样式”，转换我们的思维，我们有没有什么方法将我们不想要的三角形覆盖掉？

具体做法：将我们需要的那边的颜色设置为我们的背景色–白色，对的这样就可以得到我们想要的效果。代码如下（以想要上边的三角形为例）：

```
border-color: yellow white white white;
```

是不是这样就算完成我们的三角形效果了呢？

我们可以试试修改整个body的背景颜色为黑色，看有什么变化：

![原来css中的border还可以这样玩6](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016215842045-256056530.png)

发现该div仍占据着那么大的空间，并且背景颜色设置为白色并不是最科学

思考四：我们该如何将不想要的颜色设置为消失呢？

思路：我们将不想要表现出来的颜色设置为父级容器的背景色，border-color: yellow transparent transparent transparent;

结果如下：

![原来css中的border还可以这样玩7](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016215910967-287546268.png)

思考三：我们如何将div设置不占那么大的空间呢？

思路：直接将想要的三角形的对边的border的宽度去掉

具体做法：（这次以想要下面的三角形为例），代码如下：

```
.div{
width:0px;
height: 0px;
border-bottom: 50px solid red;
border-left: 50px solid transparent;
border-right: 50px solid transparent;
}
```

结果如图：

![原来css中的border还可以这样玩8](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016215929842-1973085104.png)

## 关于三角形的扩展的一些思考：

思考一：我们平时的三角形有锐角三角形，钝角三角形，直角三角形，等边三角形，等腰三角形等，我们有什么办法让我们直接得到的就是我们想要的三角形效果不？

思路：当底边和水平线平行时，我们直接通过控制宽高比来实现我们想要的三角形效果；当与水平线不重合时这个时候就比较复杂了，就需要用到宽高比和 CSS3中的transform属性和rotate相结合，让我们的三角形呈现出我们想要的效果（这里只是介绍思路，不去具体实现，其中有涉及到数学方面 的知识可以自己百度）。

思考二：我们能不能用多个三角形在一起拼出更多的形状？

（这个可以有，比如说我们可以用两个三角形和一个长方形拼成平行四边形，甚至说我们用多个div在一起拼成简单的小木屋效果……）

补充：

1、在我们思考一的前面那张图，我们可以看到其实那中间的几个分别是梯形，用同样的方法，我们可以得到梯形的效果（具体做法不再另外介绍）。

2、通过旋转，我们可以将我们的正方形变成菱形的效果

## 多边形的制作（以六边形为例）

首先我们分析一下六边形，看能不能把它分解成我们前面有说过的简单的图形，下面看图：

![原来css中的border还可以这样玩9](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016215948108-1622004561.png)

分析：以上面的为例，我们可以看出六边形由两个三角形和一个矩形构成。

思考一：我们有没有什么方法将这三个图形拼在一起？

思路：用伪元素：after和:before，然后在各自的区域绘制图形

参考代码如下：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title>六边形的制作</title>
        <style type="text/css">
            #hexagon {
                width: 100px;
                height: 55px;
                background: #fc5e5e;
                position: relative;
                margin: 100px auto;
            }
            
            #hexagon:before {
                content: "";
                width: 0;
                height: 0;
                position: absolute;
                top: -25px;
                left: 0;
                border-left: 50px solid transparent;
                border-right: 50px solid transparent;
                border-bottom: 25px solid yellow;
            }
            
            #hexagon:after {
                content: "";
                width: 0;
                height: 0;
                position: absolute;
                bottom: -25px;
                left: 0;
                border-left: 50px solid transparent;
                border-right: 50px solid transparent;
                border-top: 25px solid aqua;
            }
        </style>
    </head>

    <body>
        <div id="hexagon"></div>
    </body>

</html>
```

（当然这里知识介绍了一种情况，也可以尝试三角形所在的边不一样）

## 多角星的制作（以六角星为例）

分析：试着用前面的方法，我们分析六角星的结构，我们可以理解为一个六角星是由两个三角形一起重叠而成的，接下来就好办了，我们直接看代码：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>六角星</title>
        <style type="text/css">
            
            div {
                width: 0;
                height: 0;
                display: block;
                position: absolute;
                border-left: 100px solid transparent;
                border-right: 100px solid transparent;
                border-bottom: 200px solid #de34f7;
                margin: 10px auto;
            }
              
            div:after {
                content: "";
                /*content插入内容*/
                width: 0;
                height: 0;
                position: absolute;
                border-left: 100px solid transparent;
                border-right: 100px solid transparent;
                border-top: 200px solid #de34f7;
                margin: 60px 0 0 -100px;
            }
        </style>
    </head>
    <body>
        <div></div>
    </body>
</html>
```

最终实现效果如图：

![原来css中的border还可以这样玩10](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016220237905-1822663767.png)

 

五角星的制作（实际操作起来比六角星困难）：我们先自己画一个五角星，然后将其分割为三个，然后利用前面的步骤去实现，这里我只是列出一种方法作为参考（其中有几个细节的处理有点复杂），分析图如下：

![原来css中的border还可以这样玩11](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016220259311-1013147642.png)

参考代码如下：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            #star{
                width: 0px;
                height: 0px;
                margin: 50px 0;
                color: red;
                position: relative;
                display: block;
                border-bottom: 70px solid red;
                border-left: 100px solid transparent;
                border-right: 100px solid transparent;
                -webkit-transform: rotate(35deg);
            }
            #star:before{
                content: '';
                width: 0px;
                height: 0px;
                margin: 50px 0;
                color: yellow;
                position: relative;
                display: block;
                border-bottom: 80px solid yellow;
                border-left: 30px solid transparent;
                border-right: 30px solid transparent;
                -webkit-transform: rotate(-35deg);
                top: -45px;
                left: -65px;
            }
            #star:after{
                content: '';
                width: 0;
                height: 0;
                position: absolute;
                display: block;
                top: 3px;
                left: -105px;
                color: #fc2e5a;
                border-right: 100px solid transparent;
                border-bottom: 70px solid #fc2e5a;
                border-left: 100px solid transparent;
                -webkit-transform: rotate(-70deg);
                -moz-transform: rotate(-70deg);
                -ms-transform: rotate(-70deg);
                -o-transform: rotate(-70deg);
                
            }
        </style>
    </head>
    <body>
        <div id="star"></div>
    </body>
</html>
```

## CSS小图标效果：

到这里，你是不是还没看过瘾呢？下面在来分享一下自己做的CSS小图标：对话框的制作

对话框的制作：

分析：对话框由一个三角形和一个圆角举行组成

实现：代码如下：

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            * {
                margin: 0px;
                padding: 0px;
            }
            
            div {
                margin: 100px;
            }
            
            #comment_bubble {
                width: 300px;
                height: 100px;
                background: #088cb7;
                position: relative;
                -moz-border-radius: 12px;
                -webkit-border-radius: 12px;
                border-radius: 12px;
            }
            
            #comment_bubble:before {
                content: "";
                width: 0;
                height: 0;
                right: 100%;
                top: 38px;
                position: absolute;
                border-top: 13px solid transparent;
                border-right: 26px solid #088cb7;
                border-bottom: 13px solid transparent;
            }
        </style>
    </head>

    <body>
        <p>消息提示框可以先制作一个圆角矩形，然后在需要的地方放置一个三角形。</p>
        <div id="comment_bubble">

        </div>
    </body>
</html>
```

实现结果：

![原来css中的border还可以这样玩12](http://www.webhek.com/wordpress/wp-content/uploads/2016/10/994899-20161016220441874-57204683.png)

