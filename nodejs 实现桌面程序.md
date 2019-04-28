nodejs 实现桌面程序，这里主要使用electron

安装electron的包，这里如果一直安装不了，可能是网络的原因，可以下载[淘宝](https://www.baidu.com/s?wd=%E6%B7%98%E5%AE%9D&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)镜像：

```
npm install -g electron1
```

```
npm install cnpm -g --registry=http://registry.npm.taobao.org
cnpm install electron -g
```

安装electron-prebuilt :

```
cnpm install -g electron-prebuilt
```

安装 packager 发布工具:

```
cnpm install -g electron-packager
```

安装electron-builder:

```
cnpm install electron-builder -g
```

安装 aser 打包工具:

```
cnpm install -g asar
```

初始化项目:

```
npm init
```

生成了package.json，配置如下：

```
{
  "name": "app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
  	"test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
  	"electron": "^2.0.6"
  }
}
```

建立js程序 index.js :

```
const {app, BrowserWindow} = require('electron')
const path = require('path')
const url = require('url')

// 保存窗口对象的全局引用, 如果不这样做, 当JavaScript对象被当做垃圾回收时，window窗口会自动关闭
let win

function createWindow () {
  // 创建浏览器窗口.
  win = new BrowserWindow({width: 800, height: 600,autoHideMenuBar :true})

  win.setMenu(null);

  // 加载项目的index.html文件.
  win.loadURL(url.format({
    pathname: path.join(__dirname, 'index.html'),
    protocol: 'file:',
  // 当窗口关闭时候的事件.
    slashes: true
  }))

  // 打开开发工具.
  //win.webContents.openDevTools()

  win.on('closed', () => {
    // 取消引用窗口对象, 如果你的应用程序支持多窗口，通常你会储存windows在数组中，这是删除相应元素的时候。
    console.log("haha");

    win = null
  })
}

app.on('activate', () => {
  console.log('activate')
  if (win === null) {
    createWindow()
  } else {
    win.show()
  }
})

// 当Electron完成初始化并准备创建浏览器窗口时，将调用此方法
// 一些api只能在此事件发生后使用。
app.on('ready', createWindow)

// 当所有窗口关闭时退出。
app.on('window-all-closed', () => {
  // 在macOS上，用得多的是应用程序和它们的菜单栏，用Cmd + Q退出。
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
  // 在macOS上，当点击dock图标并且没有其他窗口打开时，通常会在应用程序中重新创建一个窗口。
  if (win === null) {
    createWindow()
  }
})
```

添加index.html 文件：

```
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="UTF-8"/>
    <title>test</title>
</head>
<body>
    Hello World!

    <button id="button">点击这里</button>

<script>
    console.log(window)

    var button = document.getElementById('button');

    button.onclick = function(){
            alert("你好")
    }

</script>
</body>
</html>
```

可以使用electron . 命令预览

```
electron .
```

```
npm run-scritpt package
```

执行打包命令：（可能需等待一会）

```
build --win --x64
```

打包后会在项目目录生成dist文件夹，生成exe文件，点击安装就可以了 。

![这里写图片描述](https://img-blog.csdn.net/20180803095633813?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NzY5Njc3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如果想要设置exe的显示图标，可以利用网上的在线[图片格式转换工具](https://www.baidu.com/s?wd=%E5%9B%BE%E7%89%87%E6%A0%BC%E5%BC%8F%E8%BD%AC%E6%8D%A2%E5%B7%A5%E5%85%B7&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)，将图片转换成256*256像素的ICO图片。

如图所示目录，新建image文件夹 

![这里写图片描述](https://img-blog.csdn.net/20180804193107710?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NzY5Njc3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

文件夹内放置ico图片

 ![这里写图片描述](https://img-blog.csdn.net/20180804193122860?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NzY5Njc3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

json配置：

```
{
  "name": "Play",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC","devDependencies": {
    "electron": "^2.0.6"
  },
  "build":{
    "appId": "com.gao.Play",
    "copyright": "Yimi",
    "productName": "Play",
    "dmg":{
      "window": {
        "x":100,
        "y":100,
        "width": 500,
        "height": 400
      }
    },
    "win": {
      "icon": "image/icon.ico"
    }
  }
}
```

执行build –win –x64命令后，就会生成有图标的执行应用。

就可以生成桌面快捷方式了。