* 了解Webpack相关
  * 什么是webpack
    * Webpack是一个模块打包器(bundler)。
    * 在Webpack看来, 前端的所有资源文件(js/css/img/less/...)都会作为模块处理
    * 它将根据模块的依赖关系进行静态分析，生成对应的静态资源
  * 理解Loader
    * Webpack 本身只能加载JS模块，如果要加载其他类型的文件(模块)，就需要使用对应的loader 进行转换/加载
    * Loader 本身也是运行在 node.js 环境中的 JavaScript 模块
    * 它本身是一个函数，接受源文件作为参数，返回转换的结果
    * loader 一般以 xxx-loader 的方式命名，xxx 代表了这个 loader 要做的转换功能，比如 json-loader。
  * 配置文件(默认)
    * webpack.config.js : 是一个node模块，返回一个 json 格式的配置信息对象
  * 插件
    * 插件件可以完成一些loader不能完成的功能。
    * 插件的使用一般是在 webpack 的配置信息 plugins 选项中指定。
    * Webpack 本身内置了一些常用的插件，还可以通过 npm 安装第三方插件
* 学习文档 : 
  * webpack官方入门: http://webpack.github.io/docs/tutorials/getting-started/
  * Webpack中文指南: http://zhaoda.net/webpack-handbook/index.html
* 你将学到:
  * How to install webpack
  * How to use webpack
  * How to use loaders
  * How to use the development server
  * How to use image
* 初始化项目: package.json
  ```   
  {
    "name": "webpack_test",
    "version": "1.0.0"
  } 
  ```
* 安装webpack
  ```
  npm install webpack@1 -g  //全局安装
  npm install webpack@1 --save-dev  //局部安装
  ```
* 开始编译
  * 创建入口src/js/ : entry.js
    ```
    document.write("entry.js is work");
    ```
  * 创建主页面 build/: index.html
    ```
    <script type="text/javascript" src="js/build.js"></script>
    ```
  * 编译js
    ```
    webpack src/js/entry.js build/js/build.js  
    ```
  * 查看页面效果
* 第二个js
  * 创建第二个js: src/js/content.js
    ``` 
    module.exports = " <br> It works from content.js";
    ```
  * 更新入口js : entry.js
    ```
    * document.write("entry.js is works.");
    * document.write(require("./content.js"));
    ```
  * 编译js:
    ```
    webpack src/js/entry.js build/js/build.js
    ```
  * 查看页面效果
* 第一个加载器(loader)
  * 安装样式的loader
    ```
    npm install css-loader style-loader --save-dev
    ```
  * 创建样式文件: src/css/test.css
    ```
    body {
      background: red;
    }
    ```
  * 更新入口js : entry.js
    ```
    + require("style-loader!css-loader!../css/test.css");
    * document.write("entry.js is works.");
    * document.write(require("./content.js"));
    ```
  * 编译js, 并查看页面效果
    ```
    webpack src/js/entry.js build/js/bundle.js
    ```
* 绑定加载器
  * 更新入口js : entry.js
    ```
    - require("style-loader!css-loader!./test.css");
    + require("./test.css");
    ```
  * 编译:
    ```
    webpack src/js/entry.js build/js/bundle.js --module-bind css=style-loader!css-loader
    ```
  * 查看页面效果
* 使用webpack配置文件
  * 创建webpack.config.js
    ```
    module.exports = {
      entry: "./src/js/entry.js",//入口路径配置
      output: {//出口配置
          path: __dirname + '/build/js',//输出路径
          filename: "bundle.js"//输出文件名
      },
      module: {
        loaders: [//模块加载器配置
          { test: /\.css$/, loader: "style!css"}  //所有css文件声明使用css-loader和style-loader加载器
        ]
      }
    };
    ```
  * 编译
    ```
    webpack
    webpack --progress   //编译显示进度
    ```
  * 问题---每次修改模块文件内容的重新手动打包，刷新
  * 自动编译
    ```
    webpack --watch  //编译并启动监视(但需要刷新浏览器)
    
    ```
  * 浏览器自动刷新(热加载)----解决手动刷新浏览器问题
    ```
    npm install webpack-dev-server@1 -g
    webpack-dev-server
      访问: http://localhost:8080/webpack-dev-server/
          ; http://localhost:8080
      !!!发现问题----访问的是文件夹路径而不是页面
        devServer:{
              contentBase: './build',//内置服务器动态加载页面所在的目录
              historyApiFallback:true,//不跳转
              inline:true
          }
          
        contentBase : 默认webpack-dev-server为根文件夹提供内置的服务，如果其他目录下的文件
                      提供服务需要在此设置目录(我们设置为build文件夹)
        historyApiFallback : 开发单页面的时候非常有用，它依赖于H5的 history API，当
                            设置为true的时候所有的跳转都指向index.html；
        port：可以设置端口号，不设置时候默认为 8080
        inline ： 设置为true的时候回自动刷新(有的版本需要配合hot : true使用)
        
      webpack-dev-server
      访问: http://localhost:8080
      
     问题：页面没有热加载，自动刷新，因为index的src引入的硬盘中的build文件而不是webpack-dev-server服务器内存中的build
    特点:
      自动编译并刷新界面
      不生成编译后的文件, 直接在内存中编译处理, 并启动服务器运行项目
    ```
* 加载图片
    * 安装依赖的loader
      ```
      npm install url-loader file-loader --save-dev
      url-loader比file-loader功能更加完善是对file-loader的上层封装,但二者需配合使用
      ```
    * 添加config中loader的配置
      ```
      { test: /\.(png|jpg)$/, loader: "url-loader?limit=8192" }  //如果图片小于limit就会进行Base64编码
      ```
    * 拷入2张图片: 
      * 小图: img/logo.png
      * 大图: img/big.jpg
    * 定义引用图片的样式: test.css
      ```
      #box1{
        background-image: url(../img/logo.jpg);
        height: 200px;
        width: 200px;
      }
      #box2{
        background-image: url(../img/big.jpg);
        height: 200px;
        width: 200px;
      }
      ```
    * 在页面引用样式或图片: index.html
      ```
      <div id="box1"></div>
      <div id="box2"></div>
      ```
    * 编译并浏览
      ```
      webpack-dev-server
      ```
    * 图片打包问题
        ```
        1、图片大于8kb的时候无法打包到js文件中，
        2、index.html引入js的时候发现没有打包进去的图片路径不对
        3、解决方法： 
            * //publicPath : './js/',//webpack的绝对路径,设置服务index.html资源的路径
        ```
* 配置npm script命令
  ``` 
  "scripts": {
    "start": "webpack-dev-server",
    "build": "webpack"
  }
 
  npm start: 编译运行项目
  npm build: 编译打包
  
  ```
