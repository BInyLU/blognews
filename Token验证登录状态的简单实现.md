## Token验证登录状态的简单实现

**传统身份验证的方法：**
HTTP 是一种没有状态的协议，也就是它并不知道是谁是访问应用。这里我们把用户看成是客户端，客户端使用用户名还有密码通过了身份验证，不过下回这个客户端再发送请求时候，还得再验证一下。解决的方法就是，当用户请求登录的时候，如果没有问题，我们在服务端生成一条记录，这个记录里可以说明一下登录的用户是谁，然后把这条记录的 ID 号发送给客户端，客户端收到以后把这个 ID 号存储在 Cookie 里，下次这个用户再向服务端发送请求的时候，可以带着这个 Cookie ，这样服务端会验证一个这个 Cookie 里的信息，看看能不能在服务端这里找到对应的记录，如果可以，说明用户已经通过了身份验证，就把用户请求的数据返回给客户端。上面说的就是 Session，我们需要在服务端存储为登录的用户生成的 Session ，这些 Session 可能会存储在内存，磁盘，或者数据库里。我们可能需要在服务端定期的去清理过期的 Session 。

 

**Token**
1、用户第一次登录，服务器通过数据库校验其UserId和Password合法，则再根据随机数字+userid+当前时间戳 再经过DES加密生成一个token串

2、Token是在服务端产生的。如果前端使用用户名/密码向服务端请求认证，服务端认证成功，那么在服务端会返回Token给前端。前端可以在每次请求的时候带上 Token 证明自己的合法地位

3、token的生成一般是采用uuid保证唯一性，当用户登录时为其生成唯一的token，存储一般保存在数据库中。token过期时间采用把token二次保存在cookie或session里面，根据cookie和session的过期时间去维护token的过期时间。

4、Token，就是令牌，最大的特点就是随机性，不可预测。一般黑客或软件无法猜测出来。

 

**Token的身份验证方法**

使用基于 Token 的身份验证方法，在服务端不需要存储用户的登录记录(只保留用户签名过程中的加密算法和密钥即可)。

大概的流程是这样的：

1、当客户端第一次请求时，发送用户信息至服务器(用户名、密码)，服务器对用户信息使用HS256算法及密钥进行签名，再将这个签名和数据一起作为Token一起返回给客户端

2、服务器不保存Token，客户端保存Token(比如放在 Cookie 里或者 Local Storage 里)

3、当客户端再次发送请求时，在请求信息中将Token一起发给服务器

4、服务器用同样的HS256算法和同样的密钥，对数据再进行一次签名，和客户端返回的Token的签名进行比较，如果验证成功，就向客户端返回请求的数据


请求参数中带token
1、用户在调用需要登录操作的接口时，无需传递userid和password即可完成操作（因为token代表登录成功）

2、服务器控制过期时间，假如一个极端情况，服务器端的token规则泄露，则可以控制用户可以重新登录，获取新的token



**token的优势**
无状态、可扩展：
​    1、在客户端存储的Tokens是无状态的，并且能够被扩展。基于这种无状态和不存储Session信息，负载均衡器能够将用户信息从一个服务传到其他服务器上(session是存在服务器里面的，因此不能再服务器间共享，若负载均衡器将用户请求路由到其他服务器上后，服务器就不能识别)。
   2、如果我们将已验证的用户的信息保存在Session中，则每次请求都需要用户向已验证的服务器发送验证信息(称为Session亲和性)。用户量大时，可能会造成一些拥堵。
安全性
   3、请求中发送token而不再是发送cookie能够防止CSRF(跨站请求伪造)。即使在客户端使用cookie存储token，cookie也仅仅是一个存储机制而不是用于认证。不将信息存储在Session中，让我们少了对session操作。
   4、token是有时效的，一段时间之后用户需要重新验证。我们也不一定需要等到token自动失效，token有撤回的操作，通过token revocataion可以使一个特定的token或是一组有相同认证的token无效。



