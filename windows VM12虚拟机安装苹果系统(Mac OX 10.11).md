# windows VM12虚拟机安装苹果系统(Mac OX 10.11)



本人最近需要使用苹果系统学习开发React Native，由于没有苹果电脑，只能安装个黑苹果对付一下了，以下是本人的经历。

首先需要工具：

1、vm12安装包下载：https://pan.baidu.com/share/init?surl=skOlZv3 提取码tcua；

2、unlocker208工具下载：https://pan.baidu.com/share/init?surl=bptgDI3 提取码：ja2q

3、映像文件下载推荐：https://pan.baidu.com/s/1eRKScvC提取码：dqbi

VM12的注册码自行百度，下载安装完vm12后先不要着急打开，首先打开unlocker208z中的win-install,记住这步很关键，关系到之后能不能找到ox印象文件。

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/1.png)



然后打开安装好的VM12。

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/2.png)

选择ox映象的安装位置

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/3.png)

如果这块不显示就是之前unlocker208的问题，多执行几遍或者重新下载一个

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/4.png)

安装位置任意，名称任意

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/5.png)

无脑下一步

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/6.png)

点击完成！
打开虚拟机之后会提示错误，如下图，

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/7.png)

这需要我们将虚拟机安装位置中的OS X 10.12以记事本打开，手动在smc.present="TRUE"下面添加smc.version=0,然后保存即可。

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/8.png)

这个时候我们在打开虚拟机就不会出现之前的问题了,进入系统选择语言

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/9.png)

然后选择实用工具--->磁盘工具

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/10.png)

然后在磁盘里面对磁盘进行分区

![img](https://raw.githubusercontent.com/BInyLU/webimg/master/20190601/11.png)

之后就是基本的配置了，祝你安装成功。

