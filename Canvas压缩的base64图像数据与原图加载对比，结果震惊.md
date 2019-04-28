使用HTML5 FileReader和Canvas压缩上传的图片。



总体思路是：

\1. 使用html5的FileReader接口来读取用户上传的图片

\2. 使用canvas drawImage接口绘制到Canvas 2d中

\3. 使用canvas toDataUrl接口把图片转成base64编码字符串（这里可以降低图片质量）

\4. 完成image src的替换后，表单提交时，就提交新的被压缩过的图像

（该方案支持IE10+, FF, Chrome, Safari等现代浏览器）



- [ 在线演示地址](http://uip16584232.cms1.91mb.com.cn/content/templates/mini/demo.html)  可以看看我写的demo，写法不一样





以下使用network测试效率，结果非常惊人有木有！

原图2.3m加载平均耗时10秒，

base64图像数据加载平均1毫秒以下！

图片质量相差不大，加载效率却迥然不同！





![Animation.gif](http://uip16584232.cms1.91mb.com.cn/content/uploadfile/201811/4d631541601074.gif)











[![text.png](http://uip16584232.cms1.91mb.com.cn/content/uploadfile/201811/d9d91541601254.png)](http://uip16584232.cms1.91mb.com.cn/content/uploadfile/201811/d9d91541601254.png)
有两点需要注意：

\1. 注意在FF下，类似这样的处理方案必须确保canvas绘制和toDataUrl的处理是同步进行的，

也就是不能是异步处理的，否则可能会出现其他事件触发页面组合（Composite）而导致canvas缓存被清空的情况，那样toDataUrl出来的会是空白字符串。

\2. 需要等image加载完成再做draw和转换的动作，否则一些浏览器会有问题。





[ 在线演示地址](http://uip16584232.cms1.91mb.com.cn/content/templates/mini/demo.html) 快把你的图片转化为base64图像数据吧~