**token如何保证安全**
服务器端：不多说了，代码安全。
客户端：     
​    ①本地存储  token对称加密(因为Android一般存在SharedPreference里)

    ①Apk加固 防止被反编译，动态调试等等
    
    ③传输安全采用https方式，且最好双向验证，如果没有你还不想被别人抓到看到原始数据，那就至少要做一下对称加密。如果你是Http方式一定记得至少要对称加密再传输数据。


备注：
1、Token一般用在两个地方:
​    ①防止表单重复提交
​    ①anti csrf攻击（跨站点请求伪造）

如果应用于“anti csrf攻击”，则服务器端会对Token值进行验证，判断是否和session中的Token值相等，若相等，则可以证明请求有效，不是伪造的。
如果应用于“防止表单重复提交”，服务器端第一次验证相同过后，会将session中的Token值更新下，若用户重复提交，第二次的验证判断将失败，因为用户提交的表单中的Token没变，但服务器端session中Token已经改变了。比如，应对“重复提交”时，当第一次提交后便把已经提交的信息写到cookie中，当第二次提交时，由于cookie已经有提交记录，因此第二次提交会失败。

2、运用Token服务器就不用保存session ID了，只负责生成Token，然后验证Token。Token通常被放在cookie中，如果客户端不支持cookie，Token也可以放在请求头中。和cookie一样，为了数据安全性Token中不应该放如密码等敏感信息，可以通过抓包工具获取Token值


**Json Web Token是干什么**

简称JWT，在HTTP通信过程中，进行身份认证。(是一种开放标准。用于将各方之间的信息传输为JSON对象，该信息通过数字签名进行验证，使用HMAC算法或使用RSA的公钥/私钥对JWT进行前面，是rest接口的一种安全策略。允许我们使用JWT在用户和服务器之间传递安全可靠的信息。)

我们知道HTTP通信是无状态的，因此客户端的请求到了服务端处理完之后是无法返回给原来的客户端。因此需要对访问的客户端进行识别，常用的做法是通过session机制：客户端在服务端登陆成功之后，服务端会生成一个sessionID，返回给客户端，客户端将sessionID保存到cookie中，再次发起请求的时候，携带cookie中的sessionID到服务端，服务端会缓存该session（会话），当客户端请求到来的时候，服务端就知道是哪个用户的请求，并将处理的结果返回给客户端，完成通信。
通过上面的分析，可以知道session存在以下问题：
​      1、session保存在服务端，当客户访问量增加时，服务端就需要存储大量的session会话，对服务器有很大的考验；
​      2、当服务端为集群时，用户登陆其中一台服务器，会将session保存到该服务器的内存中，但是当用户的访问到其他服务器时，会无法访问，通常采用缓存一致性技术来保证可以共享，或者采用第三方缓存来保存session，不方便。

 

**Json Web Token是怎么做的**
​     1、客户端通过用户名和密码登录服务器；
​      2、服务端对客户端身份进行验证；
​      3、服务端对该用户生成Token，返回给客户端；
​      4、客户端将Token保存到本地浏览器，一般保存到cookie中；
​      5、客户端每次发起请求，需要携带该Token(让服务器知道此次操作是谁--服务器对其解码和解密并验证签名后对Token内的信息进行验证，验证通过后则返回信息)
​      6、服务端收到请求后，首先验证Token，之后返回数据。

服务端不需要保存Token，只需要对Token中携带的信息进行验证即可；无论客户端访问后台的那台服务器，只要可以通过用户信息的验证即可。


适用场景
1、用于向Web应用传递一些非敏感信息。例如完成加好友、下订单的操作等等。

2、用于设计用户认证和授权系统。

3、实现Web应用的单点登录。

 

**实例场景**
在A用户关注了B用户的时候，系统发邮件给B用户，并且附有一个链接“点此关注A用户”。链接的地址：https://your.awesome-app.com/make-friend/?from_user=B&target_user=A。让B用户不用登录就可以完成这个操作。

JWT的数据结构

