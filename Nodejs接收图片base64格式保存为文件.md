## Nodejs接收图片base64格式保存为文件

base64的形式为`data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0。。。。`；
当接收到上边的内容后，需要将`data:image/png;base64`这段内容过滤掉，过滤成：`iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0。。。`；然后进行保存。

```javascript
app.post('/upload', function(req, res){
    //接收前台POST过来的base64
    var imgData = req.body.imgData;
    //过滤data:URL
    var base64Data = imgData.replace(/^data:image\/\w+;base64,/, "");
    var dataBuffer = new Buffer(base64Data, 'base64');
    fs.writeFile("image.png", dataBuffer, function(err) {
        if(err){
          res.send(err);
        }else{
          res.send("保存成功！");
        }
    });
});
```

