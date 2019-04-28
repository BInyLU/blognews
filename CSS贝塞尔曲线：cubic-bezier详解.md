在animation和transition两个属性中，cubic-bezier是控制变化的速度曲线，下面我们来了解下什么是cubic-bezier。

cubic-bezier称为三次贝塞尔曲线，主要是生成速度曲线的函数，规定是`cubic-bezier(<x1>,<y1>,<x2>,<y2>)` .

 

[![20171213181108192.png](http://binylu.cn/content/uploadfile/201812/70631545334808.png)](http://binylu.cn/content/uploadfile/201812/70631545334808.png)[![20171213181125110.jpg](http://binylu.cn/content/uploadfile/201812/6e901545334815.jpg)](http://binylu.cn/content/uploadfile/201812/6e901545334815.jpg)

 

从上图中我们可以看到，cubic-bezier有四个点： 两个默认的，即：P0(0,0)，P3(1,1)； 两个控制点，即：P1(x1,y1)，P2(x2,y2) 注：X轴的范围是0~1，超出cubic-bezier将失效，Y轴的取值没有规定，但是也不宜过大。 我们只要调整两个控制点P1和P2的坐标，最后形成的曲线就是动画曲线。

 

下面以我们常用到的曲线为例：

1、`linear`，即`cubic-bezier(0,0,1,1) / cubic-bezier(1,1,0,0)`，左边cubic-bezier曲线，右边浏览器动画曲线展示。下同。

[![20171213181316960.png](http://binylu.cn/content/uploadfile/201812/61151545334822.png)](http://binylu.cn/content/uploadfile/201812/61151545334822.png) ![这里写图片描述](https://img-blog.csdn.net/20171213185917767?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2puZjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

2、`ease`，即`cubic-bezier(0.25,0.1,0.25,1)`。

[![20171213182911681.png](http://binylu.cn/content/uploadfile/201812/7c301545334833.png)](http://binylu.cn/content/uploadfile/201812/7c301545334833.png) ![这里写图片描述](https://img-blog.csdn.net/20171213185940589?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2puZjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

3、`ease-in`，即`cubic-bezier(0.42,0,1,1)`。

[![20171213182947124.png](http://binylu.cn/content/uploadfile/201812/f5cb1545334841.png)](http://binylu.cn/content/uploadfile/201812/f5cb1545334841.png) ![这里写图片描述](https://img-blog.csdn.net/20171213190000030?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2puZjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

4、`ease-out`，即`cubic-bezier(0,0,0.58,1)`。

[![20171213183007150.png](http://binylu.cn/content/uploadfile/201812/14031545334853.png)](http://binylu.cn/content/uploadfile/201812/14031545334853.png) ![这里写图片描述](https://img-blog.csdn.net/20171213190019631?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2puZjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

5、`ease-in-out`，即`这里写代码片`。

[![20171213183128947.png](http://binylu.cn/content/uploadfile/201812/31bf1545334862.png)](http://binylu.cn/content/uploadfile/201812/31bf1545334862.png) ![这里写图片描述](https://img-blog.csdn.net/20171213190038356?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2puZjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

6、随机示例1：`cubic-bezier(0.68,-0.55,0.27,0.55)`。

[![20171213183528714.png](http://binylu.cn/content/uploadfile/201812/1e9b1545334868.png)](http://binylu.cn/content/uploadfile/201812/1e9b1545334868.png) ![这里写图片描述](https://img-blog.csdn.net/20171213190057097?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2puZjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

7、随机示例2：`cubic-bezier(0.17,0.86,0.73,0.14)`。

[![20171213185808868.png](http://binylu.cn/content/uploadfile/201812/aa661545334876.png)](http://binylu.cn/content/uploadfile/201812/aa661545334876.png) ![这里写图片描述](https://img-blog.csdn.net/20171213190114949?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2puZjAxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

更多缓动函数请查看： <https://easings.net/zh-cn#>