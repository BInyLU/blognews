### 函数节流

在一些高频触发的事件中（如onresize，scroll，mousemove等），有时候我们并不希望每次触发事件就执行回调，这时候我们就需要用到函数节流

以函数方式实现函数节流

```
// 节流函数
function throttle(fn, interval = 500){
    let timer = null;
    return function(...args){
        let ctx = this;
        if(timer) return;
        timer = setTimeout(function(){
            clearTimeout(timer)
            timer = null;
            fn.apply(ctx, args)
        }, interval)
    }
}

// 绑定onresize
window.onresize = function(){
    console.log(Math.random() + '===========')
    t(fn,this,200);
}
```

### 函数防抖

函数防抖主要用于持续触发某个事件，但我们只希望最后一次触发才执行的回调方法的场景，比如搜索框内容变更时进行实时查询

```
const debounce = function(fn, delay) {
    // 此处timer用到了闭包，防止timer被释放掉
    let timer = null;
    return function(...args){
        clearTimeout(timer)
        timer = setTimeout(() => {
            fn.apply(this, args);
        }, delay)
    }
}

function test(){
    console.log(new Date())
}

document.addEventListener('scroll',debounce(test, 500))
```

### 执行顺序

------

javascript是一门解释语言，默认情况下是从上到下顺序执行的。 但这也不是绝对的。在没有用关键字var申明的全局函数就会首先被执行（函数声明提升）

```
aF();    //2
aV();    //error: bV is not defined 
function aF(){
    b(b);    //调用全局函数b
}
unction b(){
    alert(2);
}
function aV(){
    alert(bV);    //调用全局变量bV
}
bV = 'bv';
```

使用var声明的变量会被提到执行环境有最上方，但变量的赋值还是在原来的位置（变量声明提升）

```
if (true) {
    var a = 123;
}
alert(a);    //123   外面能够访问到if语句块里面声明的变量
```

### for……in vs Object.keys()

------

for……in 通常用于遍历对象的key，但原型上的key也会被一并返回

Object.keys() 返回对象实例属性key组成的数组

```
Object.prototype.a = 'aa';

let obj = {
ob: 'bb'
};

for(var i in obj) {
oconsole.log(i);
}
// 输出
// a
// b

console.log(Object.keys(obj)); // [ b ]
```

### 函数柯里化

　　柯里化函数，实现原理是递归调用，当参数和原函数的参数个数相同时，则执行原函数，并返回结果，哪果参数个数小于原函数时，则返回柯里化函数自身。

```
/**
 * [curry] 柯里化函数，实现原理是递归调用，当参数和原函数的参数个数相同时，则执行原函数，并返回结果，哪果参数个数小于原函数时，则返回柯里化函数自身
 * @param  {Function} fn   [description]
 * @param  {[type]}   args [description]
 * @return {[type]}   
 *         如果param数组的长度和fn参数的长度一至时，执行fn方法，返回计算结果；
 *         如果param数组的长度小于fn参数的长度时，递归调用cur，继续拼装params。 
 */
var curry = function(fn, args){ //1
    return function(){
        let param = [...arguments];
        if(args !== undefined) {
            // param = param.concat(args);
            param = [...param, ...args];
        }

        if(param.length < fn.length) {
            return curry(fn, param)
        }
        return fn.apply(null, param)
    }
}

let adds = curry(add);
console.log(adds(1,2,4))
console.log(adds(1)(2)(3))
console.log(adds(2)(3)(4))
```

### 递归

递归是编程语言中最基础的方法，但在平时的工作中并不是很频繁使用 但在一些复杂的业务场景又很适用

- 上面的函数柯里化
- 数组深度

```
function depth(arr){
    if(typeof arr != 'object') return 0;
    let a = []
    arr.forEach( (item, i) =>{
        a.push(depth(item))
    })
    let maxDepth = Math.max(...a)

    return maxDepth+1;    
}
let arr = [1, 2, [3, [5,[6],[7,[8]]],[4]]]
console.log(depth(arr))
```

快速排序

```
/**
 * 从数组中抽取任意一个元素做为基准元素；
 * 循环抽离基准元素后的数组，比基准元素值大的放在next数组中，其余则放在左pre数组中；
 * 然后对pre和nex数组做递归调用，如果数组中只有一个元素，且返回当前数组，否则，返回 sort(pre).concat(split, sort(next))
 * 
 * @param  {[type]} arr [description]
 * @return {[type]}     [description]
 */
function sort(arr) {
    if(arr.length <= 1) {
        return arr;
    }

    let split = arr.splice(0,1), pre = [], next = [];
    arr.forEach( item=> {
        if (item <= split) {
            pre.push(item)
        } else {
            next.push(item)
        }
    })
    return sort(pre).concat(split,sort(next))
}
let arr = [7,3,4.1,1,3,34,12,21,9,7.3,16];
console.log(sort(arr));
```

- 全组合

```
function all(arr, index = 0, group = []) {

    if(index == arr.length) return group;

    let temp = [];
    temp.push(arr[index]);
    for(let i = 0; i < group.length; i++) {
        temp.push(group[i] + arr[index])
    }
    group = [...group, ...temp]
    return all(arr, index+1, group)
}
console.log(all(data))
```

- 全排列

```
function allSort(arr, index = 0, group = []){
    if(index == arr.length) return group;

    let temp = [];
    temp.push(arr[index]);
    for(let i = 0; i < group.length; i++) {
        temp.push(group[i] + arr[index])
        temp.push(arr[index] + group[i])
    }
    group = [...group, ...temp]
    return allSort(arr, index+1, group)
}
console.log(allSort(data))
```