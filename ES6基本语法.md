## ES6基本语法

### 一、前言：

- let和const申明变量和常量
- 作用域只限于当前代码块
- 使用let申明的变量作用域不会提升
- 在相同的作用域下不能申明相同的变量
- for循环体现let的父子作用域

### 二、es6的解构赋值：一一对应

数组：

```javascript
let [name, age, sex] = ["Samve", 30, "men"];

console.log(name);

console.log(age);

console.log(sex);
```

对象：

```javascript
let {name, age, sex} = {name: "张三" , age: 30, sex: "men"};

console.log(name);

console.log(age);

console.log(sex);
```

字符：

```javascript
let [a, b, c, d, e] = "我是中国人！";

console.log(a);

console.log(b);

console.log(c);

console.log(d);

console.log(e);
```

### 三、es6新增的数据类型set：

特点：

1、类似数组，但没有重复的元素；

2、开发中用于去重；

3、key和value都是相等的；

实例：

```javascript
let set = new Set();

console.log(set);

let set2 = new Set(['张三', '李四', '王五']);

console.log(set2);

let set3 = new Set(['张三', '李四', '王五', '张三', '王五']);

console.log(set3);
```

一个属性：

```javascript
console.log(set3.size);
```

四个方法：

add：true/fasle

set3.add("赵六").add("孙钱");

delete：true/fasle

has：true/fasle

clear：

### 四、es6中新增的数据类型map：

1、键值对的集合：

```javascript
let obj1 = {a:1}, obj2 = {b:2};
const map = new Map([
	['name', '张三'],
	['age', 18],
	['sex', '男'],
	[obj1, '今天天气不错'],
	[obj2, '适合敲代码'],
	[[1, 2], 'hhh']
]);
console.log(map);
```

2、常用 属性：size

```javascript
console.log(map.size);
```

3、常用方法：set、get、delete、has、clear、keys、values、entries

```javascript
    //set
    map.set('friend', ['赵六', '李七']).set(['dog'], '小花');
    console.log(map);

    //get
    console.log(map.get('name'));
    console.log(map.get(obj1));

    //delete
    console.log(map.delete(obj1));;
    console.log(map);

    //has
    console.log(map.has(obj2));

    //clear
    //map.clear();
    console.log(map);

    //keys()
    console.log(map.keys());

    //values()
    console.log(map.values());

    //entries()
    console.log(map.entries());
```

4、遍历：forEach

```javascript
    map.forEach(function(value, index){
        console.log(index + ": " + value);
    })
```

### 五、es6中新增的数据类型symbol

解决对象中属性命名冲突的问题。

```javascript
    //定义
    let str1 = Symbol();
    let str2 = Symbol();
    console.log(str1 === str2);

    console.log(typeof str1);
    console.log(typeof str2);

    //描述
    let str3 = Symbol("name");
    let str4 = Symbol("name");
    console.log(str3);
    console.log(str4);
    console.log(str3 === str4);

    //对象的属性名
    const obj = {};
    obj.name = '张三';
    console.log(obj);
    obj.name = '李四';
    console.log(obj);
    obj[Symbol('name')] = '张三';
    obj[Symbol('name')] = '李四';
    console.log(obj);
```

### 六、class的基本运用：

构造函数的另一种写法，作用在于对象原型的写法更加清晰，更加面向对象的编程方式

```javascript
    //1、构造函数
    function Person(name, age){
        this.name = name;
        this.age = age;
    }

    Person.prototype = {
            constructor: Person,
            print(){
                    console.log("我叫：" + this.name + ",今年：" + this.age + "!");
            }
    };

    let person = new Person("张三", 19);
    console.log(person);

    //2、通过Class面向对象
    class Person{
        constructor(name, age){
            this.name = name;
            this.age = age;
        }

        print(){
            console.log("我叫：" + this.name + ",今年：" + this.age + "!");
        }
    }

    let person = new Person("张三", 19);
    console.log(person);
    person.print();
```

### 七、模板字符串：

```javascript
    let str = '适合敲代码';
    let html = `
                    <html>
                        <head></head>
                        <body>
                            <p>今天天气不错</p>
                            <div>${str}</div>
                        </body>
                    </html>
                `;
    console.log(html);
```

### 八、解构：

数组和对象是JS中最常用也是最重要表示形式。为了简化提取信息，ES6新增了解构，这是将一个数据结构分解为更小的部分的过程。

```javascript
const people = {
    name : 'Samve',
    age : 30
};

const {name, age} = people;

console.log(`${name} --- ${age}`);

const color = ['red', 'green'];
const [first, second] = color;
console.log(first);
console.log(second);
```

### 九、Object.assign()这个方法来实现浅复制：

Object.assign() 可以把任意多个源对象自身可枚举的属性拷贝给目标对象，然后返回目标对象。第一参数即为目标对象。在实际项目中，我们为了不改变源对象。一般会把目标对象传为{}