1、它是一个很长的字符串，中间用点（.）分隔成三个部分。注意，JWT内部是没有换行的，并且都使用Base64编码
2、WT的三个部分依次如下。Header（头部）、Payload（负载）、Signature（签名）


Header
Header部分是一个JSON对象，描述JWT最基本的信息，例如，其类型及签名所用的算法等。通常是下面的样子

```
{
  "alg": "HS256",
  "typ": "JWT"
}
```


注：
alg属性表示签名的算法（algorithm），默认是 HMAC SHA256（写成 HS256）；typ属性表示这个令牌（token）的类型（type），JWT令牌统一写为JWT。最后，将上面的JSON对象使用Base64URL算法转成的字符串。


Payload
Payload部分也是一个JSON对象也是使用Base64URL算法转成的字符串，用来存放实际需要传递的数据。JWT规定了7个官方字段，供选用。
例子：

```
{
"iss": "John Wu JWT",
"iat": 1441593502,
"exp": 1441594722,
"aud": "www.example.com",
"sub": "jrocket@example.com",
"from_user": "B",
"target_user": "A"
}
```


iss (issuer)	该JWT的签发者
exp (expiration time)	过期时间(为一个UNIX时间戳)
sub (subject)	该JWT所面向的用户
aud (audience)：	接受该JWT的一方
nbf (Not Before)	生效时间
iat (Issued At)	签发时间
jti (JWT ID)	编号
除了官方字段，你还可以在这个部分定义私有字段，下面就是一个例子。

```
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```


注意，JWT默认是不加密的，任何人都可以读到，所以不要把秘密信息放在这个部分。


签名
将上面的两个编码后的字符串都用点号连接在一起（头部在前），就形成了，

例如：eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmcm9tX3VzZXIiOiJCIiwidGFyZ2V0X3VzZXIiOiJBIn0最后，我们将上面拼接完的字符串用HS256算法进行加密。在加密的时候，我们还需要提供一个密钥（secret）。如果我们用mystar作为密钥的话，那么就可以得到我们加密后的内容rSWamyAYwuHCo7IFAgd1oRpSP7nzL7BF5t7ItqpKViM这一部分又叫做签名。后将这一部分签名也拼接在被签名的字符串后面，

我们就得到了完整的JWT：eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmcm9tX3VzZXIiOiJCIiwidGFyZ2V0X3VzZXIiOiJBIn0.rSWamyAYwuHCo7IFAgd1oRpSP7nzL7BF5t7ItqpKViM于是，我们就可以将邮件中的URL改成https://your.awesome-app.com/make-friend/?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmcm9tX3VzZXIiOiJCIiwidGFyZ2V0X3VzZXIiOiJBIn0.rSWamyAYwuHCo7IFAgd1oRpSP7nzL7BF5t7ItqpKViM


JWT 的几个特点
（1）JWT默认是不加密，但也是可以加密的。生成原始Token以后，可以用密钥再加密一次。

（2）JWT不加密的情况下，不能将秘密数据写入 WT。

（3）JWT不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。

（4）JWT的最大缺点是，由于服务器不保存session状态，因此无法在使用过程中废止某个token，或者更改token的权限。也就是说，一旦JWT签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

（5）JWT本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。

（6）为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。
（7）便于传输，jwt的构成非常简单，字节占用很小，所以它是非常便于传输的
（8）它不需要在服务端保存会话信息，所以它易于应用的扩展


单点登录
Session方式来存储用户id，一开始用户的Session只会存储在一台服务器上。对于有多个子域名的站点，每个子域名至少会对应一台不同的服务器，例如：www.taobao.com、nv.taobao.com、nz.taobao.com、login.taobao.com所以如果要实现在login.taobao.com登录后，在其他的子域名下依然可以取到Session，这要求我们在多台服务器上同步Session。

使用JWT的方式则没有这个问题的存在，因为用户的状态已经被传送到了客户端。因此，我们只需要将含有JWT的Cookie的domain设置为顶级域名即可，例如

