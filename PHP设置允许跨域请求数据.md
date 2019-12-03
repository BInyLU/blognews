## PHP设置允许跨域请求例子

> 考虑到平时开发都是在本地调试的，所以做前后端分离难免逃避跨域问题，今天我就总结如何从后端角度允许前端进行跨域请求数据。



**方法一：在后端进行头部允许**

```php
<?php
    header('Content-Type: text/html;charset=utf-8');
    header('Access-Control-Allow-Origin:*'); // *代表允许任何网址请求
    header('Access-Control-Allow-Methods:POST,GET,OPTIONS,DELETE'); // 允许请求的类型
    header('Access-Control-Allow-Credentials: true'); // 设置是否允许发送 cookies
    header('Access-Control-Allow-Headers: Content-Type,Content-Length,Accept-Encoding,X-Requested-with, Origin'); // 设置允许自定义请求头的字段
?>
```



**方法二：在后端进行输出函数调用**

```php
<?php
$arr = array(
  'name' => '魏艳辉',
    'nick' => '为梦翱翔',
     'contact' => array(
         'email' => 'zhuoweida@163.com',
         'website' => 'http://zhuoweida.blog.tianya.cn',
      )
    );
$json_string = json_encode($arr);
echo "getProfile($json_string)";
?>
```

```html
<script type="text/javascript" src="http://localhost/demo/profile.php"></script>
<script type="text/javascript">
  //前端调用
  function getProfile(str) {
      var arr = str;
      document.getElementById('nick').innerHTML = arr.nick;
  }
</script>
<body>
   <div id="nick"></div>
</body>
```

