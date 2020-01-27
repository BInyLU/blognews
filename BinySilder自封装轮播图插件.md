### 使用原生JavaScript封装一个轮播图插件

> `BinySilder.js`是一款原生`JavaScript`语言封装的轮播图插件，它以最少的结构与样式代码快速生成轮播图，并且能自定义设置轮播图进出动画，是一款轻量的动画轮播图插件。

### 示例插件初始化：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />
		<!--引入动画库 animation.css，必须把动画库放在此处-->
		<link rel="stylesheet" type="text/css" href="animation.css" />
	</head>
	<body>
		<!--要放置轮播图的盒子-->
		<div class="BinySilderDemo"></div>
		
		<!--引入轮播图插件 BinySilder.js-->
		<script src="BinySilder.js" type="text/javascript" charset="utf-8"></script>
		<script type="text/javascript">
			//轮播图片
            var BinySilderImg = [
                'https://photo.scol.com.cn/items/201911/19110310242748200010DFE2.jpg',
                'https://photo.scol.com.cn/items/201911/191113090348628000111B8D.jpg',
                'https://photo.scol.com.cn/items/201911/191113090212018000111B8C.jpg'
            ];

            //初始化 (简写版)
            newBinySilder(BinySilderImg, {
                SilderId: '.BinySilderDemo' //要放置轮播图的盒子ID
            });
		</script>
	</body>
</html>
```



**最多可调配参数（扩展参数版）：**

```javascript
newBinySilder(BinySilderImg, {
	SilderId: '.BinySilderDemo', //要放置轮播图的盒子ID
	SilderTimes: 3, //轮播图每几秒下一张
	SilderWidth: '1000px', //轮播图盒子宽度
	SilderHight: '500px', //轮播图盒子高度
	SilderLeftAmin: 'bounceInLeft', //上一张动画类名（Animation.css）
	SilderLeftAminTime: '.8s', //上一张动画做的时长
	SilderRightAmin: 'bounceInRight', //下一张动画类名（Animation.css）
	SilderRightAminTime: '.8s', //下一张动画做的时长
	SilderHideAmin: 'fadeOut', //动画消失类名，注意只能用带Out的类名（Animation.css）
	SilderHideAminTime: '.5s' //动画消失做的时长
},function(){
	//用户自定义回调事件
	alert('初始化newBinySilder.js');
});
```



### 扩展参数：

| 属性：                | 属性值类型： | 备注：                                               |
| --------------------- | ------------ | ---------------------------------------------------- |
| `SilderId`            | class / id   | 要放置轮播图的盒子类名或者ID                         |
| `SilderTimes`         | number       | 轮播图计时下一张前秒数                               |
| `SilderWidth`         | string       | 轮播图盒子宽度                                       |
| `SilderHight`         | string       | 轮播图盒子高度                                       |
| `SilderLeftAmin`      | class / id   | 上一张动画类名（Animation.css）                      |
| `SilderLeftAminTime`  | string       | 上一张动画做的时长                                   |
| `SilderRightAmin`     | class / id   | 下一张动画类名（Animation.css）                      |
| `SilderRightAminTime` | string       | 下一张动画做的时长                                   |
| `SilderHideAmin`      | class / id   | 动画消失类名，注意只能用带Out的类名（Animation.css） |
| `SilderHideAminTime`  | string       | 动画消失做的时长                                     |



### 动画类名（Animation.css）：

| Class Name        |                    |                     |                      |
| ----------------- | ------------------ | ------------------- | -------------------- |
| `bounce`          | `flash`            | `pulse`             | `rubberBand`         |
| `shake`           | `headShake`        | `swing`             | `tada`               |
| `wobble`          | `jello`            | `bounceIn`          | `bounceInDown`       |
| `bounceInLeft`    | `bounceInRight`    | `bounceInUp`        | `bounceOut`          |
| `bounceOutDown`   | `bounceOutLeft`    | `bounceOutRight`    | `bounceOutUp`        |
| `fadeIn`          | `fadeInDown`       | `fadeInDownBig`     | `fadeInLeft`         |
| `fadeInLeftBig`   | `fadeInRight`      | `fadeInRightBig`    | `fadeInUp`           |
| `fadeInUpBig`     | `fadeOut`          | `fadeOutDown`       | `fadeOutDownBig`     |
| `fadeOutLeft`     | `fadeOutLeftBig`   | `fadeOutRight`      | `fadeOutRightBig`    |
| `fadeOutUp`       | `fadeOutUpBig`     | `flipInX`           | `flipInY`            |
| `flipOutX`        | `flipOutY`         | `lightSpeedIn`      | `lightSpeedOut`      |
| `rotateIn`        | `rotateInDownLeft` | `rotateInDownRight` | `rotateInUpLeft`     |
| `rotateInUpRight` | `rotateOut`        | `rotateOutDownLeft` | `rotateOutDownRight` |
| `rotateOutUpLeft` | `rotateOutUpRight` | `hinge`             | `jackInTheBox`       |
| `rollIn`          | `rollOut`          | `zoomIn`            | `zoomInDown`         |
| `zoomInLeft`      | `zoomInRight`      | `zoomInUp`          | `zoomOut`            |
| `zoomOutDown`     | `zoomOutLeft`      | `zoomOutRight`      | `zoomOutUp`          |
| `slideInDown`     | `slideInLeft`      | `slideInRight`      | `slideInUp`          |
| `slideOutDown`    | `slideOutLeft`     | `slideOutRight`     | `slideOutUp`         |
| `heartBeat`       |                    |                     |                      |