1
Set-Cookie: jwt=lll.zzz.xxx; HttpOnly; max-age=980000; domain=.taobao.com
注意domain必须设置为一个点加顶级域名，即.taobao.com。这样，taobao.com和*.taobao.com就都可以接受到这个Cookie，并获取JWT了。

 

## 登录创建Token的原理

### 测试

【login.html】



![img](https:////upload-images.jianshu.io/upload_images/13065313-391b201b45cf7267.png?imageMogr2/auto-orient/strip|imageView2/2/w/458/format/webp)



```xml
<!DOCTYPE html>
<html>  
<head>
<title>Login</title>
<link rel="stylesheet" href="../res/css/login.css">
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/jquery-cookie/1.4.1/jquery.cookie.js"></script>
</head>
<body>
    <form>
        <input type="text" name="username" id="username">
        <input type="password" name="password" id="password">
    </form>
    <input type="button" value="Login" onclick="login()">
</body>
<script type="text/javascript">
function login(){
    $.ajax({
        url: "/tokenAuth/token",
        dataType: "json",
        data: {'username':$("#username").val(), 'password':$("#password").val()},
        type:"GET",
        success:function(res){
            console.log(res);
            if(res.code == 200){
                var authStr = res.data.userId + "_" + res.data.token;
                //把生成的token放在cookie中
                $.cookie("authStr", authStr);
                window.location.href = "index.html";
            }else alert(res.msg);
        }
    });
}
</script>
</html>
```

【index.html】



![img](https:////upload-images.jianshu.io/upload_images/13065313-95c203962eab5c44.png?imageMogr2/auto-orient/strip|imageView2/2/w/137/format/webp)



```xml
<!DOCTYPE html>
<html>  
<head>
<title>Index</title>
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/jquery-cookie/1.4.1/jquery.cookie.js"></script>
</head>
<body>
    <input type="button" value="Get" onclick="get()">
    <input type="button" value="logout" onclick="logout()">
</body>
<script type="text/javascript">

function get(){
    $.ajax({
        url: "/tokenAuth/user/bpf",
        dataType: "json",   
        type:"GET",
        beforeSend: function(request) {
            //将cookie中的token信息放于请求头中
            request.setRequestHeader("authStr", $.cookie('authStr'));
        },
        success:function(res){
            console.log(res);
        }
    });
}

function logout(){
    $.ajax({
        url: "/tokenAuth/token",
        dataType: "json",   
        type:"DELETE",
        beforeSend: function(request) {
            //将cookie中的token信息放于请求头中
            request.setRequestHeader("authStr", $.cookie('authStr'));
        },
        success:function(res){
            console.log(res);
        }
    });
}
</script>
</html>
```

测试环境中两个页面login.html和index.html均当做静态资源处理
 【未登录状态】

- 首先再未登录状态下直接访问index页面<http://localhost:8080/tokenAuth/page/index.html> 

- 点击get按钮获取数据，由于没有携带token导致认证失败

- 

  ![img](https:////upload-images.jianshu.io/upload_images/13065313-3124f40d1b898258.png?imageMogr2/auto-orient/strip|imageView2/2/w/380/format/webp)


【登录状态】

- 访问登录网站<http://localhost:8080/tokenAuth/page/login.html>，输入username和password进行点击Login按钮登录

- 登录成功并跳转到index页面，并且生成cookie，这里没有设置cookie有效期，默认关闭浏览器失效



  ![img](https:////upload-images.jianshu.io/upload_images/13065313-720a84c7ad05f426.png?imageMogr2/auto-orient/strip|imageView2/2/w/506/format/webp)

- 再次点击get按钮请求数据，请求成功



  ![img](https:////upload-images.jianshu.io/upload_images/13065313-a75b77b35aa6c8c5.png?imageMogr2/auto-orient/strip|imageView2/2/w/452/format/webp)

- 点击logout按钮销毁登录状态，然后再次请求数据



  ![img](https:////upload-images.jianshu.io/upload_images/13065313-1564f2b03a1dba78.png?imageMogr2/auto-orient/strip|imageView2/2/w/373/format/webp)