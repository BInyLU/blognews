> Reptile-baidu是博主自写的node爬虫程序。
>
> 基于用户输入的关键字自动下载百度图库里的图片，省去在网站上下载的麻烦事~



**请确保node环境已安装再进行下面的步骤**

1、安装依赖

```
npm install
```

2、在  `test.js` 中输入图片关键字

```
let downImg = require("./index");

downImg({
    word : "在这里输入图片关键字"
});
```

3、运行 `test.js` 

```
node test.js
```

4、然后node程序就会在当前文件夹创建存放图片的文件夹，打开就能看见下载好的图片。