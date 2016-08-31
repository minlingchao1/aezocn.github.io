---
layout: default
title: 你好，世界
---

<h2>{{ page.title }}</h2>
<p>我的第一篇文章</p>
<p>{{ page.date | date_to_string }}</p>


## github-jeykll-markdown书写注意事项

- md文件中可以使用html标签
- 标题和字段间要有换行
- 无序列表下面显示代码时：相对列表的该子项代码需要多缩进一个Tab，且中间要空行，如：

  <pre>
  - 标题

    ```html
      ...
    ```
  </pre>

-
