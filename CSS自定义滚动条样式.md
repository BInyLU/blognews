###### -webkit-scrollbar

众所周知，浏览器默认的滚动条样式实在是..不怎么美观，当然，既然不堪入目，那么我们就要去改变它。

 css3的强势登场，颠覆了往日js独大浏览器动画 效果 美观 体验等。回到当前，先了解下滚动条。

滚动条从外观来看是由两部分组成：

1、可以滑动的部分，即为滑块。

2、滚动条的轨道，即滑块的轨道，为了明显 美观，通常轨道颜色较浅，突出滑块。

##### 滚动条的样式主要有三部分组成：

```
1、::-webkit-scrollbar   滚动条整体样式；
2、::-webkit-scrollbar-thumb  滑块部分；
3、::-webkit-scrollbar-thumb  轨道部分；
```

##### 下面就来看一下代码：

```
<div class="box">
    <div class="scrollbar"></div>
</div>
```

##### CSS代码：

```
    .box{
        width: 150px;
        height: 200px;
        overflow: scroll;
        margin: 100px;
    }
    .scrollbar{
        width: 30px;
        height: 300px;
        margin: 0 auto;

    }

    /*滚动条整体样式*/
    .box::-webkit-scrollbar {
        width: 10px;
        height: 1px;
    }
    /*滚动条滑块*/
    .box::-webkit-scrollbar-thumb {
        border-radius: 10px;
        -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
        background: #535353;
    }
    /*滚动条轨道*/
    .box::-webkit-scrollbar-track {
        -webkit-box-shadow: inset 0 0 1px rgba(0,0,0,0);
        border-radius: 10px;
        background: #ccc;
    }
```

##### 看下效果：







![img](https:////upload-images.jianshu.io/upload_images/11248214-02a76d0e64aae3b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/83/format/webp)

