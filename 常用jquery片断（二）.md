##### **1、检测Internet Explorer版本**

当涉及到CSS设计时，对开发者和设计者而言Internet Explorer一直是个问题。尽管IE6的黑暗时代已经过去，IE也越来越不流行，它始终是一个能够容易检测的好东西。当然了，下面的代码也能用于检测别的浏览器。

 

```
$(document).ready(function() {

      if (navigator.userAgent.match(/msie/i) ){

        alert('I am an old fashioned Internet Explorer');

      }

});
```

 

##### **2、平稳滑动到页面顶部**

```
$("a[href='#top']").click(function() {

  $("html, body").animate({ scrollTop: 0 }, "slow");

  return false;

});
```

 

##### **3、固定在顶部**

它允许一个元素固定在顶部。对导航按钮、工具栏或重要信息框是超级有用的。

 

```
$(function(){

var $win = $(window)

var $nav = $('.mytoolbar');

var navTop = $('.mytoolbar').length && $('.mytoolbar').offset().top;

var isFixed=0;

 

processScroll();

$win.on('scroll', processScroll);

 

function processScroll() {

var i, scrollTop = $win.scrollTop();

if (scrollTop >= navTop && !isFixed) { 

isFixed = 1;

$nav.addClass('subnav-fixed');

} else if (scrollTop <= navTop && isFixed) {

isFixed = 0;

 　　　　　 $nav.removeClass('subnav-fixed');

}

}
```

 

##### **4、用其他内容取代html标志**

```
$('li').replaceWith(function(){

  return $("<div />").append($(this).contents());

});
```

 

##### **5、检测视窗宽度**

现在移动设备比过时的电脑更普遍，能够方便去检测一个更小的视窗宽度会很有帮助。幸运的是，用jQuery来做超级简单。

 

```
var responsive_viewport = $(window).width();
```

 

##### **6、自动定位并修复损坏图片**

如果你的站点比较大而且已经在线运行了好多年，你或多或少会遇到界面上某个地方有损坏的图片。这个有用的函数能够帮助检测损坏图片并用你中意的图片替换它，并会将此问题通知给访客。

 

```
$('img').error(function(){

$(this).attr('src', 'img/broken.png');

});
```

 

##### **7、检测复制、粘贴和剪切的操作**

使用jQuery可以很容易去根据你的要求去检测复制、粘贴和剪切的操作。

 

```
/*复制*/

$("#textA").bind('copy', function() {

    $('span').text('copy behaviour detected!')

}); 

 

/*粘贴*/

$("#textA").bind('paste', function() {

    $('span').text('paste behaviour detected!')

}); 

 

/*剪切*/

$("#textA").bind('cut', function() {

    $('span').text('cut behaviour detected!')

});
```

 

##### **8、遇到外部链接自动添加target=”_blank”的属性**

当链接到外部站点时，你可能使用target=”_blank”的属性去在新界面中打开站点。下面这段代码将会检测是否链接是外链，如果是，会自动添加一个target=”_blank”属性。

 

```
var root = location.protocol + '//' + location.host;

$('a').not(':contains(root)').click(function(){

    this.target = "_blank";

});
```

 

##### **9、在文本或密码输入时禁止空格键**

在很多表格领域都不需要空格键，例如，电子邮件，用户名，密码等等等。这里是一个简单的技巧可以用于在选定输入中禁止空格键。

 

```
$('input.nospace').keydown(function(e) {

if (e.keyCode == 32) {

return false;

}

});
```

 

##### 10、浏览器识别，手机访问就跳转到wap端，pc访问跳转到pc端

```
<script>
//判断是否手机端访问
    var userAgentInfo = navigator.userAgent.toLowerCase();
    var Agents = ["android", "iphone",
                "symbianos", "windows phone",
                "ipad", "ipod"];
    var ly=document.referrer;  //返回导航到当前网页的超链接所在网页的URL
    for (var v = 0; v < Agents.length; v++) {
        if (userAgentInfo.indexOf(Agents[v]) >= 0&&(ly==""||ly==null)) {
            this.location.href='http://m.***.com';  //wap端地址
        }
    }
</script>
```



##### **11、JS倒计时跳转到指定页面**

 

```
<script type="text/javascript">
window.onload = function(){
var secObj = document.getElementById('second');
var sec = secObj.innerHTML;
var timer = null;
timer = setInterval(function(){
sec--;
secObj.innerHTML=sec;
if(sec == 0){
window.location.href = "***";  //跳转地址
}
},1000);
}
</script>
```

