打开网页上的第一眼，让人感觉耳目一新，这是在对用户体验下功夫，用户也会对你的网站产生粘合度，同时带动 “ 佛性SEO “ 大法，进而推广你的网站文章，就这样一举多得。

实现思路：渐入的效果其实是Css动画效果做的，写在body里，当每次打开一个新的页面，body就会开始做动画，动画是透明度，从0至1的过程。
  

```js
body {
animation: fadeIn .5s linear 1;
-webkit-animation: fadeIn .5s linear 1;
-moz-animation: fadeIn .5s linear 1;
-o-animation: fadeIn .5s linear 1;
-ms-animation: fadeIn .5s linear 1;
}
@keyframes fadeIn {
0% {
opacity: 0;
}
100% {
opacity: 1;
}
}

@-webkit-keyframes fadeIn {
0% {
opacity: 0;
}
100% {
opacity: 1;
}
}

@-moz-keyframes fadeIn {
0% {
opacity: 0;
}
100% {
opacity: 1;
}
}

@-o-keyframes fadeIn {
0% {
opacity: 0;
}
100% {
opacity: 1;
}
}

@-ms-keyframes fadeIn {
0% {
opacity: 0;
}
100% {
opacity: 1;
}
}
```