1、返回顶部按钮

减少插件的使用，利用 animate 和 scrollTop 来实现返回顶部的动画：

```js
// Back to top

$('a.top').click(function () {

$(document.body).animate({scrollTop: 0}, 800);

return false;

});
```


2、自动修改破损图像

前端显示图像破损是一个比较常见的事情，为避免这类用户体验度不友好的事情发生，你可以用一个不易被替换的图像来代替它们。添加下面这段代码可以节省很多麻烦：

```js
$('img').on('error', function () {

$(this).prop('src', 'img/broken.png');

});
```



3、鼠标悬停 (hover) 切换 class 属性

假如当用户鼠标悬停在一个可点击的元素上时，你希望改变其效果，下面这段代码可以在其悬停在元素上时添加 class 属性，当用户鼠标离开时，则自动取消该 class 属性：

```js
$('.btn').hover(function () {

$(this).addClass('hover');

}, function () {

$(this).removeClass('hover');

});
```

你只需要添加必要的 CSS 代码即可。如果你想要更简洁的代码，可以使用 toggleClass 方法：

```js
$('.btn').hover(function () {

$(this).toggleClass('hover');

});
```

注：上述效果的实现，直接使用 CSS 可能是更好的解决方案，但你仍然有必要时还是要用该方法。



4、切换 fade/slide

fade 和 slide 是我们在 jQuery 中经常使用的动画效果，它们可以使元素显示效果更好。但是如果你希望元素显示时使用第一种效果，而消失时使用第二种效果，则可以这么实现：

```js
// Fade

$('.btn').click(function () {

$('.element').fadeToggle('slow');

});

// Toggle

$('.btn').click(function () {

$('.element').slideToggle('slow');

});
```



5、简单的手风琴效果

对于手风琴效果，可以通过下述代码，快速简洁的实现：

```js
// Close all panels

$('#accordion').find('.content').hide();

// Accordion

$('#accordion').find('.accordion-header').click(function () {

var next = $(this).next();

next.slideToggle('fast');

$('.content').not(next).slideUp('fast');

return false;

});
```