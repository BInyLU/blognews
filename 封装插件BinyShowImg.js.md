### **使用原生JavaScript写一个封装插件**

> **`BinyShowImg.js` 是一款原生封装的预览图插件，点击网站上的图片可放大预览。**
>
> 最近一直在学写规范代码，然后打算用原生`JavaScript`写类似于面向对象编程的写法，练习类的封装继承，封装一个插件，所以写了一个`BinyShowImg.js`插件来练手。
>
> 目前功能只可放大预览，后续会继续加上一张/下一张功能，预览图标题显示功能等。
>
> 后面还会逐一推出轮播图插件、提示框插件等，均为个人手写封装；目前已经在优化状态，亮点会很多，很快推出。

**使用例子：**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>BinyShowImg.js 插件说明</title>
	</head>
	<body>
		<img src='https://photo.scol.com.cn/items/201911/19110310242748200010DFE2.jpg'/>
		<img src='https://photo.scol.com.cn/items/201911/191113090348628000111B8D.jpg'/>
		<img src='https://photo.scol.com.cn/items/201911/191113090212018000111B8C.jpg'/>
		<img src='https://photo.scol.com.cn/items/201911/191113090530732000111B8E.jpg'/>
	</body>
    <script src="https://ebuxsf.coding.io/js/BinyShowImg.js" ></script>
    <script>
        
        /**
         * @param {Object} imgBoxId  预览图ID (自由设置ID)
         * @param {Object} callback  用户自定义回调事件
         */
        
        //newShowSmallImgBox(‘imgBoxId’);
        //newShowSmallImgBox(‘imgBoxId’，function(){ }); 
        
        newShowSmallImgBox(); //初始化 BinyShowImg.js 插件
        
    </script>
</html>
```

**`BinyShowImg.js`插件在线链接：**

[https://ebuxsf.coding.io/js/BinyShowImg.js](https://ebuxsf.coding.io/js/BinyShowImg.js)

### **插件主要设计过程：**

- 这款插件我封装使用了几个方法，其中就有获取多元素节点和获取单元素节点。这也是对`jQuery`的一个封装原理小理解，使用方法很类似`jQuery`。（当然，我和`jQuery`的封装写法差得远了）。`$$(elemt)`获取多元素节点，`$(elemt)`获取单元素节点。

  ```javascript
  //获取元素节点的封装技巧
  function $$(elemt) {
  	if(document.querySelectorAll(elemt)){
  		return document.querySelectorAll(elemt)
  	}
  }
  
  function $(elemt) {
  	if(document.querySelector(elemt)){
  		return document.querySelector(elemt)
  	}
  }
  ```

- 然后就是我对`prototype`原型的理解，在典型的面向对象的语言中，如`java`，都存在类`class`的概念，类就是对象的模板，对象就是类的实例。但是在`Javascript`语言体系中，是不存在类`Class`的概念的，`javascript`中不是基于 ‘ 类的 ’ ，而是通过构造函数`constructor`和原型链`prototype chains`实现的。但是在`ES6`中提供了更接近传统语言的写法，引入了`Class`（类）这个概念，作为对象的模板。通过`class`关键字，可以定义类。基本上，`ES6`的`class`可以看作只是一个语法糖，它的绝大部分功能，`ES5`都可以做到，新的`class`写法只是让原型对象的写法更加清晰、更像面向对象编程的语法而已。这里说不完 -。-，以后会单独出一篇文章。

  ```javascript
  //简单的原型封装，构造函数
  function Person(name,height){
      this.name = name;
      this.height = height;
  }
  Person.prototype.hobby = function(){
      return 'watching movies';
  }
  var boy = new Person('keith',180);
  var girl = new Person('rascal',153);
  
  console.log(boy.name); //'keith'
  console.log(girl.name); //'rascal'
  
  console.log(boy.hobby === girl.hobby); //true
  ```

- 其实为什么要代码规范化，模块化，是因为多人合作写代码时，需要把代码细化，并工程化，这样才能使代码更加具有扩展性，这样的代码更是一线互联网公司的标准规范。从代码方面说，那就是对内存分配的调用更加好，更加优化，带来好的用户体验。

- 当然，以上皆为个人看法，如有不对的地方，可评论指出，谢谢。
