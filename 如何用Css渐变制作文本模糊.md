实现的思路：

文字收缩的时候，给它下面添加一个渐变的遮罩层，当展开的时候，把那个遮罩层给去掉。
[在线预览地址](http://uip16584232.cms1.91mb.com.cn/content/templates/mini/demo5.html)


主要代码：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>文本模糊</title>
<style type="text/css">
body {
line-height: 1.7;
}

.question-description {
position: relative;
max-height: 5.1em;
margin-top: 5px;
overflow: hidden;
width: 370px
}

.question-description.is-expanded {
max-height: none
}

.question-mask {
position: absolute;
top: 0;
width: 100%;
height: 100%;
background-image: -webkit-linear-gradient(top, hsla(0, 0%, 100%, .5), #fff);
background-image: linear-gradient(180deg, hsla(0, 0%, 100%, .5), #fff)
}

.expandBtn {
position: absolute;
right: 0;
bottom: 0;
color: #999;
background-color: #fff;
cursor: pointer;
line-height: 1.5;
border-radius: 4px;
border: 0 none;
font-size: 16px
}
</style>
</head>
<body>
<div class="question-description">
<div class="rich-text">
银行IPO迎来好年景：
9家银行IPO进程已处于“预披露更新”
对于拟上市银行而言，2018年或许是IPO的“好年景”。
据《证券日报》记者统计，共有17家银行排队推进A股IPO进程，合计募集资金规模上限接近900亿元，如果按照下限标准计算，合计的募资规模将降至约400亿元。
此外，在拟上市银行中，郑州银行、威海市商业银行、亳州药都农商行、长沙银行、浙商银行的市盈率低于行业均值，也最有希望按照预期上限规模发行。
17家银行排队IPO
9家达标“预披露更新”
《证券日报》记者注意到，证监会5月31日披露的《发行监管部首次公开发行股票正常审核状态企业基本信息情况表》显示，包括已经通过发审会的郑州银行和长沙银行在内，共有17家银行排队推进A股IPO进程，其中2家银行排队状态为“已通过发审会”、9家状态为“预先披露更新”，5家为“已反馈”、1家为“已受理”。
如今在监管部门备案且依旧排队等待A股上市的银行中，除了2家已过会银行，在上交所排名最为靠前的是威海市商业银行，在深交所排名居前的是兰州银行。
此外，今年曾有两家H股银行暂时放弃“H+A”大计，撤回A股发行申请。2月12日晚间，徽商银行发布公告称，鉴于公司仍需就相关法律法规，及证监会要求所涉及的部分事项与个别董事和股东进一步协商，并经董事会审议通过，决定撤回A股发行申请；3月16日，哈尔滨银行于香港交易所发布公告称，经与保荐人审慎研究，并经该行董事会审议批淮，决定撤回A股上市申请，待内资股股权结构变动完成后再重启A股上市申请。此外，两家银行均表示，银行业务运作良好，撤回A股上市申请将不会对财务状况或营运造成任何重大不利影响。
合计募资
下限约400亿元
《证券日报》记者对于目前排队的17家拟上市银行的招股说明书，和最新披露的财报数据进行了比对和测算：按照最高发行规模或发行计划测算，合计募集资金规模上限接近900亿元（根据2016年和2017年银行股IPO情况，发行价大多与当期每股净资产对应）。
不过，2016年银行股IPO虽然是时隔六年后重新开闸，但其“二级市场杀伤力”显然远低于市场最初千亿元的预期：当时9家获批银行合计的募集资金净额不足300亿元。
现实与预期差距的主要原因是拟上市银行的发行规模被压缩。
2016年以来，银行获批发行的股份数量大多占发行后总股份的比例仅为10%，也就是说，是按照《证券法》规定的下限“贴地发行”。如果按照下限标准计算，目前排队银行的发行规模较上限数据则会有所变化，合计的募资规模将降至约400亿元。
从监管态度来看，决定拟上市银行发行规模和发行价格的最核心要素是市盈率指标。贵阳银行彼时就是凭借发行价格对应的市盈率低于行业平均水平，避免了推迟发行，也就是在2016年银行股IPO中，唯一一家发行规模与预期维持不变的上市银行。
如果按照发行价格不低于每股净资产测算，目前排队银行的市盈率在5倍至15倍之间；其中，郑州银行、长沙银行、亳州药都农商行的市盈率低于6倍；威海市商业银行的市盈率低于7倍；浙商银行略高于7倍。
从目前已上市银行的静态市盈率来看，行业均值大约在7.5倍左右。也就是说，郑州银行、威海市商业银行、亳州药都农商行、长沙银行、浙商银行的市盈率低于行业均值，也最有希望按照预期上限规模发行。另有多家银行的动态市盈率略低于行业均值或与行业均值接近，并有望保持预期的发行规模。
</div>
<div class="question-mask">
<button class="expandBtn">查看问题描述</button>
</div>
</div>
<script type="text/javascript">
var oBtn = document.getElementsByClassName('expandBtn');
var oContent = document.getElementsByClassName('question-description');
var oMask = document.getElementsByClassName('question-mask');
oBtn[0].onclick = function() {
oContent[0].className = 'question-description is-expanded';
oMask[0].parentNode.removeChild(oMask[0]); //去掉遮罩层
}
</script>
</body>
</html>
```

