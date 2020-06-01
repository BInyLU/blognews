## 预防常见 XSS 方法

> 原则：不相信客户输入的数据。
>  注意:  攻击代码不一定在`<script></script>`中。

1. 将重要的`cookie`标记为`http only`,   这样的话`Javascript` 中的`document.cookie`语句就不能获取到`cookie`了.
2. 只允许用户输入我们期望的数据。 例如：　年龄的textbox中，只允许用户输入数字。 而数字之外的字符都过滤掉。
3. 对数据进行`Html Encode` 处理
4. 过滤或移除特殊的Html标签， 例如: `<script>`, `<iframe>`  ` &lt` `for`  `&gt` `&quot` 
5. 过滤`JavaScript` 事件的标签。例如 `"onclick="`, `"onfocus"` 等等。

