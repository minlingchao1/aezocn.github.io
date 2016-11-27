---
layout: default
title: 你好，世界
---

<h2>{{ page.title }}</h2>
<p>我的第一篇文章</p>
<p>{{ page.date | date_to_string }}</p>


## github-jeykll-markdown个人书写习惯

### 元信息

categories和tage都可以有多个

```html
    categories: [cat1, cat2]
    tags: [tag1, tag2, tag3]
```

### md语法

- md文件中可以使用html标签
- `---`代表分割线

### 排版

正文的第一级标题用h2(`##`)，标题和字段间要有换行

### 列表

- 列表（有序/无序）下面显示 **代码、引用、图片** 时：相对列表的该子项代码需要多缩进一个Tab（4个空格），且中间要空行，如：

  <pre>
  - 标题

    ```html
      ...
    ```
  </pre>

- 子列表基于父列表要有一个Tab缩进（4个空格），中间无需空行

### 引用

应用支持链接跳转，注意引用标注与被引用文本之间有一个空格

<pre>
    标题或者文字 [^1]

    ---

    参考文章

    [^1]: [http://blog.aezo.cn](http://blog.aezo.cn)
</pre>














-----------------------------
