# Q&A

### 页面导入样式时，使用 link 和 @import 有什么区别？

1. 从属关系：link 是 HTML 标签，@import 是 css 提供的
2. 加载顺序：页面加载时，link 会同时加载，@import 引用需要等页面加载完后再加载
3. 兼容性：@import 只有 IE5 以上才能被识别，link 无兼容性问题
4. DOM 操作：link 可以通过 js 操作 DOM 动态引入样式，@import 不行
