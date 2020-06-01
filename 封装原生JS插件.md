# 封装原生JS插件

作为一个前端er，如果不会写一个小插件，都不好意思说自己是混前端界的。写还不能依赖jquery之类的工具库，否则装得不够高端。那么，如何才能装起来让自己看起来逼格更高呢？当然是利用js纯原生的写法啦。以前一直说，掌握了js原生，就基本上可以解决前端的所有脚本交互工作了，这话大体上是有些浮夸了。不过，也从侧面说明了原生js在前端中占着多么重要的一面。好了。废话不多说。咱们就来看一下怎么去做一个自己的js插件吧。

### 插件的需求

我们写代码，并不是所有的业务或者逻辑代码都要抽出来复用。首先，我们得看一下是否需要将一部分经常重复的代码抽象出来，写到一个单独的文件中为以后再次使用。再看一下我们的业务逻辑是否可以为团队服务。
 插件不是随手就写成的，而是根据自己业务逻辑进行抽象。没有放之四海而皆准的插件，只有对插件，之所以叫做插件，那么就是开箱即用，或者我们只要添加一些配置参数就可以达到我们需要的结果。如果都符合了这些情况，我们才去考虑做一个插件。

### 插件封装的条件

一个可复用的插件需要满足以下条件：

1. 插件自身的作用域与用户当前的作用域相互独立，也就是插件内部的私有变量不能影响使用者的环境变量；
2. 插件需具备默认设置参数；
3. 插件除了具备已实现的基本功能外，需提供部分API，使用者可以通过该API修改插件功能的默认参数，从而实现用户自定义插件效果；
4. 插件支持链式调用；
5. 插件需提供监听入口，及针对指定元素进行监听，使得该元素与插件响应达到插件效果。

