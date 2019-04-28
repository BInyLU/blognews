> 近来在公开课上学习了一下如何使用canvas，发现一个事实，能把canvas特效写得很六的人，往往他的数学一定好，还不是一般的好。因为不仅仅概念抽象，还居多理论....今天我将使出吃奶的劲，把当年高考30多分的数学功力全部拿出来......
>
> 这篇文章将会带领大家使用canvas绘制一个简单的时钟，小白初步进行canvas绘制。

### 准备工作

在HTML中指定一个区域放置时钟：

```html
<div id="clock" style="position: relative;"></div>
```

时钟的一些外观设定：

```javascript
var width = 260; // 桌布宽度
var height= 260; // 桌布高度
var dot = {
    x : width / 2,
    y : height / 2,
    radius : 6
}; // 圆点位置、半径
var radius = 120; // 圆半径
var borderWidth = 6; // 圆边框宽度
```

创建canvas元素：

```javascript
var clock = document.getElementById('clock');
var clockBg = document.createElement('canvas');
var clockPointers = document.createElement('canvas');

clockPointers.width = clockBg.width = width;
clockPointers.height = clockBg.height = height;
clockPointers.style.position = 'absolute';
clockPointers.style.left = 0;
clockPointers.style.right = 0;

clock.appendChild(clockBg);
clock.appendChild(clockPointers);
```

这里要创建两个canvas元素，目的在于把时钟的圆盘跟指针分离开。这是因为指针要根据当前时间擦除重绘，如果放置在一个canvas中，擦除的时候就会把圆盘也给擦掉了。

### 绘制圆盘

但凡要在canvas中绘图，都要先获得其上下文，对应的接口是 **canvas.getContext**：

```javascript
var bgCtx = clockBg.getContext('2d');
```

目前canvas.getContext接口的唯一一个合法参数是'**2d**'，将来应该会支持3D绘图。

先来绘制最外面的圆框：

```javascript
bgCtx.beginPath();
bgCtx.lineWidth = borderWidth;
bgCtx.strokeStyle = '#000';
bgCtx.arc(dot.x, dot.y, radius, 0, 2 * Math.PI, true);
bgCtx.stroke();
bgCtx.closePath();
```

绘图的流程其实都是类似的：

1. 调用 **context.beginPath()** 新建路径；
2. 设置颜色等样式；
3. 调用路径函数生成路径；
4. 画线（**stroke**）或者填充（**fill**）；
5. 调用 **context.closePath()** 关闭路径；

上面用到的 **context.arc** 接口可以生成圆弧路径，其详细说明参见[此处](http://www.html5canvastutorials.com/tutorials/html5-canvas-arcs/)。

用类似的方法，画出圆点：

```javascript
bgCtx.beginPath();
bgCtx.fillStyle = '#000';
bgCtx.arc(dot.x, dot.y, dot.radius, 0, 2 * Math.PI, true);
bgCtx.fill();
bgCtx.closePath();
```

此时，结果如下图所示：

![时钟圆框和圆点](https://mrluo.life/upload/article/201109/2011090323102236129.png)

### 绘制刻度

最复杂的地方就是画刻度了，这里要先复习一下数学中的[三角函数](http://zh.wikipedia.org/wiki/%E4%B8%89%E8%A7%92%E5%87%BD%E6%95%B0)：

![三角函数](http://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/Versine.svg/240px-Versine.svg.png)

刻度的起始位置就是圆框上的一个点，第一步就是要知道这个点的坐标。上图中：

```
sinθ = AC / AO
cosθ = OC / AO
```

其中**AO即为圆半径**，而θ的值则根据刻度而定。0是π/2，3是0，6是3π/2，9是π：

![三角函数坐标](http://upload.wikimedia.org/wikipedia/commons/thumb/c/c9/Unit_circle_angles.svg/300px-Unit_circle_angles.svg.png)

由此可得到刻度起始点的位置为：

```
x = 圆点横坐标 + AO * cosθ
y = 圆点纵坐标 + AO * sinθ
```

同理可算出刻度结束点的位置为（结束点相当于在一个半径为**圆框半径-刻度长度**的圆上）：

```
x = 圆点横坐标 + (AO - 刻度长度) * cosθ
y = 圆点纵坐标 + (AO - 刻度长度) * sinθ
```

于是，这程序可以写了：

```javascript
for (var i = 0, angle = 0, tmp, len; i < 60; i++) {
    bgCtx.beginPath();

    // 突出显示能被5除尽的刻度
    if (0 === i % 5) {
        bgCtx.lineWidth = 5;
        len = 12;
        bgCtx.strokeStyle = '#000';
    } else {
        bgCtx.lineWidth = 2;
        len = 6;
        bgCtx.strokeStyle = '#999';
    }

    tmp = radius - borderWidth / 2; // 因为圆有边框，所以要减去边框宽度
    bgCtx.moveTo(
        dot.x + tmp * Math.cos(angle),
        dot.y + tmp * Math.sin(angle)
    );
    tmp -= len;
    bgCtx.lineTo(dot.x + tmp * Math.cos(angle), dot.y + tmp * Math.sin(angle));
    bgCtx.stroke();
    bgCtx.closePath();

    angle += Math.PI / 30; // 每次递增1/30π
}
```

画好刻度后，结果应该是这样：

![时钟刻度](https://mrluo.life/upload/article/201109/2011090323102447480.png)

### 画指针

先得获取指针canvas的上下文：

```javascript
var ptxContext = clockPointers.getContext('2d');
```

由于画指针的操作每隔一秒都要执行一次，所以这里就写成一个函数，方便传给setInterval调用：

```javascript
function updatePointers() {
    ptCtx.clearRect(0, 0, width, height);　　// 清掉原来的指针

    // 获取当前时间
    var now = new Date();
    var h = now.getHours();
    var m = now.getMinutes();
    var s = now.getSeconds();

    // 算出时分秒指针现在应指向圆的几分之几处
    h = h > 12 ? h - 12 : h;
    h = h + m / 60;
    h = h / 12;
    m = m / 60;
    s = s / 60;

    drawPointers(s, 2, 92); // 画秒针
    drawPointers(m, 4, 82); // 画分针
    drawPointers(h, 6, 65); // 画时针
}
```

drawPointers函数的实现是：

```javascript
// angle是角度，lineWidth是指针宽度，length是指针长度
function drawPointers(angle, lineWidth, length) {
    angle = angle * Math.PI * 2 - Math.PI / 2;

    ptCtx.beginPath();
    ptCtx.lineWidth = lineWidth;
    ptCtx.strokeStyle = "#000";
    ptCtx.moveTo(dot.x, dot.y);
    ptCtx.lineTo(dot.x + length * Math.cos(angle), dot.y + length * Math.sin(angle));
    ptCtx.stroke();
    ptCtx.closePath();
}
```

这里主要也是用到三角函数，就不啰嗦了，但是要注意angel的计算。由于传入的angel是一个百分数，所以要乘以一个圆周，也就是2π，才知道对应的弧度，算出来以后还要减去π/2，因为从上面的坐标图就可以看到，0是位于x轴而不是y轴，刚好比正常的时钟多了π/2。

最后别忘了调用updatePointers实时更新指针：

```javascript
setInterval(updatePointers, 1000);
updatePointers();
```

这下时钟完全出来了，除了初步熟悉canvas绘图API外，还顺便复习了一次三角函数.....说实话，过程痛苦...

![时钟最终结果](https://mrluo.life/upload/article/201109/2011090323102452366.png)