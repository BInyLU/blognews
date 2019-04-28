> 中高级前端在写页面的同时，他们更注重的是代码的可用性/重用性/可维护性/灵活性，这样做的好处是利于后期添加或修改，只需要在后台操作即可，或者最多添加一两行JS代码即可重新利用，而不用重新找HTML、CSS修改DOM。

话不多说，咱们来做以下这个效果。

![Animation](http://binylu.cn/new/3D.gif)

首先，我先把结构大概写出来，`side`大盒子固定起来，`side-box`再做父级，`side-content`是装载图片内容，`side-back``side-top``side-bottom``side-front`这些都是改变3d方向的属性。那么，事情就好办了。

```
<div class="side">
    <div class="side-box">
        <div class="side-content side-back"></div>
        <div class="side-content side-top"></div>
        <div class="side-content side-bottom"></div>
        <div class="side-content side-front"></div>
    </div>
</div>
```

首先我们的`side`大盒子，好让我们给个`perspective`透视属性，顺带兼容`webkit`内核。

```c&#39;s
.side {
        margin: 100px auto;
        width: 800px;
        height: 300px;
        perspective: 1000px;
        -webkit-perspective: 1000px;
    }

```

接下来好办了，我们给轮播图片来个父级`side-box`，再声明这是个3D属性`preserve-3d`，那么接下来就可以把子级所有图片初始化在同一位置，这样，我们只需要做的，只需要把这个父级进行动画翻动图片即可。

```
 .side-box {
        position: relative;
        width: 800px;
        height: 300px;
        transform-style: preserve-3d;
        transform: translateZ(-150px) rotateX(0deg);
        animation: more 10s ease-in-out infinite 2s;
    }

```

四个图片，四个子级，子级里面的元素做好3D定位，再给每一个子级加上图片。

```
    .side-content {
        position: absolute;
        width: 800px;
        height: 300px;
        box-sizing: border-box;
        background: 0/100%;
        box-shadow: 8px 14px 38px rgba(0,0,0,.2);
    }
    .side-back {
        transform: translateZ(-150px) rotateX(180deg);
		background-image: url('https://askd.github.io/codepen/back.jpg');
    }
    .side-top {
        transform: translateY(-150px) rotateX(90deg);
		background-image: url('https://askd.github.io/codepen/top.jpg');
    }
    .side-bottom {
        transform: translateY(150px) rotateX(270deg);
		background-image: url('https://askd.github.io/codepen/bottom.jpg');
    }
    .side-front {
        transform: translateZ(150px);
		background-image: url('https://askd.github.io/codepen/front.jpg');
    }
```

接下来就是动画效果了，把动画加在父级盒子，让它转起来。0%-5%是动画过渡的时间段，5%-25%是图片内容停留的时间段，以此类推。

```
    @keyframes more {
        0% { transform: translateZ(-150px) rotateX(0deg); }
        5% { transform: translateZ(-150px) rotateX(90deg); }
        25% { transform: translateZ(-150px) rotateX(90deg); }
        30% { transform: translateZ(-150px) rotateX(180deg); }
        50% { transform: translateZ(-150px) rotateX(180deg); }
        55% { transform: translateZ(-150px) rotateX(270deg); }
        75% { transform: translateZ(-150px) rotateX(270deg); }
        80% { transform: translateZ(-150px) rotateX(360deg); }
        100% { transform: translateZ(-150px) rotateX(360deg); }
    }

```

只用了html、css就可以实现轮播图，神奇吧。