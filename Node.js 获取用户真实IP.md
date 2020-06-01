### 前言

微信H5支付时候需要获取用户的真实IP，如果代码中报的IP与微信获取的IP不一致，那么此时微信就会报错。通常Node.js部署时候是通过Nginx本地转发，所以获取真实IP时候需要做以下配置。

### Nginx配置

```bash
# 只转发test
location /test/ {
    proxy_pass http://localhost:3000/;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Real-Port $remote_port;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

### Node.Js获取IP代码

首先本着避免造轮子的原则，使用了request-ip库。

```bash
    # 锁定版本号
    npm i request-ip --save --save-exact
```

然后就要获取IP了，以上都是准备工作

```jsx
    var requestIp = require('request-ip');
    var IP = requestIp.getClientIp(req),
```

可以愉快地微信支付了。

