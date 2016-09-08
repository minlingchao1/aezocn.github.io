---
layout: "post"
title: "projectRule - 项目开发规范"
date: "2016-07-07 09:38"
categories: extend
tags: [rule]
---

## 前台

### 文件命名
针对一个功能的前台页面(ftl/jsp)应该以名词开头，如果功能名称为"设置用户信息"，可起名为"userInfoSet"而不用"setUserInfo"(这一般像是方法名)

### 查询选项
1. input输入，主键外键的字段查询条件为等于，其他字段无特殊要求为左右模糊；下拉框为等于

### 查询结果


## 后台

### 方法
1. 参数验证
  - 方法内部需要对必须的参数进行验证，如方法有个形参username，内部拿到username的值去操作数据时，当username为空时报错，则此时在方法开始代码块中需对username进行非空判断
  - 验证一般包括：非空验证、类型验证(项目实践中一般会遇到Long和String的验证)等