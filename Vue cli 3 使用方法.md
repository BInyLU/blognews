# vue cli 3 使用方法

## 创建vue 环境生成项目

1.随便找一个你觉得舒服的文件夹，打开命令行工具

```
npm install -g @vue/cli 
```

2.安装完理所当然要查看下是否安装成功，那么需要执行

```
vue --version
```

3.接下来就是重点了让我们来创建一个项目（命令与之前有所变化）

```
vue create hello-world
```

执行之后我们会看到如图所示界面，此时它会让我们选择default（默认）还是manually select features（手动）这里建议选择后者
![执行之后我们会看到这样的界面](https://img-blog.csdnimg.cn/20190110102214359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
选择后者回车之后会看到
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019011010273097.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)

这几个选项是我们需要加在的配置，这里我们把常用的几个选上（注意空格键是选中）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110103030543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)

然后回车
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110103105993.png)
这句话的意思是是否启用history作为router，我这里选择yes，如果history不是很了解的同学这里有传送门[history介绍](https://router.vuejs.org/zh/guide/essentials/history-mode.html)
我们继续回车，会看到css设置，这里我们选择第一个
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019011010391934.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)

接着执行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110104056142.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
这段是代码风格相关，这里我选择第三个然后继续执行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110104350192.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
这里让我们选择保存时检查
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110104625681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
这部分是让我们选择测试方式这里选择第二个，然后继续
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110105035419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
这里让我选择配置文件防止得地方我选则单独的文件，然后继续执行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110105152824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
这里让我们选择是否保存配置，我这里选择否（注意这里要明确输入N 否则会默认Y）接下来我们等待依赖包安装
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110105508331.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
安装完成会提示我们执行两条命令，进入文件夹启动项目

```
cd hello-world
npm run serve
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110105742296.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
这里会让我们打开链接，并不会直接启动浏览器，接下来我们手动打开一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110105914882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
好了此刻我们的项目就生成好了

## 项目文件及配置

那么接下来让我们看一下刚刚生成的项目结构吧。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110110527504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)

这里我们会看到，曾经我们熟悉的build文件不见了，config文件也不见了，如果你用过vue之前的版本，那么你一定会说，诶呦卧槽，老子怎么配置相关文件。不要急不要慌，楼主给你解决办法
第一步在项目的根目录下创建`vue.config.js`文件

```json
module.exports = {
    // 基本路径
    publicPath: '/',//注意新版本这里改成了publicpath
    // 输出文件目录
    outputDir: 'dist',
    // eslint-loader 是否在保存的时候检查
    lintOnSave: true,
    // use the full build with in-browser compiler?
    // https://vuejs.org/v2/guide/installation.html#Runtime-Compiler-vs-Runtime-only
    runtimeCompiler: false,
    // webpack配置
    // see https://github.com/vuejs/vue-cli/blob/dev/docs/webpack.md
    chainWebpack: () => {},
    configureWebpack: () => {},
    // vue-loader 配置项
    // https://vue-loader.vuejs.org/en/options.html
    // vueLoader: {},
    // 生产环境是否生成 sourceMap 文件
    productionSourceMap: true,
    // css相关配置
    css: {
     // 是否使用css分离插件 ExtractTextPlugin
     extract: true,
     // 开启 CSS source maps?
     sourceMap: false,
     // css预设器配置项
     loaderOptions: {},
     // 启用 CSS modules for all css / pre-processor files.
     modules: false
    },
    // use thread-loader for babel & TS in production build
    // enabled by default if the machine has more than 1 cores
    parallel: require('os').cpus().length > 1,
    // 是否启用dll
    // See https://github.com/vuejs/vue-cli/blob/dev/docs/cli-service.md#dll-mode
    // dll: false,
    // PWA 插件相关配置
    // see https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa
    pwa: {},
    // webpack-dev-server 相关配置
    devServer: {
     open: process.platform === 'darwin',
     host: '0.0.0.0',
     port: 8088,
     https: false,
     hotOnly: false,
     proxy: null, // 设置代理
     before: app => {}
    },
    // 第三方插件配置
    pluginOptions: {
     // ...
    }
   }
```

把这段代码复制进去重新启动项目你就会发现配置文件已经生效了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110111335838.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110111404686.png)

而且还可以自动打开浏览器哟，当然如果你有其他的配置这里就按照webpack的官方配置即可[vue cli 3 最新配置说明](https://cli.vuejs.org/guide/plugins-and-presets.html#plugins)，[webpack官网](https://www.webpackjs.com/concepts/)。接下来就是你随心所欲写代码的时间了。哦忘记说了如果你觉得命令行不是很熟悉那么vue很贴心的出了一个图形化界面创建项目的UI，（感觉前端距离失业又近了一步）。

## 图形化界面创建项目

1.安装vue环境是必须的
2.随便找个地方用命令行工具执行`vue UI`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110112335971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
这个时候浏览器中我们会看到这样的界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110112420502.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
然后就是软件常规操作
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110112519780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTc2NTkwMQ==,size_16,color_FFFFFF,t_70)
其实就是把命令行的那部分隐藏起来执行了一遍，接下来的就大家自己摸索吧。