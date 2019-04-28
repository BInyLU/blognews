> JavaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，内置支持类型。
>
> JS作用：表单验证，减轻服务端的压力；添加页面动画效果；动态更改页面内容；Ajax网络请求。
>
> 下面简单介绍JS的基础知识。



**一、基本结构**

```html
<script type="text/javascript">

　　　　alert("hahaha");

</script>
```

**二、使用JS的三种方式**

　1、直接在HTML标签中，使用事件属性，调用JS代码：

```html
　　<button onclick="alert('点我');">点我！</button>
```

　2、在页面的任意位置，使用script标签，插入JS代码。

```html
　　<script type="text/javascript">

　　　　alert("hahaha");

　　</script>
```

　3、引入外部JS文件：

```javascript
　<script src= "js/01.js" type="text/javascript"></script>
```

**注意事项**

　　① JS代码可以放在页面的任意位置使用，但是放置的位置不同，将影响JS执行的顺序

　　② 引入外部JS的script标签中，不能再包含任何的JS代码。

**三、JS中的变量**

　1、变量的声明

```javascript
　　　var num = 1;        // 使用var声明的变量，属于局部变量，只在当前作用于有效

　　　num = "hahaha";      // 不用var声明的变量，默认为全局变量，在整个JS文件中可用

　　　var a=1,b,c=2; 　　 // 使用一行代码，声明多个语句。其中b为Undefined
```

** JS中变量声明的注意事项**

　　① JS中声明变量的关键字只有一个var，变量的类型，取决于所赋的值；

　　　如果声明后为赋值，则为Undefined类型。

　　② JS中同一个变量，可以在多次赋值中，被修改数据类型；

```javascript
　　　　var num1=1;

　　　　num = "字符串";
```

　　③ 变量可以使用var声明，也可以直接赋值声明。（区别：使用var声明的作用域为局部变量）　　

　　④ 在JS中，一个变量可以多次使用var声明，后面的声明相当于直接赋值，没有任何作用；

　　⑤ JS变量区分大小写，大写和小写不是一个变量；

　2、JS中的数据类型：

　　**Undefined：使用var声明，但是没有赋值的变量**

- null：表示空的引用

- Boolean：真假

- Number：数值类型，包括整型和浮点型

- Object：对象


　3、常用数值函数

　　① **isNaN**：用于检测是一个变量，是不是非数值（Not a Number）；

　　　isNaN在检测时，会先调用Number函数，尝试将变量转为数值类型，如果最终结果能够转化为数值，则不是NaN。

　　② **Number**函数：用于将各种数据类型转为数值类型

- Undefined：无法转换，返回NaN；

- null：转为0；

- Boolean：true转为1，false转为0；

- 字符串：如果字符串是纯数值字符串，可以转换，”123”–>123

  如果字符串包含非数值字符，不能转换，”123a”–>NaN

  如果是空字符串，转为0，””–>0 ” ”–>0



　　③ **parseInt()**：将字符串转为数值类型

- 如果是空字符串，不能转，” ”–>NaN
- 如果是纯数值类型字符串，可以转换，且小数点直接舍去，不保留，”123”–>123 “123.9”–>123
- 如果字符串包含非数值字符，则将非数值字符前面的整数进行转换，”123a”–>123 “a123”–>NaN

　　④ **parseFloat()**：转换机制与java相同。

　　　不同的是：转换数值字符串时，如果字符串为小数则可以保留小数点，”123.5”–>123.5 “123”–>123

　　⑤ **typeof()**：检测一个变量的数据类型。

　　　字符串->String  数值->number   true/false->boolean

　　　未定义->undefined  对象/null->object  函数->function

**四、JS中常用的输入输出语句**

　 1、**alert()**：弹窗输出

　 2、**prompt()**：弹窗输入

　　接受两部分参数：① 输入提示内容；② 输入框的默认文本。（两部分都可以省略）

　　输入的内容默认都是字符串。

　3、**document.write**(”12345hahaha”);

　　　在浏览器屏幕上面打印。

　4、**console.log**(”hahaha”);

　　　浏览器控制台打印。

**五、JS中的运算符**

　1、除号：无论符号两边是整数还是小数，**除完后都将按照实际结果保留小数**；

　　例如：22/10 –> 2.2

　2、===：要求等号两边的数据、类型和值都必须相同。如果类型不同，直接返回false

　　　==：只判断两边的数据，值是否相等，并不关心等式两边是否是同一种数据类型

　　　！=：不等  ！==：不全等

　3、&、| 只能进行按位运算，如果两边不是数值类型，将转为数值类型再运算；

　　&&、|| 进行逻辑运算

　4、各级运算符的优先级别表：

![image](http://upload-images.jianshu.io/upload_images/12577968-29789455ebda4a78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 六、 JS分支与循环

**一、if判断**

　1、JS中的真假判断：

　　① Boolean类型：true为真，false为假；

　　② 数值类型：0为假，非0为真；

　　③ 字符串类型：””为假，非空字符串为真；

　　④ Null/Undefined/NaN：全为假；

　　⑤ Object：全为真。

　2、if判断：

```javascript
if(undefined) {
   　　          console.log(true);
　　    } else {
    　　 console.log(false);      
　　    }</pre>
```

**二、循环**

　1、switch

　　switch结构的()中可以放各种数据类型：

　　比对时，采用  ”===”  进行判断，要求数据类型完全相等

```javascript
var num=1;
　　switch (num){
　　　　case 1:
         console.log("dengyu");
         break;
 　　　default:
         console.log("budeng");
         break;
　　}
```

Java中switch不能判断区间，而JS中switch可以判断区间 ↓↓↓

```javascript
switch (true){
            case num>=0 && num<10:
                    console.log(1);
                    break;
            case num>=10 && num<100
                    console.log(2);
                    break;
            default://欢迎加入全栈开发交流圈一起学习交流：864305860 
                    console.log(3);  //面向1-3年前端人员
                    break; //帮助突破技术瓶颈，提升思维能力 
    }
```

2、do-while

```javascript
　　　　do{
　　　　}while (false);
```

　3、for循环

```javascript
　　　　for(var i=0;i<100;i++){ }
```

　4、例：输入一个数，判断其是否是正整数，如果不是正整数，提示输入有误，请重新输入；如果是正整数，反转输出这个数。

```javascript
var num=prompt("请输入一个正整数：");
       var str="";
       if(parseInt(num) == num){
       　　while (num>0){
          　　var a = num%10;
             str += a;
             num = parseInt(num/10);
          }
          console.log(str);
       }else {
             console.log("您输入的数不是正整数!");
       }
```

**结语**

此为本人整理。

感谢您的观看，如有不足之处，欢迎批评指正。