```javascript
const objA = { name: 'cc', age: 18 }
    const objB = { address: 'beijing' }
    const objC = {} // 这个为目标对象
    const obj = Object.assign(objC, objA, objB)

    // 我们将 objA objB objC obj 分别输出看看
    console.log(objA)   // { name: 'cc', age: 18 }
    console.log(objB) // { address: 'beijing' }
    console.log(objC) // { name: 'cc', age: 18, address: 'beijing' }
    console.log(obj) // { name: 'cc', age: 18, address: 'beijing' }

    // 是的，目标对象ObjC的值被改变了。
    // so，如果objC也是你的一个源对象的话。请在objC前面填在一个目标对象{}
    Object.assign({}, objC, objA, objB)
```

### 十、对象初始化简写：

ES5我们对于对象都是以键值对的形式书写，是有可能出现键值对重名的。例如：

```javascript
    function people(name, age) {
        return {
            name: name,
            age: age
        };
    }
```

键值对重名，ES6可以简写如下：

```javascript
    function people(name, age) {
        return {
            name,
            age
        };
    }
```

同样，key和value一样的，写一个就够了：

```javascript
    let name = "张三";
    let age = "李四";
    let person = {
        name,
        age
    }
    console.log(person);
```

ES6 同样改进了为对象字面量方法赋值的语法。ES5为对象添加方法：

```javascript
    const people = {
        name: 'lux',
        getName: function() {
            console.log(this.name)
        }
    }
```

ES6通过省略冒号与 function 关键字，将这个语法变得更简洁

```javascript
    const people = {
        name: 'lux',
        getName () {
            console.log(this.name)
        }
    }
```

### 十一、箭头函数：

箭头函数最直观的三个特点。

不需要 function 关键字来创建函数

省略 return 关键字

继承当前上下文的 this 关键字

```javascript
//例如：
    [1,2,3].map(x => x + 1)
    
//等同于：
    [1,2,3].map((function(x){
        return x + 1
    }).bind(this))
```

说个小细节。

当你的函数有且仅有一个参数的时候，是可以省略掉括号的。当你函数返回有且仅有一个表达式的时候可以省略{} 和 return；例如:

```javascript
    var people = name => 'hello' + name
    //参数name就没有括号
```

作为参考

```javascript
    var people = (name, age) => {
        const fullName = 'hello' + name
        return fullName
    } 
    //如果缺少()或者{}就会报错
```

### 十二、函数默认参数：

在ES5我们给函数定义参数默认值是怎么样？

```javascript
    function action(num) {
        num = num || 200
        //当传入num时，num为传入的值
        //当没传入参数时，num即有了默认值200
        return num
    }
```

但细心观察的同学们肯定会发现，num传入为0的时候就是false，但是我们实际的需求就是要拿到num = 0，此时num = 200 明显与我们的实际想要的效果明显不一样。

ES6为参数提供了默认值。在定义函数时便初始化了这个参数，以便在参数没有被传递进去时使用。

```javascript
    function action(num = 200) {
        console.log(num)
    }
    action(0) // 0
    action() //200
    action(300) //300
```

### 十三、import 和 export：

import导入模块、export导出模块

```javascript
//全部导入
import people from './example'

//有一种特殊情况，即允许你将整个模块当作单一对象进行导入
//该模块的所有导出都会作为对象的属性存在
import * as example from "./example.js"
console.log(example.name)
console.log(example.age)
console.log(example.getName())

//导入部分
import {name, age} from './example'

// 导出默认, 有且只有一个默认
export default App

// 部分导出
export class App extend Component {};
```

### 模块化总结：

1. 当用export default people导出时，就用 import people 导入（不带大括号）

2. 一个文件里，有且只能有一个export default。但可以有多个export。

3. 当用export name 时，就用import { name }导入（记得带上大括号）

4. 当一个文件里，既有一个export default people, 又有多个export name 或者 export age时，导入就用 import people, { name, age } 

5. 当一个文件里出现n多个 export 导出很多模块，导入时除了一个一个导入，也可以用import * as example

### 十四、Spread Operator 展开运算符

 

ES6中另外一个好玩的特性就是Spread Operator 也是三个点儿...接下来就展示一下它的用途。

组装对象或者数组

```javascript
//数组
    const color = ['red', 'yellow']
    const colorful = [...color, 'green', 'pink']
    console.log(colorful) //[red, yellow, green, pink]
    
    //对象
    const alp = { fist: 'a', second: 'b'}
    const alphabets = { ...alp, third: 'c' }
    console.log(alphabets) //{ "fist": "a", "second": "b", "third": "c"
}
```

有时候我们想获取数组或者对象除了前几项或者除了某几项的其他项

```javascript
//数组
    const number = [1,2,3,4,5]
    const [first, ...rest] = number
    console.log(rest) //2,3,4,5
    //对象
    const user = {
        username: 'lux',
        gender: 'female',
        age: 19,
        address: 'peking'
    }
    const { username, ...rest } = user
    console.log(rest) //{"address": "peking", "age": 19, "gender": "female"
}
```

对于 Object 而言，还可以用于组合成新的 Object 。(ES2017 stage-2 proposal) 当然如果有重复的属性名，右边覆盖左边

```javascript
const first = {
        a: 1,
        b: 2,
        c: 6,
    }
    const second = {
        c: 3,
        d: 4
    }
    const total = { ...first, ...second }
    console.log(total) // { a: 1, b: 2, c: 3, d: 4 }
```