网上答题的页面，考虑到要防止考生利用复制粘贴来提高作弊的可能性，就设计了不允许复制。 

方法也很简单，通过设置CSS 的 user-select就可以达到目的：

```css
-moz-user-select:none; /* Firefox私有属性 */
-webkit-user-select:none; /* WebKit内核私有属性 */
-ms-user-select:none; /* IE私有属性(IE10及以后) */
-khtml-user-select:none; /* KHTML内核私有属性 */
-o-user-select:none; /* Opera私有属性 */
user-select:none; /* CSS3属性 */
```

