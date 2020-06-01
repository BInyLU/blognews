**南宁师范大学教务系统 API参数：**

- 请求地址：<http://121.43.124.47:3001/nsd>
- 请求方式：GET
- 必须参数：username（账号）、password（密码）
- API说明：目前可以调用的数据只有个人信息查询、考试安排查询、学期课表查询；后面会逐一更新增加接口数据，如专业执行计划查询，空教室查询等。
- **API例子**：<http://121.43.124.47:3001/nsd?username=1915100317&password=xxxxx>



**小陆磁力搜 API参数：**

主站地址：http://121.43.124.47:3002

- 获取关键字列表结果：<http://121.43.124.47:3002/cili>
  - 必须参数：wd（关键字）
  - 可选参数：page（页码）
  - **API例子**：<http://121.43.124.47:3002/cili?wd=111&page=1>
- 指定ID请求地址：<http://121.43.124.47:3002/seed>
  - 必须参数：id（指定id值）
  - **API例子**：<http://121.43.124.47:3002/cili?id=wNWYor>
- 请求方式：GET
- API说明：BinySou 磁力搜索 是本人扒取某个磁力库得到的数据，经过整合API后，目前可以调用数据。



**bilibili 订阅动画API参数：**

- 请求地址：https://api.bilibili.com/x/space/bangumi/follow/list?type=1&follow_status=0&pn=1&ps=15&vmid=54200948
- 请求方式：GET
- 必须参数：vmid（B站UID账号）



**bilibili 个人信息API参数：**

- 请求地址1：https://api.bilibili.com/x/space/acc/info?mid=54200948
- 请求地址2：https://api.bilibili.com/x/web-interface/card?mid=54200948&photo=true
- 请求方式：GET
- 必须参数：mid（B站UID账号）



**bilibili 我的视频API参数：**

- 请求地址：https://api.bilibili.com/x/space/arc/search?mid=54200948&ps=30&tid=0&pn=1
- 请求方式：GET
- 必须参数：mid（B站UID账号）



**网易云音乐歌单API参数：**

- 请求地址：http://localhost:3000/v1/playlist/detail?id=477684994
- 请求方式：GET
- 必须参数：id（歌单ID）



**全国新型肺炎疫情最新数据接口API参数：**

- 请求地址：https://www.tianqiapi.com/api?version=epidemic&appid=24322911&appsecret=VGfMOF1p
- 请求方式：GET
- 必须参数：appid、appsecret



**QQ头像API参数：**

- 请求地址：[https://q1.qlogo.cn/g?b=qq&s=100&nk=1559867170](https://q1.qlogo.cn/g?b=qq&s=100&nk=1559867170)
- 请求方式：GET
- 必须参数：nk



**分享到QQ空间接口**：

- 请求地址：[https://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?url=你的网址&sharesource=qzone&title=你的分享标题&pics=你的分享图片&summary=你的分享描述信息](https://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?url=你的网址&sharesource=qzone&title=你的分享标题&pics=你的分享图片&summary=你的分享描述信息)
- 请求方式：GET
- 必须参数：url、title、pics、summary



**备案底部链接**

- 地址：http://www.beian.gov.cn/portal/registerSystemInfo?recordcode=11010802028541
- 参数：recordcode