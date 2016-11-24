---
layout: "post"
title: "linux系统命令"
date: "2016-07-21 19:19"
categories: linux
tags: [linux, shell]
---

## 系统命令
1. 查看系统信息
  - 查看操作系统版本 `cat /proc/version`
  - 查看内核 `uname -a`
  > 如 Linux iZ23i7ib8jgZ 3.10.0-327.el7.x86_64 \#1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux 中的 x86_64 表示是64位系统

  - 查看内存 `grep MemTotal /proc/meminfo`

2. 查看启动的服务(不包括系统的)：`chkconfig --list`
3. `shutdown -r now` root登录可立刻重启
4. `df -hl` 查看磁盘使用情况
5. `netstat -anp` 常看所有端口
  - `netstat -anp | grep tomcat` 查看含有tomcat相关的进程
6. 安装程序包 `rpm -ivh 安装包名`
7. 查看安装程序(支持模糊查询)：`rpm -qa | grep vsftpd` 查看是否安装vsftpd(一款ftp服务器软件)
8. 检查网络连接：`ping 192.168.1.1`(或者`ping www.baidu.com`)，检查端口：`telnet 192.168.1.1 8080`

## 文件系统
1. 新建文件夹 **`mkdir` DirName** (或者用绝对路径 `mkdir /usr/local/DirName`)
2. 删除文件夹
  - 删除文件夹 **`rmdir` DirName** (如果文件夹不为空则无法删除)
  - 强制删除文件夹和其子文件夹 `rm -rf`
  > -r 就是向下递归，不管有多少级目录，一并删除
  > -f 就是直接强行删除，不作任何提示的意思

3. 切换目录
  - 进入目录 **`cd` DirName**
  - 返回上一级目录 `cd ..`
  - 返回某一级目录 **`cd` /usr/local/xxx**
  - 返回根目录 `cd ~` 或者 `cd $HOME`
4. 新建文件 **`vi` FileName**
4. 删除文件 **`rm` File**
5. 复制文件 **`cp` xxx.txt /usr/local/xxx** (将xxx.txt移动到/usr/local/xxx)
6. 重命名文件或目录、将文件由一个目录移入另一个目录中
  - **`mv` a.txt b.txt** (将a.txt重命名为b.txt)
7. 查看当前目录绝对路径 `pwd`
8. 查看目录结构 `ls`
  - 查看目录结构包括隐藏文件 `ls -a`
9. 查看文件属性 **`file` FileName**
10. 查询文件 **`whereis` FileName**
  - 查询可执行文件位置 **`which` exeName** (在PATH路径中寻找)


## 权限系统
1. 设置文件或目录的所有者 **`chown [-R]` 账号名称 文件或目录**
  - `-R` 递归设置子目录下所有文件和目录
