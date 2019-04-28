> 本文转载：<https://www.cnblogs.com/furzoom/p/mvc-php.html>
>
> 在PHP中使用MVC越来越流行了，特别是在一些开源的框架当中。
>
> MVC足以应对大多数的情况，但还有一些情况是其不太适合的，如比较简单的个人博客，对于只有几百篇文章量级的博客，使用MVC让人觉得有些太复杂了；同样对于新浪等门户网站，使用MVC，将有大量的文件被加载，对于速度的影响是无法接受的。
>
> 枫竹梦介绍MVC的基本原理及一种简单的实现。如下介绍内容适用PHP开发。

 

**PHP中的MVC**

MVC在软件工程中是一种软件的架构。从php的角度来讲MVC有一些不同。

Model(模型)，程序应用功能的实现，程序的逻辑的实现。在PHP中负责数据管理，数据生成。

View(视图)，图形界面逻辑。在PHP中负责输出，处理如何调用模板、需要的资源文件。

Controller(控制器)，负责转发请求，对请求处理。在PHP中根据请求决定调用的视图及使用的数据。

 

**为什么使用MVC**

MVC的主要作用是为了将代码分层、分类。

MVC的主要目的是为了解决Web开发中分离开发与设计工作，使其工作相对独立。

在这样的过程中还发现了其他的一些优点，网站的目录结构更加清晰，网站更易维护与扩展，可以实现模块的复用。

 

**MVC的实现操作**

 

**网址例子：**

> **请求URL**
>
> 首先，约定请求页面时的URL，以如下结构进行实现：
>
> localhost/index.php?c=demo&a=index&param=welcome
>
> 如果想得到更加优美的URL结构，可以进行优化，为由这URL结构优化与本文关系不大，以后进行分享。
>
> 从上面的参数可以看出，访问的文件是index.php，同时含有3个参数分别为c、a、param。

 

**MVC目录结构**

接着，规划MVC的目录结构如下：

 

> ├─www # 网站根目录
>
> │ ├─controller # 控制器目录
>
> │ │ ├─democontroller.php # demo控制器
>
> │ ├─model # 模型目录
>
> │ │ ├─model.php # model模型
>
> │ ├─view # 视图目录
>
> │ │ ├─index.php # index视图
>
> │ ├─index.php # 入口文件



 

**控制器controller**

将如下代码添加到controller/democontroller.php文件中。

代码如下:



```

```









```
 // controller/democontroller.php
 class DemoController
 {
     public function index()
     {
     echo 'hello world';
     }
 }// End of the class DemoController
 // End of file democontroller.php
```



在这个文件中仅仅定义了一个DemoController的类，且其只包含一个index方法，用于输出hello world。

将下面代码添加到入口文件index.php文件中。

代码如下:



```

```









```
 //index.php
 require('controller/democontroller.php');
 $controller = new DemoController();
 $controller->index();
 // End of index.php
```



在浏览器中使用上面的约定的URL进行访问，看到输出hello world。当然如果你请求的URL不是那样，而是如下面所示也能得到同样的输出。

localhost/index.php?c=abc

发现URL中的参数还没有任何作用。

如果使用下面的URL进行访问，可以预见不会有任何输出。

localhost/controller/democontroller.php

可以看到要想访问这个网站并得到正确的结果，目前只能通过index.php来访问，这也是为什么称它为入口文件的原因。现在无论参数如何只能访问同样一个页面，那么如何来决定显示不同的结果呢？或者调用不同的控制器呢？

 

**改进入口文件**

下面利用URL中的参数来决定使用哪个控制器。

代码如下:



```

```









```
 //index.php
 // get runtime controller prefix
 $c_str = $_GET['c'];
 // the full name of controller
 $c_name = $c_str.'controller';
 // the path of controller
 $c_path = 'controller/'.$c_name.'.php';
 // get runtime action
 $method = $_GET['a'];
 // load controller file
 require($c_path);
 // instantiate controller
 $controller = new $c_name;
 // run the controller  method
 $controller->$method();
 // End of index.php
```



同样在浏览器中使用上面的约定的URL进行访问，看到输出hello world。代码中的注释已经说明了每一步的目的。当然可以通过改变URL参数中的c与a值来调用不同的controller及其方法，以输出不同的结果。

 

**视图View**

前面只是使用了控制器controller，同时在入口文件index.php中实现了动态调用不同的控制器。接着加入视图将显示分离。

代码如下:



```

```









```
 // view/index.php
 class Index {
     public function display($output) {
         // ob_start();
         echo $output;
     }
 }
 // End of index.php
```



视图目录中的index.php文件中定义了Index方法，且只有一个display()函数，负责将传递给它的变量进行输出。 下面修改控制器文件。

代码如下:



```

```









```
 // controller/democontroller.php
 class DemoController
 {
     private $data = 'Hello furzoom!';
     public function index()
     {
     //echo 'hello world';
     require('view/index.php');
     $view = new Index();
     $view->display($data);
     }
 }// End of the class DemoController
 // End of file democontroller.php
```



在控制器中定义了一个data私有变量，index()方法不再直接输出，而是使用视图对象处理输出。此时，按上面的约定的URL进行访问时，将看到输出：

Hello furzoom! 可以根据不同的请求调用不同的视图类，以不同的形式显示数据。这样就将增加了视图的作用，设计人员可以只针对视图进行页面的设计。

 

**模型Model**

上面貌似已经很cool了，但是显示什么样的内容是在控制器中直接指定的，希望内容也由URL指定，这样将数据的处理交给模型来处理。

代码如下:



```

```









```
 // model/model.php
 class Model {
     private $data = array(
                 'title' => 'Hello furzoom',
                 'welcome' => 'Welcome to furzoom.com',
                 );
     public function getData($key) {
         return $this->data[$key];
     }
 }
 // End of model.php
```



视图文件model.php定义了一个Model类，类中定义了一个getData()的方法，用于返回请求的数据。

同时修改入口文件index.php如下：

代码如下:



```

```









```
 //index.php
 // get runtime controller prefix
 $c_str = $_GET['c'];
 // the full name of controller
 $c_name = $c_str.'controller';
 // the path of controller
 $c_path = 'controller/'.$c_name.'.php';
 // get runtime action
 $method = $_GET['a'];
 // get runtime parameter
 $param = $_GET['param'];
 // load controller file
 require($c_path);
 // instantiate controller
 $controller = new $c_name;
 // run the controller  method
 $controller->$method($param);
 // End of index.php
```



增加了一个参数$param，将其作为控制器的方法调用参数。

还需要修改控制器的方法根据不同参数取得不同的数据。

代码如下:



```

```









```
 // controller/democontroller.php
 class DemoController
 {
     // private $data = 'Hello furzoom!';
     function index($param)
     {
     // echo 'hello world';
         require('view/index.php');
     require('model/model.php');
     $model = new Model();
     $view = new Index();
     $data = $model->getData($param);
     $view->display($data);
     }
 }// End of the class DemoController
 // End of file democontroller.php
```



包含需要的视图文件和模型文件，然后生成视图与模型文件，接着通过模型对象取得数据，再用视图对象来输出取得的数据。

此时，在浏览器中使用上面的约定的URL进行访问，将得到输出如下：

Welcome to furzoom.com

如下图：

![img](http://files.jb51.net/file_images/article/201502/201502051128556.png)

 

至此PHP的MVC模式已经基本介绍完成了，剩余的工作就是根据需要进行添加扩充了，很简单吧！！