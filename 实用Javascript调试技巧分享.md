> 见过太多同学调试Javascript只会用简单的 `console.log`甚至 `alert`，看着真为他们捉鸡。。因为大多数同学追求优雅而高效地写代码，却忽略了如何优雅而高效地调试代码，不得不说是有点“偏科”了。下面我就分享一些实用且聪明的调试技巧，希望能让大家调试自己代码的时候更加从容自信。

## 1. 不要使用 `alert`

首先， `alert`只能打印出字符串，如果打印的对象不是 `String`，则会调用 `toString()`方法将该对象转成字符串（比如转成 `[object Object]`这种），所以除非你打印 `String`类型的对象，其他什么信息都获取不到。其次， `alert`会阻塞UI和javascript的执行，必须点击'OK'按钮才能继续，非常低效。所以，喜欢使用 `alert`的同学可以改改这个习惯了。

## 2. 学会使用 `console.log`

`console.log`谁都会用，但是很多同学只知道最简单的 `console.log(x)`这样打印一个对象，当你的代码里面 `console.log`多了之后，会很难将某条打印结果和代码对应，所以我们可以给打印信息加上一个标签便于区分：

```
let x = 1;console.log('aaaaaaaa', x);
```

![https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/1.webp](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/1.webp)

得到：

![console.log labeled](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/2.webp)

标签不一定要有明确的含义，视觉效果显著就可以了，当然有明确意义更好。

事实上， `console.log`可以接收任意多的参数，最后将这些对象拼接输出，比如：

![console.log many params](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/3.gif)

如果打印信息过多，不容易找到目标信息的话，可以在控制台中进行过滤。

**注意点**

在使用 `console.log`打印一个引用类型（比如数组和自定义对象）的对象的时候，输出结果可能并不是执行 `console.log`方法那个时间点的值。举个例子：

![console.log reference](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/4.webp)

可以发现两个 `console.log`输出的结果展开后都是 `[1, 2, 3, 4]`，因为数组是引用类型，所以在展开后获取到的都是数组最新的状态。我们可以使用 `JSON.parse(JSON.stringify(...))`来解决这个问题：

![console.log json](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/5.webp)

## 3. 学会使用 `console.dir`

我们有时候想看看一个DOM对象里面到底有什么属性和方法，但是常规的 `console.log`打印出来的只是HTML标签，就像这样：

![console.log DOM](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/6.webp)

和直接审查元素没有什么区别。

如果我们想看到DOM对象作为JavaScript对象的结构可以使用 `console.dir`，比如：

![console.dir DOM](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/7.webp)

事实上， `console.dir`可以打印出任何JavaScript对象的属性列表，比如打印一个方法：

![console.dir function](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/8.webp)

## 4. 学会使用 `console.table`

我们经常会遇到这样的场景：获取到一个用户列表，每个用户有很多属性，但是我们只想查看其中的某些属性，在用 `console.log`打印出来的时候需要把每个用户对象展开一个个查看，非常麻烦。而 `console.table`完美的解决这个问题，比如我只想获取到下列用户的id和坐标：

**console.log打印结果：**

![console.log users](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/9.webp)

**console.table打印结果：**

![console table users](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/10.webp)

非常的准确和快速！

## 5. 学会使用 `console.time`

有时候我们想知道一段代码的性能或者一个异步方法需要运行多久，这时候需要用到定时器，JavaScript提供了现成的 `console.time`方法，例如：

![console.time](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/11.webp)

## 6. 使用 `debugger`打断点

有时候我们需要打断点进行单步调试，一般会选择在浏览器控制台直接打断点，但这样还需要先去**Sources**里面找到源码，然后再找到需要打断点的那行代码，比较费时间。使用 `debugger`关键词，我们可以直接在源码中定义断点，方便很多，比如：

![debugger](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/12.webp)

## 7. 查到源码文件

有时候我们想在控制台的 `Sources`中查找某个js源文件，要把文件夹逐一点开找，非常麻烦。其实Chrome提供了文件的搜索功能，只不过大部分时候我们给忽略了。。

![command p](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/13.webp)

只要按 `Command + P`（windows的快捷键请自行查看）就能弹出搜索框搜索你想要找的文件啦：

![command p open](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/14.webp)

## 8. 压缩JS文件的阅读

有时候我们需要在 `Sources`中阅读一段js代码，但是发现它被压缩了，Chrome也提供了和方便的格式化工具，让代码变得重新可读：

![unminify btn](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/15.webp)

**点完之后变成这样：**

![unminified](https://raw.githubusercontent.com/BInyLU/webimg/master/20190617/16.webp)

以上就是我个人在平时比较常用的一些调试小技巧，如果大家有其他好的调试技巧也欢迎分享，谢谢🙏！