关于插件封装的条件，可以查看一篇文章：[原生JavaScript插件编写指南](http://geocld.github.io/2016/03/10/javascript_plugin/)
 而我想要说明的是，如何一步一步地实现我的插件封装。所以，我会先从简单的方法函数来做起。

### 插件的外包装

#### 用函数包装

所谓插件，其实就是封装在一个闭包中的一种函数集。我记得刚开始写js的时候，我是这样干的，将我想要的逻辑，写成一个函数，然后再根据不同需要传入不同的参数就可以了。
 比如，我想实现两个数字相加的方法：

```javascript
function add(n1,n2) {
    return n1 + n2;
}
// 调用
add(1,2)
// 输出：3
```

这就是我们要的功能的简单实现。如果仅仅只不过实现这么简单的逻辑，那已经可以了，没必要弄一些花里胡哨的东西。js函数本身就可以解决绝大多数的问题。不过我们在实际工作与应用中，一般情况的需求都是比较复杂得多。
 如果这时，产品来跟你说，我不仅需要两个数相加的，我还要相减，相乘，相除，求余等等功能。这时候，我们怎么办呢？
 当然，你会想，这有什么难的。直接将这堆函数都写出来不就完了。然后都放在一个js文件里面。需要的时候，就调用它就好了。

```javascript
// 加
function add(n1,n2) {
    return n1 + n2;
}
// 减
function sub(n1,n2) {
    return n1 - n2;
}
// 乘
function mul(n1,n2) {
    return n1 * n2;
}
// 除
function div(n1,n2) {
    return n1 / n2;
}
// 求余
function sur(n1,n2) {
    return n1 % n2;
}
```

OK，现在已经实现我们所需要的所有功能。并且我们也把这些函数都写到一个js里面了。如果是一个人在用，那么可以很清楚知道自己是否已经定义了什么，并且知道自己写了什么内容，我在哪个页面需要，那么就直接引入这个js文件就可以搞定了。
 不过，如果是两个人以上的团队，或者你与别人一起协作写代码，这时候，另一个人并不知道你是否写了add方法，这时他也定义了同样的add方法。那么你们之间就会产生**命名冲突**，一般称之为变量的 **全局污染**

#### 用全局对象包装

为了解决这种全局变量污染的问题。这时，我们可以定义一个js对象来接收我们这些工具函数。

```javascript
var plugin = {
    add: function(n1,n2){...},//加
    sub: function(n1,n2){...},//减
    mul: function(n1,n2){...},//乘
    div: function(n1,n2){...},//除
    sur: function(n1,n2){...} //余
}
// 调用
plugin.add(1,2)
```

上面的方式，约定好此插件名为`plugin`，让团队成员都要遵守命名规则，在一定程度上已经解决了全局污染的问题。在团队协作中只要约定好命名规则了，告知其它同学即可以。当然不排除有个别人，接手你的项目，并不知道此全局变量已经定义，则他又定义了一次并赋值，这时，就会把你的对象覆盖掉。当然，可能你会这么干来解决掉命名冲突问题：

```javascript
if(!plugin){ //这里的if条件也可以用： (typeof plugin == 'undefined')
    var plugin = {
        // 以此写你的函数逻辑
    }
}
```

或者也可以这样写：

```javascript
var plugin;
if(!plugin){
    plugin = {
        // ...
    }
}
```

这样子，就不会存在命名上的冲突了。

> 也许有同学会疑问，为什么可以在此声明plugin变量？实际上js的解释执行，会把所有声明都提前。如果一个变量已经声明过，后面如果不是在函数内声明的，则是没有影响的。所以，就算在别的地方声明过var plugin，我同样也以可以在这里再次声明一次。关于声明的相关资料可以看阮一锋的[如何判断Javascript对象是否存在](http://www.ruanyifeng.com/blog/2011/05/how_to_judge_the_existence_of_a_global_object_in_javascript.html)。

基本上，这就可以算是一个插件了。解决了全局污染问题，方法函数可以抽出来放到一单独的文件里面去。

#### 利用闭包包装

上面的例子，虽然可以实现了插件的基本上的功能。不过我们的plugin对象，是定义在全局域里面的。我们知道，js变量的调用，从全局作用域上找查的速度会比在私有作用域里面慢得多得多。所以，我们最好将插件逻辑写在一个私有作用域中。
 实现私有作用域，最好的办法就是使用闭包。可以把插件当做一个函数，插件内部的变量及函数的私有变量，为了在调用插件后依旧能使用其功能，闭包的作用就是延长函数(插件)内部变量的生命周期，使得插件函数可以重复调用，而不影响用户自身作用域。
 故需将插件的所有功能写在一个立即执行函数中：

```javascript
;(function(global,undefined) {
    var plugin = {
        add: function(n1,n2){...}
        ...
    }
    // 最后将插件对象暴露给全局对象
    'plugin' in global && (global.plugin = plugin);
})(window);
```

对上面的代码段传参问题进行解释一下：

1. 在定义插件之前添加一个分号，可以解决js合并时可能会产生的错误问题；
2. undefined在老一辈的浏览器是不被支持的，直接使用会报错，js框架要考虑到兼容性，因此增加一个形参undefined，就算有人把外面的 `undefined` 定义了，里面的 undefined 依然不受影响；
3. 把window对象作为参数传入，是避免了函数执行的时候到外部去查找。

其实，我们觉得直接传window对象进去，我觉得还是不太妥当。我们并不确定我们的插件就一定用于浏览器上，也有可能使用在一些非浏览端上。所以我们还可以这么干，我们不传参数，直接取当前的全局this对象为作顶级对象用。

```javascript
;(function(global,undefined) {
    "use strict" //使用js严格模式检查，使语法更规范
    var _global;
    var plugin = {
        add: function(n1,n2){...}
        ...
    }
    // 最后将插件对象暴露给全局对象
    _global = (function(){ return this || (0, eval)('this'); }());
    !('plugin' in _global) && (_global.plugin = plugin);
}());
```

如此，我们不需要传入任何参数，并且解决了插件对环境的依事性。如此我们的插件可以在任何宿主环境上运行了。

> 上面的代码段中有段奇怪的表达式：`(0, eval)('this')`，实际上`(0,eval)`是一个表达式，这个表达式执行之后的结果就是`eval`这一句相当于执行`eval('this')`的意思，详细解释看此篇：[(0,eval)('this')释义](https://www.jianshu.com/p/205a4033010a)或者看一下这篇[(0,eval)('this')](http://www.cnblogs.com/qianlegeqian/p/3950044.html)

关于立即自执行函数，有两种写法：

```javascript
// 写法一
(function(){})()

//写法二
(function(){}())
```

上面的两种写法是没有区别的。都是正确的写法。个人建议使用第二种写法。这样子更像一个整体。

> **附加一点知识：**
>  js里面`()`括号就是将代码结构变成表达式，被包在`()`里面的变成了表达式之后，则就会立即执行，js中将一段代码变成表达式有很多种方式，比如：

```javascript
void function(){...}();
// 或者
!function foo(){...}();
// 或者
+function foot(){...}();
```

当然，我们不推荐你这么用。而且乱用可能会产生一些歧义。

到这一步，我们的插件的基础结构就已经算是完整的了。

#### 使用模块化的规范包装

虽然上面的包装基本上已经算是ok了的。但是如果是多个人一起开发一个大型的插件，这时我们要该怎么办呢？多人合作，肯定会产生多个文件，每个人负责一个小功能，那么如何才能将所有人开发的代码集合起来呢？这是一个讨厌的问题。要实现协作开发插件，必须具备如下条件：

- 每功能互相之间的依赖必须要明确，则必须严格按照依赖的顺序进行合并或者加载
- 每个子功能分别都要是一个闭包，并且将公共的接口暴露到共享域也即是一个被主函数暴露的公共对象

关键如何实现，有很多种办法。最笨的办法就是按顺序加载js

```html
<script type="text/javascript" src="part1.js"></script>
<script type="text/javascript" src="part2.js"></script>
<script type="text/javascript" src="part3.js"></script>
...
<script type="text/javascript" src="main.js"></script>
```

但是不推荐这么做，这样做与我们所追求的插件的封装性相背。
 不过现在前端界有一堆流行的模块加载器，比如[require](https://github.com/requirejs/requirejs)、[seajs](https://github.com/seajs/seajs)，或者也可以像类似于Node的方式进行加载，不过在浏览器端，我们还得利用打包器来实现模块加载，比如[browserify](https://github.com/substack/node-browserify/)。不过在此不谈如何进行模块化打包或者加载的问题，如有问题的同学可以去上面的链接上看文档学习。
 为了实现插件的模块化并且让我们的插件也是一个模块，我们就得让我们的插件也实现模块化的机制。
 我们实际上，只要判断是否存在加载器，如果存在加载器，我们就使用加载器，如果不存在加载器。我们就使用顶级域对象。

```javascript
if (typeof module !== "undefined" && module.exports) {
    module.exports = plugin;
} else if (typeof define === "function" && define.amd) {
    define(function(){return plugin;});
} else {
    _globals.plugin = plugin;
}
```

这样子我们的完整的插件的样子应该是这样子的：

```javascript
// plugin.js
;(function(undefined) {
    "use strict"
    var _global;
    var plugin = {
        add: function(n1,n2){ return n1 + n2; },//加
        sub: function(n1,n2){ return n1 - n2; },//减
        mul: function(n1,n2){ return n1 * n2; },//乘
        div: function(n1,n2){ return n1 / n2; },//除
        sur: function(n1,n2){ return n1 % n2; } //余
    }
    // 最后将插件对象暴露给全局对象
    _global = (function(){ return this || (0, eval)('this'); }());
    if (typeof module !== "undefined" && module.exports) {
        module.exports = plugin;
    } else if (typeof define === "function" && define.amd) {
        define(function(){return plugin;});
    } else {
        !('plugin' in _global) && (_global.plugin = plugin);
    }
}());
```

我们引入了插件之后，则可以直接使用plugin对象。

```javascript
with(plugin){
    console.log(add(2,1)) // 3
    console.log(sub(2,1)) // 1
    console.log(mul(2,1)) // 2
    console.log(div(2,1)) // 2
    console.log(sur(2,1)) // 0
}
```

### 插件的API

#### 插件的默认参数

我们知道，函数是可以设置默认参数这种说法，而不管我们是否传有参数，我们都应该返回一个值以告诉用户我做了怎样的处理，比如：

```javascript
function add(param){
    var args = !!param ? Array.prototype.slice.call(arguments) : [];
    return args.reduce(function(pre,cur){
        return pre + cur;
    }, 0);
}

console.log(add()) //不传参，结果输出0，则这里已经设置了默认了参数为空数组
console.log(add(1,2,3,4,5)) //传参，结果输出15
```

则作为一个健壮的js插件，我们应该把一些基本的状态参数添加到我们需要的插件上去。
 假设还是上面的加减乘除余的需求，我们如何实现插件的默认参数呢？道理其实是一样的。

```javascript
// plugin.js
;(function(undefined) {
    "use strict"
    var _global;

    function result(args,fn){
        var argsArr = Array.prototype.slice.call(args);
        if(argsArr.length > 0){
            return argsArr.reduce(fn);
        } else {
            return 0;
        }
    }
    var plugin = {
        add: function(){
            return result(arguments,function(pre,cur){
                return pre + cur;
            });
        },//加
        sub: function(){
            return result(arguments,function(pre,cur){
                return pre - cur;
            });
        },//减
        mul: function(){
            return result(arguments,function(pre,cur){
                return pre * cur;
            });
        },//乘
        div: function(){
            return result(arguments,function(pre,cur){
                return pre / cur;
            });
        },//除
        sur: function(){
            return result(arguments,function(pre,cur){
                return pre % cur;
            });
        } //余
    }

    // 最后将插件对象暴露给全局对象
    _global = (function(){ return this || (0, eval)('this'); }());
    if (typeof module !== "undefined" && module.exports) {
        module.exports = plugin;
    } else if (typeof define === "function" && define.amd) {
        define(function(){return plugin;});
    } else {
        !('plugin' in _global) && (_global.plugin = plugin);
    }
}());

// 输出结果为：
with(plugin){
    console.log(add()); // 0
    console.log(sub()); // 0
    console.log(mul()); // 0
    console.log(div()); // 0
    console.log(sur()); // 0

    console.log(add(2,1)); // 3
    console.log(sub(2,1)); // 1
    console.log(mul(2,1)); // 2
    console.log(div(2,1)); // 2
    console.log(sur(2,1)); // 0
}
```

实际上，插件都有自己的默认参数，就以我们最为常见的表单验证插件为例：[validate.js](https://github.com/rickharrison/validate.js)

```javascript
(function(window, document, undefined) {
    // 插件的默认参数
    var defaults = {
        messages: {
            required: 'The %s field is required.',
            matches: 'The %s field does not match the %s field.',
            "default": 'The %s field is still set to default, please change.',
            valid_email: 'The %s field must contain a valid email address.',
            valid_emails: 'The %s field must contain all valid email addresses.',
            min_length: 'The %s field must be at least %s characters in length.',
            max_length: 'The %s field must not exceed %s characters in length.',
            exact_length: 'The %s field must be exactly %s characters in length.',
            greater_than: 'The %s field must contain a number greater than %s.',
            less_than: 'The %s field must contain a number less than %s.',
            alpha: 'The %s field must only contain alphabetical characters.',
            alpha_numeric: 'The %s field must only contain alpha-numeric characters.',
            alpha_dash: 'The %s field must only contain alpha-numeric characters, underscores, and dashes.',
            numeric: 'The %s field must contain only numbers.',
            integer: 'The %s field must contain an integer.',
            decimal: 'The %s field must contain a decimal number.',
            is_natural: 'The %s field must contain only positive numbers.',
            is_natural_no_zero: 'The %s field must contain a number greater than zero.',
            valid_ip: 'The %s field must contain a valid IP.',
            valid_base64: 'The %s field must contain a base64 string.',
            valid_credit_card: 'The %s field must contain a valid credit card number.',
            is_file_type: 'The %s field must contain only %s files.',
            valid_url: 'The %s field must contain a valid URL.',
            greater_than_date: 'The %s field must contain a more recent date than %s.',
            less_than_date: 'The %s field must contain an older date than %s.',
            greater_than_or_equal_date: 'The %s field must contain a date that\'s at least as recent as %s.',
            less_than_or_equal_date: 'The %s field must contain a date that\'s %s or older.'
        },
        callback: function(errors) {

        }
    };

    var ruleRegex = /^(.+?)\[(.+)\]$/,
        numericRegex = /^[0-9]+$/,
        integerRegex = /^\-?[0-9]+$/,
        decimalRegex = /^\-?[0-9]*\.?[0-9]+$/,
        emailRegex = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/,
        alphaRegex = /^[a-z]+$/i,
        alphaNumericRegex = /^[a-z0-9]+$/i,
        alphaDashRegex = /^[a-z0-9_\-]+$/i,
        naturalRegex = /^[0-9]+$/i,
        naturalNoZeroRegex = /^[1-9][0-9]*$/i,
        ipRegex = /^((25[0-5]|2[0-4][0-9]|1[0-9]{2}|[0-9]{1,2})\.){3}(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[0-9]{1,2})$/i,
        base64Regex = /[^a-zA-Z0-9\/\+=]/i,
        numericDashRegex = /^[\d\-\s]+$/,
        urlRegex = /^((http|https):\/\/(\w+:{0,1}\w*@)?(\S+)|)(:[0-9]+)?(\/|\/([\w#!:.?+=&%@!\-\/]))?$/,
        dateRegex = /\d{4}-\d{1,2}-\d{1,2}/;

    ... //省略后面的代码
})(window,document);
/*
 * Export as a CommonJS module
 */
if (typeof module !== 'undefined' && module.exports) {
    module.exports = FormValidator;
}
```

当然，参数既然是默认的，那就意味着我们可以随意修改参数以达到我们的需求。插件本身的意义就在于具有复用性。
 如表单验证插件，则就可以new一个对象的时候，修改我们的默认参数：

```javascript
var validator = new FormValidator('example_form', [{
    name: 'req',
    display: 'required',
    rules: 'required'
}, {
    name: 'alphanumeric',
    rules: 'alpha_numeric'
}, {
    name: 'password',
    rules: 'required'
}, {
    name: 'password_confirm',
    display: 'password confirmation',
    rules: 'required|matches[password]'
}, {
    name: 'email',
    rules: 'valid_email'
}, {
    name: 'minlength',
    display: 'min length',
    rules: 'min_length[8]'
}, {
    names: ['fname', 'lname'],
    rules: 'required|alpha'
}], function(errors) {
    if (errors.length > 0) {
        // Show the errors
    }
});
```

#### 插件的钩子

我们知道，设计一下插件，参数或者其逻辑肯定不是写死的，我们得像函数一样，得让用户提供自己的参数去实现用户的需求。则我们的插件需要提供一个修改默认参数的入口。
 如上面我们说的修改默认参数，实际上也是插件给我们提供的一个API。让我们的插件更加的灵活。如果大家对API不了解，可以百度一下[API](http://baike.baidu.com/link?url=KFl22zafFz19o4AEWL1JO58Pv7uXZaykWKVpsdztsNn6CDtuZSsw-TBj3Wj0SLQDv6FBOVSgWWHT2YkhkEq7Ea)
 通常我们用的js插件，实现的方式会有多种多样的。最简单的实现逻辑就是一个方法，或者一个js对象，又或者是一个构造函数等等。
 ** 然我们插件所谓的API，实际就是我们插件暴露出来的所有方法及属性。 **
 我们需求中，加减乘除余插件中，我们的API就是如下几个方法：

```javascript
...
var plugin = {
    add: function(n1,n2){ return n1 + n2; },
    sub: function(n1,n2){ return n1 - n2; },
    mul: function(n1,n2){ return n1 * n2; },
    div: function(n1,n2){ return n1 / n2; },
    sur: function(n1,n2){ return n1 % n2; } 
}
...
```

可以看到plubin暴露出来的方法则是如下几个API：

- add
- sub
- mul
- div
- sur

在插件的API中，我们常常将容易被修改和变动的方法或属性统称为**钩子(Hook)**，方法则直接叫**钩子函数**。这是一种形象生动的说法，就好像我们在一条绳子上放很多挂钩，我们可以按需要在上面挂东西。
 实际上，我们即知道插件可以像一条绳子上挂东西，也可以拿掉挂的东西。那么一个插件，实际上就是个形象上的**链**。不过我们上面的所有钩子都是挂在对象上的，用于实现链并不是很理想。

#### 插件的链式调用（利用当前对象）

插件并非都是能链式调用的，有些时候，我们只是用钩子来实现一个计算并返回结果，取得运算结果就可以了。但是有些时候，我们用钩子并不需要其返回结果。我们只利用其实现我们的业务逻辑，为了代码简洁与方便，我们常常将插件的调用按链式的方式进行调用。
 最常见的jquery的链式调用如下：

```javascript
$(<id>).show().css('color','red').width(100).height(100)....
```

那，如何才能将链式调用运用到我们的插件中去呢？假设我们上面的例子，如果是要按照plugin这个对象的链式进行调用，则可以将其业务结构改为：

```javascript
...
var plugin = {
    add: function(n1,n2){ return this; },
    sub: function(n1,n2){ return this; },
    mul: function(n1,n2){ return this; },
    div: function(n1,n2){ return this; },
    sur: function(n1,n2){ return this; } 
}
...
```

显示，我们只要将插件的当前对象this直接返回，则在下一下方法中，同样可以引用插件对象plugin的其它勾子方法。然后调用的时候就可以使用链式了。

```javascript
plugin.add().sub().mul().div().sur()  //如此调用显然没有任何实际意义
```

显然这样做并没有什么意义。我们这里的每一个钩子函数都只是用来计算并且获取返回值而已。而链式调用本身的意义是用来处理业务逻辑的。

#### 插件的链式调用（利用原型链）

JavaScript中，万物皆对象，所有对象都是继承自原型。JS在创建对象（不论是普通对象还是函数对象）的时候，都有一个叫做`__proto__`的内置属性，用于指向创建它的函数对象的原型对象prototype。关于原型问题，感兴趣的同学可以看这篇：[js原型链](https://www.jianshu.com/p/e2fd87a40dcc)
 在上面的需求中，我们可以将plugin对象改为原型的方式，则需要将plugin写成一个构造方法，我们将插件名换为`Calculate`避免因为Plugin大写的时候与Window对象中的API冲突。

```javascript
...
function Calculate(){}
Calculate.prototype.add = function(){return this;}
Calculate.prototype.sub = function(){return this;}
Calculate.prototype.mul = function(){return this;}
Calculate.prototype.div = function(){return this;}
Calculate.prototype.sur = function(){return this;}
...
```

当然，假设我们的插件是对初始化参数进行运算并只输出结果，我们可以稍微改一下：

```javascript
// plugin.js
// plugin.js
;(function(undefined) {
    "use strict"
    var _global;

    function result(args,type){
        var argsArr = Array.prototype.slice.call(args);
        if(argsArr.length == 0) return 0;
        switch(type) {
            case 1: return argsArr.reduce(function(p,c){return p + c;});
            case 2: return argsArr.reduce(function(p,c){return p - c;});
            case 3: return argsArr.reduce(function(p,c){return p * c;});
            case 4: return argsArr.reduce(function(p,c){return p / c;});
            case 5: return argsArr.reduce(function(p,c){return p % c;});
            default: return 0;
        }
    }

    function Calculate(){}
    Calculate.prototype.add = function(){console.log(result(arguments,1));return this;}
    Calculate.prototype.sub = function(){console.log(result(arguments,2));return this;}
    Calculate.prototype.mul = function(){console.log(result(arguments,3));return this;}
    Calculate.prototype.div = function(){console.log(result(arguments,4));return this;}
    Calculate.prototype.sur = function(){console.log(result(arguments,5));return this;}


    // 最后将插件对象暴露给全局对象
    _global = (function(){ return this || (0, eval)('this'); }());
    if (typeof module !== "undefined" && module.exports) {
        module.exports = Calculate;
    } else if (typeof define === "function" && define.amd) {
        define(function(){return Calculate;});
    } else {
        !('Calculate' in _global) && (_global.Calculate = Calculate);
    }
}());
```

这时调用我们写好的插件，则输出为如下：

```javascript
var plugin = new Calculate();
plugin
    .add(2,1)
    .sub(2,1)
    .mul(2,1)
    .div(2,1)
    .sur(2,1);
// 结果：
// 3
// 1
// 2
// 2
// 0
```

上面的例子，可以并没有太多的现实意义。不过在网页设计中，我们的插件基本上都是服务于UI层面，利用js脚本实现一些可交互的效果。这时我们编写一个UI插件，实现过程也是可以使用链式进行调用。

#### 编写UI组件

一般情况，如果一个js仅仅是处理一个逻辑，我们称之为插件，但如果与dom和css有关系并且具备一定的交互性，一般叫做组件。当然这没有什么明显的区分，只是一种习惯性叫法。
 利用原型链，可以将一些UI层面的业务代码封装在一个小组件中，并利用js实现组件的交互性。
 现有一个这样的需求:

1. 实现一个弹层，此弹层可以显示一些文字提示性的信息；
2. 弹层右上角必须有一个关闭按扭，点击之后弹层消失；
3. 弹层底部必有一个“确定”按扭，然后根据需求，可以配置多一个“取消”按扭；
4. 点击“确定”按扭之后，可以触发一个事件；
5. 点击关闭/“取消”按扭后，可以触发一个事件。

根据需求，我们先写出dom结构：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <link rel="stylesheet" type="text/css" href="index.css">
</head>
<body>
    <div class="mydialog">
        <span class="close">×</span>
        <div class="mydialog-cont">
            <div class="cont">hello world!</div>
        </div>
        <div class="footer">
            <span class="btn">确定</span>
            <span class="btn">取消</span>
        </div>
    </div>
    <script src="index.js"></script>
</body>
</html>
```

写出css结构：

```css
* { padding: 0; margin: 0; }
.mydialog { background: #fff; box-shadow: 0 1px 10px 0 rgba(0, 0, 0, 0.3); overflow: hidden; width: 300px; height: 180px; border: 1px solid #dcdcdc; position: absolute; top: 0; right: 0; bottom: 0; left: 0; margin: auto; }
.close { position: absolute; right: 5px; top: 5px; width: 16px; height: 16px; line-height: 16px; text-align: center; font-size: 18px; cursor: pointer; }
.mydialog-cont { padding: 0 0 50px; display: table; width: 100%; height: 100%; }
.mydialog-cont .cont { display: table-cell; text-align: center; vertical-align: middle; width: 100%; height: 100%; }
.footer { display: table; table-layout: fixed; width: 100%; position: absolute; bottom: 0; left: 0; border-top: 1px solid #dcdcdc; }
.footer .btn { display: table-cell; width: 50%; height: 50px; line-height: 50px; text-align: center; cursor: pointer; }
.footer .btn:last-child { display: table-cell; width: 50%; height: 50px; line-height: 50px; text-align: center; cursor: pointer; border-left: 1px solid #dcdcdc; }
```

接下来，我们开始编写我们的交互插件。
 我们假设组件的弹出层就是一个对象。则这个对象是包含了我们的交互、样式、结构及渲染的过程。于是我们定义了一个构造方法：

```javascript
function MyDialog(){} // MyDialog就是我们的组件对象了
```

对象**MyDialog**就相当于一个绳子，我们只要往这个绳子上不断地挂上钩子就是一个组件了。于是我们的组件就可以表示为：

```javascript
function MyDialog(){}
MyDialog.prototype = {
    constructor: this,
    _initial: function(){},
    _parseTpl: function(){},
    _parseToDom: function(){},
    show: function(){},
    hide: function(){},
    css: function(){},
    ...
}
```

然后就可以将插件的功能都写上。不过中间的业务逻辑，需要自己去一步一步研究。无论如何写，我们最终要做到通过实例化一个MyDialog对象就可以使用我们的插件了。
 在编写的过程中，我们得先做一些工具函数：

1.对象合并函数

```javascript
// 对象合并
function extend(o,n,override) {
    for(var key in n){
        if(n.hasOwnProperty(key) && (!o.hasOwnProperty(key) || override)){
            o[key]=n[key];
        }
    }
    return o;
}
```

2.自定义模板引擎解释函数

```javascript
// 自定义模板引擎
function templateEngine(html, data) {
    var re = /<%([^%>]+)?%>/g,
        reExp = /(^( )?(if|for|else|switch|case|break|{|}))(.*)?/g,
        code = 'var r=[];\n',
        cursor = 0;
    var match;
    var add = function(line, js) {
        js ? (code += line.match(reExp) ? line + '\n' : 'r.push(' + line + ');\n') :
            (code += line != '' ? 'r.push("' + line.replace(/"/g, '\\"') + '");\n' : '');
        return add;
    }
    while (match = re.exec(html)) {
        add(html.slice(cursor, match.index))(match[1], true);
        cursor = match.index + match[0].length;
    }
    add(html.substr(cursor, html.length - cursor));
    code += 'return r.join("");';
    return new Function(code.replace(/[\r\t\n]/g, '')).apply(data);
}
```

3.查找class获取dom函数

```javascript
// 通过class查找dom
if(!('getElementsByClass' in HTMLElement)){
    HTMLElement.prototype.getElementsByClass = function(n, tar){
        var el = [],
            _el = (!!tar ? tar : this).getElementsByTagName('*');
        for (var i=0; i<_el.length; i++ ) {
            if (!!_el[i].className && (typeof _el[i].className == 'string') && _el[i].className.indexOf(n) > -1 ) {
                el[el.length] = _el[i];
            }
        }
        return el;
    };
    ((typeof HTMLDocument !== 'undefined') ? HTMLDocument : Document).prototype.getElementsByClass = HTMLElement.prototype.getElementsByClass;
}
```

结合工具函数，再去实现每一个钩子函数具体逻辑结构：

```javascript
// plugin.js
;(function(undefined) {
    "use strict"
    var _global;

    ...

    // 插件构造函数 - 返回数组结构
    function MyDialog(opt){
        this._initial(opt);
    }
    MyDialog.prototype = {
        constructor: this,
        _initial: function(opt) {
            // 默认参数
            var def = {
                ok: true,
                ok_txt: '确定',
                cancel: false,
                cancel_txt: '取消',
                confirm: function(){},
                close: function(){},
                content: '',
                tmpId: null
            };
            this.def = extend(def,opt,true);
            this.tpl = this._parseTpl(this.def.tmpId);
            this.dom = this._parseToDom(this.tpl)[0];
            this.hasDom = false;
        },
        _parseTpl: function(tmpId) { // 将模板转为字符串
            var data = this.def;
            var tplStr = document.getElementById(tmpId).innerHTML.trim();
            return templateEngine(tplStr,data);
        },
        _parseToDom: function(str) { // 将字符串转为dom
            var div = document.createElement('div');
            if(typeof str == 'string') {
                div.innerHTML = str;
            }
            return div.childNodes;
        },
        show: function(callback){
            var _this = this;
            if(this.hasDom) return ;
            document.body.appendChild(this.dom);
            this.hasDom = true;
            document.getElementsByClass('close',this.dom)[0].onclick = function(){
                _this.hide();
            };
            document.getElementsByClass('btn-ok',this.dom)[0].onclick = function(){
                _this.hide();
            };
            if(this.def.cancel){
                document.getElementsByClass('btn-cancel',this.dom)[0].onclick = function(){
                    _this.hide();
                };
            }
            callback && callback();
            return this;
        },
        hide: function(callback){
            document.body.removeChild(this.dom);
            this.hasDom = false;
            callback && callback();
            return this;
        },
        modifyTpl: function(template){
            if(!!template) {
                if(typeof template == 'string'){
                    this.tpl = template;
                } else if(typeof template == 'function'){
                    this.tpl = template();
                } else {
                    return this;
                }
            }
            // this.tpl = this._parseTpl(this.def.tmpId);
            this.dom = this._parseToDom(this.tpl)[0];
            return this;
        },
        css: function(styleObj){
            for(var prop in styleObj){
                var attr = prop.replace(/[A-Z]/g,function(word){
                    return '-' + word.toLowerCase();
                });
                this.dom.style[attr] = styleObj[prop];
            }
            return this;
        },
        width: function(val){
            this.dom.style.width = val + 'px';
            return this;
        },
        height: function(val){
            this.dom.style.height = val + 'px';
            return this;
        }
    }

    _global = (function(){ return this || (0, eval)('this'); }());
    if (typeof module !== "undefined" && module.exports) {
        module.exports = MyDialog;
    } else if (typeof define === "function" && define.amd) {
        define(function(){return MyDialog;});
    } else {
        !('MyDialog' in _global) && (_global.MyDialog = MyDialog);
    }
}());
```

到这一步，我们的插件已经达到了基础需求了。我们可以在页面这样调用：

```html
<script type="text/template" id="dialogTpl">
    <div class="mydialog">
        <span class="close">×</span>
        <div class="mydialog-cont">
            <div class="cont"><% this.content %></div>
        </div>
        <div class="footer">
            <% if(this.cancel){ %>
            <span class="btn btn-ok"><% this.ok_txt %></span>
            <span class="btn btn-cancel"><% this.cancel_txt %></span>
            <% } else{ %>
            <span class="btn btn-ok" style="width: 100%"><% this.ok_txt %></span>
            <% } %>
        </div>
    </div>
</script>
<script src="index.js"></script>
<script>
    var mydialog = new MyDialog({
        tmpId: 'dialogTpl',
        cancel: true,
        content: 'hello world!'
    });
    mydialog.show();
</script>
```

### 插件的监听

弹出框插件我们已经实现了基本的显示与隐藏的功能。不过我们在怎么时候弹出，弹出之后可能进行一些操作，实际上还是需要进行一些可控的操作。就好像我们进行事件绑定一样，只有用户点击了按扭，才响应具体的事件。那么，我们的插件，应该也要像事件绑定一样，只有执行了某些操作的时候，调用相应的事件响应。
 这种js的设计模式，被称为 **订阅/发布模式**，也被叫做 **观察者模式**。我们插件中的也需要用到观察者模式，比如，在打开弹窗之前，我们需要先进行弹窗的内容更新，执行一些判断逻辑等，然后执行完成之后才显示出弹窗。在关闭弹窗之后，我们需要执行关闭之后的一些逻辑，处理业务等。这时候我们需要像平时绑定事件一样，给插件做一些“事件”绑定回调方法。
 我们jquery对dom的事件响应是这样的：

```javascript
$(<dom>).on("click",function(){})
```

我们照着上面的方式设计了对应的插件响应是这样的：

```javascript
mydialog.on('show',function(){})
```

则，我们需要实现一个事件机制，以达到监听插件的事件效果。关于自定义事件监听，可以参考一篇博文：[漫谈js自定义事件、DOM/伪DOM自定义事件](http://www.zhangxinxu.com/wordpress/2012/04/js-dom%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BA%8B%E4%BB%B6/)。在此不进行大篇幅讲自定义事件的问题。
 最终我们实现的插件代码为：

```javascript
// plugin.js
;(function(undefined) {
    "use strict"
    var _global;

    // 工具函数
    // 对象合并
    function extend(o,n,override) {
        for(var key in n){
            if(n.hasOwnProperty(key) && (!o.hasOwnProperty(key) || override)){
                o[key]=n[key];
            }
        }
        return o;
    }
    // 自定义模板引擎
    function templateEngine(html, data) {
        var re = /<%([^%>]+)?%>/g,
            reExp = /(^( )?(if|for|else|switch|case|break|{|}))(.*)?/g,
            code = 'var r=[];\n',
            cursor = 0;
        var match;
        var add = function(line, js) {
            js ? (code += line.match(reExp) ? line + '\n' : 'r.push(' + line + ');\n') :
                (code += line != '' ? 'r.push("' + line.replace(/"/g, '\\"') + '");\n' : '');
            return add;
        }
        while (match = re.exec(html)) {
            add(html.slice(cursor, match.index))(match[1], true);
            cursor = match.index + match[0].length;
        }
        add(html.substr(cursor, html.length - cursor));
        code += 'return r.join("");';
        return new Function(code.replace(/[\r\t\n]/g, '')).apply(data);
    }
    // 通过class查找dom
    if(!('getElementsByClass' in HTMLElement)){
        HTMLElement.prototype.getElementsByClass = function(n){
            var el = [],
                _el = this.getElementsByTagName('*');
            for (var i=0; i<_el.length; i++ ) {
                if (!!_el[i].className && (typeof _el[i].className == 'string') && _el[i].className.indexOf(n) > -1 ) {
                    el[el.length] = _el[i];
                }
            }
            return el;
        };
        ((typeof HTMLDocument !== 'undefined') ? HTMLDocument : Document).prototype.getElementsByClass = HTMLElement.prototype.getElementsByClass;
    }

    // 插件构造函数 - 返回数组结构
    function MyDialog(opt){
        this._initial(opt);
    }
    MyDialog.prototype = {
        constructor: this,
        _initial: function(opt) {
            // 默认参数
            var def = {
                ok: true,
                ok_txt: '确定',
                cancel: false,
                cancel_txt: '取消',
                confirm: function(){},
                close: function(){},
                content: '',
                tmpId: null
            };
            this.def = extend(def,opt,true); //配置参数
            this.tpl = this._parseTpl(this.def.tmpId); //模板字符串
            this.dom = this._parseToDom(this.tpl)[0]; //存放在实例中的节点
            this.hasDom = false; //检查dom树中dialog的节点是否存在
            this.listeners = []; //自定义事件，用于监听插件的用户交互
            this.handlers = {};
        },
        _parseTpl: function(tmpId) { // 将模板转为字符串
            var data = this.def;
            var tplStr = document.getElementById(tmpId).innerHTML.trim();
            return templateEngine(tplStr,data);
        },
        _parseToDom: function(str) { // 将字符串转为dom
            var div = document.createElement('div');
            if(typeof str == 'string') {
                div.innerHTML = str;
            }
            return div.childNodes;
        },
        show: function(callback){
            var _this = this;
            if(this.hasDom) return ;
            if(this.listeners.indexOf('show') > -1) {
                if(!this.emit({type:'show',target: this.dom})) return ;
            }
            document.body.appendChild(this.dom);
            this.hasDom = true;
            this.dom.getElementsByClass('close')[0].onclick = function(){
                _this.hide();
                if(_this.listeners.indexOf('close') > -1) {
                    _this.emit({type:'close',target: _this.dom})
                }
                !!_this.def.close && _this.def.close.call(this,_this.dom);
            };
            this.dom.getElementsByClass('btn-ok')[0].onclick = function(){
                _this.hide();
                if(_this.listeners.indexOf('confirm') > -1) {
                    _this.emit({type:'confirm',target: _this.dom})
                }
                !!_this.def.confirm && _this.def.confirm.call(this,_this.dom);
            };
            if(this.def.cancel){
                this.dom.getElementsByClass('btn-cancel')[0].onclick = function(){
                    _this.hide();
                    if(_this.listeners.indexOf('cancel') > -1) {
                        _this.emit({type:'cancel',target: _this.dom})
                    }
                };
            }
            callback && callback();
            if(this.listeners.indexOf('shown') > -1) {
                this.emit({type:'shown',target: this.dom})
            }
            return this;
        },
        hide: function(callback){
            if(this.listeners.indexOf('hide') > -1) {
                if(!this.emit({type:'hide',target: this.dom})) return ;
            }
            document.body.removeChild(this.dom);
            this.hasDom = false;
            callback && callback();
            if(this.listeners.indexOf('hidden') > -1) {
                this.emit({type:'hidden',target: this.dom})
            }
            return this;
        },
        modifyTpl: function(template){
            if(!!template) {
                if(typeof template == 'string'){
                    this.tpl = template;
                } else if(typeof template == 'function'){
                    this.tpl = template();
                } else {
                    return this;
                }
            }
            this.dom = this._parseToDom(this.tpl)[0];
            return this;
        },
        css: function(styleObj){
            for(var prop in styleObj){
                var attr = prop.replace(/[A-Z]/g,function(word){
                    return '-' + word.toLowerCase();
                });
                this.dom.style[attr] = styleObj[prop];
            }
            return this;
        },
        width: function(val){
            this.dom.style.width = val + 'px';
            return this;
        },
        height: function(val){
            this.dom.style.height = val + 'px';
            return this;
        },
        on: function(type, handler){
            // type: show, shown, hide, hidden, close, confirm
            if(typeof this.handlers[type] === 'undefined') {
                this.handlers[type] = [];
            }
            this.listeners.push(type);
            this.handlers[type].push(handler);
            return this;
        },
        off: function(type, handler){
            if(this.handlers[type] instanceof Array) {
                var handlers = this.handlers[type];
                for(var i = 0, len = handlers.length; i < len; i++) {
                    if(handlers[i] === handler) {
                        break;
                    }
                }
                this.listeners.splice(i, 1);
                handlers.splice(i, 1);
                return this;
            }
        },
        emit: function(event){
            if(!event.target) {
                event.target = this;
            }
            if(this.handlers[event.type] instanceof Array) {
                var handlers = this.handlers[event.type];
                for(var i = 0, len = handlers.length; i < len; i++) {
                    handlers[i](event);
                    return true;
                }
            }
            return false;
        }
    }

    // 最后将插件对象暴露给全局对象
    _global = (function(){ return this || (0, eval)('this'); }());
    if (typeof module !== "undefined" && module.exports) {
        module.exports = MyDialog;
    } else if (typeof define === "function" && define.amd) {
        define(function(){return MyDialog;});
    } else {
        !('MyDialog' in _global) && (_global.MyDialog = MyDialog);
    }
}());
```

然后调用的时候就可以直接使用插件的事件绑定了。

```javascript
var mydialog = new MyDialog({
    tmpId: 'dialogTpl',
    cancel: true,
    content: 'hello world!'
});
mydialog.on('confirm',function(ev){
    console.log('you click confirm!');
    // 写你的确定之后的逻辑代码...
});
document.getElementById('test').onclick = function(){
    mydialog.show();
}
```

给出此例子的[demo](https://huangguangjie.github.io/myDialog/)，有需要具体实现的同学可以去查阅。

### 插件发布

我们写好了插件，实际上还可以将我们的插件发布到开源组织去分享给更多人去使用（代码必须是私人拥有所有支配权限）。我们将插件打包之后，就可以发布到开源组织上去供别人下载使用了。
 我们熟知的npm社区就是一个非常良好的发布插件的平台。具体可以如下操作：
 写初始化包的描述文件：

```bash
$ npm init
```

注册包仓库帐号

```bash
$ npm adduser
Username: <帐号>
Password: <密码>
Email:(this IS public) <邮箱>
Logged in as <帐号> on https://registry.npmjs.org/.
```

上传包

```bash
$ npm publish
```

安装包

```bash
$ npm install mydialog
```

到此，我们的插件就可以直接被更多人去使用了。

### 结论

写了这么多，比较啰嗦，我在此做一下总结：
 关于如何编写出一个好的js原生插件，需要平时在使用别人的插件的同时，多查看一下api文档，了解插件的调用方式，然后再看一下插件的源码的设计方式。基本上我们可以确定大部分插件都是按照原型的方式进行设计的。而我从上面的例子中，就使用了好多js原生的知识点，函数的命名冲突、闭包、作用域，自定义工具函数扩展对象的钩子函数，以及对象的初始化、原型链继承，构造函数的定义及设计模式，还有事件的自定义，js设计模式的观察者模式等知识。这些内容还是需要初学者多多了解才能进行一些高层次一些的插件开发。

