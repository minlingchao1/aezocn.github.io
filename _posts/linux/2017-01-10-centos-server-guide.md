---
layout: "post"
title: "CentOS服务器使用指导"
date: "2017-01-10 13:19"
categories: [extend]
tags: [CentOS, server]
---

* 目录
{:toc}

## 常用软件安装

### 安装jdk

#### 下载jdk文件

> 默认登录时候在 root 目录，直接下载和解压，软件包和解压目录都默认在 root 目录，可以切换到 hoom 目录进行下载

1. 下载rpm格式
  - 获取rpm链接（下载到本地后上传到服务器）： oracle -> Downloads -> Java SE -> Java Archive -> Java SE 7 -> Java SE Development Kit 7u80 -> Accept License Agreement -> jdk-7u80-linux-x64.rpm
  - 下载jdk，运行命令：`wget http://download.oracle.com/otn/java/jdk/7u80-b15/jdk-7u80-linux-x64.rpm`(这个链接会下载成html格式，不行)
  - `rmp -ivh jdk-7u80-linux-x64.rpm` 安装rpm文件
2. 下载tar格式（推荐）
  - 下载tar文件 `wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz`
  - 解压tar `tar -zxvf jdk-7u79-linux-x64.tar.gz`
  > 网上有很多深坑，如果报 gzip: stdin: not in gzip format 错请查看：http://www.cnblogs.com/gmq-sh/p/5380078.html

#### 配置环境变量
- `vi /etc/profile` 使用vi打开profile文件
- 在末尾输入并保存（注意JAVA_HOME需要按照实际路径）
  ```linux
  export JAVA_HOME=/root/java/jre/jdk1.7.0_79
  export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
  export PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
  ```
  > `vi 文件名`打开某个文件进行编辑
  > - 点击键盘`insert`，进入vi编辑模式，开始编辑；
  > - 点击`esc`退出编辑模式，进入到vi命令行模式；
  > - 输入`:x`/`ZZ`将刚刚修改的文件进行保存，退出编辑页面，回到初始命令行
  > - `Ctrl+z` 退出 vi 编辑器

- 运行命令 `. /etc/profile` 使profile立即生效(注意 . 和 / 之间有空格)
- `java -version` 打印版本号

### git安装

1. 查看是否安装git/查看git是否安装成功：`git --version`
    - `-bash: git: command not found` 表示尚未安装
2. 下载安装：`yum install git`
