---
layout: default
title: 你好，世界
---

<h2>{{ page.title }}</h2>
<p>我的第一篇文章</p>
<p>{{ page.date | date_to_string }}</p>

=========markdown=========<br />

- md文件中可以使用html标签
- 标题和字段间要有换行
- 代码显示在无序列表下面：代码缩进和无需列表对其，且中间要空行

  <pre>
  - 标题

    ```html
      ...
    ```
  </pre>

- 
