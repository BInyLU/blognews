### 一、发布一个新包

**第一步：进入要发布的项目根目录，初始化为npm包：**

npm init

依次按提示填入包名、版本、描述、github地址、关键字、license等

![img](https://img-blog.csdn.net/20180908174017676?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这步完成之后会生成一个package.json文件，上面输入的这些信息可以在该文件中修改

注意：如果你的包引用了第三方包，则需要在package.json文件种增加dependencies节点，写入依赖的包及版本

"dependencies": {
​    "colors": "^1.3.2",
​    "on-finished": "^2.3.0"
  }
**第二步、注册npm用户，有两种方法**

方法一、npm官网注册：npm

方法二、使用npm 命令注册：npm adduser

![img](https://img-blog.csdn.net/20180908174700543?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注意：如果用户名被别人注册过，那么回报如下错误：

Unable to authenticate, need：Basic

![img](https://img-blog.csdn.net/20180908174812993?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注意：用第二种方法注册的用户登录后，发布包时候会报如下错，只能使用方法一，去官网注册

 'mypackage1' is not in the npm registry.

![img](https://img-blog.csdn.net/20180908175254813?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**第三步、账号登录**

npm login

依次输入第二步中第一种方法注册的用户名、密码和邮箱

![img](https://img-blog.csdn.net/20180908175711814?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**第四步、发布包，上传到npm包服务器**

npm publish

注意：如果报错：'You do not have permission to publish "mypackage1". Are you logged in as the correct user?'

表示包’mypackage1‘已经在包管理器已经存在被别人用了，需要更该包名称

![img](https://img-blog.csdn.net/20180908160323732?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

包名改为：mypackage_tao,再次发布

’+’符合表示发布成功了

![img](https://img-blog.csdn.net/20180908180438792?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以去自己的npm主页上验证以下，可以看到包mypackage_tao已经在列表中了

![img](https://img-blog.csdn.net/20180908180743385?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注意：如果发布时报错：‘no_perms Private mode enable, only admin can publish this module:’

表示当前不是原始镜像，可能用的是其他镜像，如淘宝镜像，

要切换回原始的npm镜像，命令：npm config set registry https://registry.npmjs.org/，如果用了nrm工具，使用命令：nrm use npm 切换

![img](https://img-blog.csdn.net/20180908160757199?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

至此，发布自己的一个新包已经大功告成了,然后别人就可以通过npm install mypackage_tao 来安装你的包了。后续包要更新怎么办呢？往下看

### 二、更新一个已经发布的包

**第一步、修改包的版本**

：这次我在包根目录下新加了一个index.js文件

npm version patch  该命令在原来的版本上自动加1,实际上是将package.json文件中的version值修改了。

![img](https://img-blog.csdn.net/20180908182358107?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**第二步、重新发布包**

npm publish

![img](https://img-blog.csdn.net/20180908183107250?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看到，已经有两个版本了

![img](https://img-blog.csdn.net/20180908190448760?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如果我发现版本1.0.1有bug，要删除，怎么办呢？往下看

### 三、删除包

**1、删除指定的版本**

npm unpublish 包名@版本号

![img](https://img-blog.csdn.net/20180908183840537?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看到版本V1.0.1已经删除

![img](https://img-blog.csdn.net/20180908190516290?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**2、删除整个包**

npm unpublish 包名 --force

会有警告提示

![img](https://img-blog.csdn.net/20180908184456991?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看到包mypackage_tao已经删除了

![img](https://img-blog.csdn.net/20180908190859924?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhb2VyY2h1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

---------------------
作者：peachesTao 
来源：CSDN 
原文：https://blog.csdn.net/taoerchun/article/details/82531549 
版权声明：本文为博主原创文章，转载请附上博文链接！