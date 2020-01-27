## ES6中的模块化



在之前的javascript中是没有模块化概念的。如果要进行模块化操作，需要引入第三方的类库。随着技术的发展，前后端分离，前端的业务变的越来越复杂化。直至ES6带来了模块化，才让javascript第一次支持了module。ES6的模块化分为导出（export）与导入（import）两个模块。

**export的用法**

在ES6中每一个模块即是一个文件，在文件中定义的变量，函数，对象在外部是无法获取的。如果你希望外部可以读取模块当中的内容，就必须使用export来对其进行暴露（输出）。先来看个例子，来对一个变量进行模块化。我们先来创建一个test.js文件，来对这一个变量进行输出：

```javascript
export let myName="laowang";
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

然后可以创建一个index.js文件，以import的形式将这个变量进行引入：

```javascript
import {myName} from "./test.js";
console.log(myName);//laowang
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如果要输出多个变量可以将这些变量包装成对象进行模块化输出：

```javascript
let myName="laowang";
let myAge=90;
let myfn=function(){
    return "我是"+myName+"！今年"+myAge+"岁了"
}
export {
    myName,
    myAge,
    myfn
}
/******************************接收的代码调整为**********************/
import {myfn,myAge,myName} from "./test.js";
console.log(myfn());//我是laowang！今年90岁了
console.log(myAge);//90
console.log(myName);//laowang
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如果你不想暴露模块当中的变量名字，可以通过as来进行操作:

```javascript
let myName="laowang";
let myAge=90;
let myfn=function(){
    return "我是"+myName+"！今年"+myAge+"岁了"
}
export {
    myName as name,
    myAge as age,
    myfn as fn
}
/******************************接收的代码调整为**********************/
import {fn,age,name} from "./test.js";
console.log(fn());//我是laowang！今年90岁了
console.log(age);//90
console.log(name);//laowang
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

也可以直接导入整个模块，将上面的接收代码修改为：

```javascript
import * as info from "./test.js";//通过*来批量接收，as 来指定接收的名字
console.log(info.fn());//我是laowang！今年90岁了
console.log(info.age);//90
console.log(info.name);//laowang
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**默认导出（default export）**

一个模块只能有一个默认导出，对于默认导出，导入的名称可以和导出的名称不一致。

```javascript
/******************************导出**********************/
export default function(){
    return "默认导出一个方法"
}
/******************************引入**********************/
import myFn from "./test.js";//注意这里默认导出不需要用{}。
console.log(myFn());//默认导出一个方法
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

可以将所有需要导出的变量放入一个对象中，然后通过default export进行导出

```javascript
/******************************导出**********************/
export default {
    myFn(){
        return "默认导出一个方法"
    },
    myName:"laowang"
}
/******************************引入**********************/
import myObj from "./test.js";
console.log(myObj.myFn(),myObj.myName);//默认导出一个方法 laowang
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

同样也支持混合导出

```javascript
/******************************导出**********************/
export default function(){
    return "默认导出一个方法"
}
export var myName="laowang";
/******************************引入**********************/
import myFn,{myName} from "./test.js";
console.log(myFn(),myName);//默认导出一个方法 laowang
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**重命名export和import**

如果导入的多个文件中，变量名字相同，即会产生命名冲突的问题，为了解决该问题，ES6为提供了重命名的方法，当你在导入名称时可以这样做：

```javascript
/******************************test1.js**********************/
export let myName="我来自test1.js";
/******************************test2.js**********************/
export let myName="我来自test2.js";
/******************************index.js**********************/
import {myName as name1} from "./test1.js";
import {myName as name2} from "./test2.js";
console.log(name1);//我来自test1.js
console.log(name2);//我来自test1.js
```