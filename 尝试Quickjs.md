> Quickjs在我看来这个已经不是前端领域了，是一个能基于js开发多端的解决方案。会根据你编写的js逻辑构建成一个可执行的应用，类似exe这样的可以运行的，只是Quickjs基于c解析js后生成的是在linux环境下的可执行文件。感觉能干很多事，但是如果你只懂js其实也做不出什么，因为很多系统底层的服务需要用c打通，Quickjs有提供相关扩展。如果有人能基于这个做一个物联网底层集成，插件化可使用js编写的话应该能发扬光大一波😀



![b](https://i.loli.net/2019/07/12/5d28013cc403684951.png)

看到阮老师这个微博和评论后对Quickjs提起了兴趣。当我点进[网站](https://bellard.org/quickjs/)看到Features时，感觉作者是真的牛逼。

## Quickjs干了什么？

它能把js构建为二进制可执行文件，能运行至任何地方，也可以基于WASM运行在浏览器上。这是大大地扩展了js的可移植性，在我看来牛逼得不行！

还不单单是这样，对于js还支持ES2019和做了一些数学计算的扩展（支持BigInt, BigFloat）, 底层部分对os支持虽然不多但是基本够用，其他细节还有待实现。

## 尝试安装

下载[quickjs-2019-07-09](https://bellard.org/quickjs/quickjs-2019-07-09.tar.xz)

在MingGW的支持下，Win也能运行。下面例子执行环境在`ubuntu16.4`。

```
# in linux
cd ./quickjs-2019-07-09
sudo make install
```

跑完大概是长这样子的：

![d](https://i.loli.net/2019/07/12/5d2830255daee53000.png)

### 意外

在安装的过程中出现一些问题：

```
In file included from /usr/include/stdlib.h:24:0,
                 from qjs.c:25:
/usr/include/features.h:367:25: fatal error: sys/cdefs.h: 没有那个文件或目录
compilation terminated.
Makefile:269: recipe for target '.obj/qjs.m32.o' failed
make: *** [.obj/qjs.m32.o] Error 1
```

提示出错没有找到`sys/cdefs.h`文件。

找到原因是缺少c标准库的安装，只需要把库安装了就好了。

```
sudo apt-get install  build-essential libc6-dev libc6-dev-i386
```

## 使用

安装后提供4个不同命令：

- qjs 执行js
- qjsc 构建js为可执行文件
- qjsbn 执行js，具有数学扩展的相应解释器和编译器
- qjsnbc 构建js为可执行文件，具有数学扩展的相应解释器和编译器

例子：

```
qjs examples/hello.js
> Hello World

qjsc -o hello examples/hello.js
./hello
> Hello World
```

使用的例子还是官方提供的简单例子，至于具体可用的场景及支持还有待调研，Quickjs的实用价值目前还没办法估量。

我这种小弟只能先膜拜一下大神的成果。