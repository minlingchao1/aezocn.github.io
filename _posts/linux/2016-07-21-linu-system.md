---
layout: "post"
title: "linux系统命令"
date: "2016-07-21 19:19"
categories: linux
tags: [linux, shell]
---

* 目录
{:toc}

## 系统命令

1. 查看系统信息
  - 查看操作系统版本 `cat /proc/version`
  > 如腾讯云服务器 `Linux version 3.10.0-327.36.3.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-4) (GCC) ) #1 SMP Mon Oct 24 16:09:20 UTC 2016` 中的 `3.10.0` 表示内核版本 `x86_64` 表示是64位系统

  - 查看CentOS版本 `cat /etc/redhat-release` 如：CentOS Linux release 7.2.1511 (Core)
  - 查看内存 `grep MemTotal /proc/meminfo`

2. 查看启动的服务(不包括系统的)：`chkconfig --list`
3. `shutdown -r now` root登录可立刻重启
4. `df -hl` 查看磁盘使用情况
5. `netstat -lnp` 查看所有进场信息(端口、PID)
    - `ss -ant` CentOS 7 查看所有监听端口
    - `netstat -tnl` 查看开放的端口
    - `netstat -lnp | grep tomcat` 查看含有tomcat相关的进程
6. 安装程序包 `rpm -ivh 安装包名`
7. 查看安装程序(支持模糊查询) `rpm -qa | grep vsftpd` 查看是否安装vsftpd(一款ftp服务器软件)
8. 检查网络连接 `ping 192.168.1.1`(或者`ping www.baidu.com`)，检查端口：`telnet 192.168.1.1 8080`
9. 关闭某个PID进程 `kill PID`
    - `netstat -lnp` 查看所有进场信息(端口、PID)
    - 强制杀进程 `kill -s 9 PID`
10. 运行sh文件：进入到该文件目录，运行`./xxx.sh`
11. 后台运行sh文件：`nohup bash xxx.sh > my.log 2>&1 &`
    - 利用客户端连接服务器，执行的程序当客户端退出后，服务器的程序也停止了，下面是解决版本
    - `nohup`这个表示拖机执行，最后面的`&`表示放在后台执行
    - `2>&1` 表示记录错误和正确的日志，日志文件位置为 `my.log`

## 文件系统

### 文件

1. 新建文件 **`vi` FileName**
2. 删除文件 **`rm` File**
    - 提示 `rm: remove regular file 'test'?` 时，在后面输入 `yes` 回车
3. 复制文件 **`cp` xxx.txt /usr/local/xxx** (将xxx.txt移动到/usr/local/xxx)
    - 复制文件到远程服务器：`scp /home/test root@192.168.1.1:/home` 将本地linux系统的test文件或者文件夹复制到远程的home目录下
4. 重命名文件或目录、将文件由一个目录移入另一个目录中
    - **`mv` a.txt b.txt** (将a.txt重命名为b.txt)
5. 查看文件属性 **`file` FileName**
6. 查询文件 **`whereis` FileName**
    - 查询可执行文件位置 **`which` exeName** (在PATH路径中寻找)

### 文件夹/目录

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

4. 查看当前目录绝对路径 `pwd`
5. 查看目录结构 `ls`
  - 查看目录结构包括隐藏文件 `ls -a`

### 压缩包 [^1]

1. 解压
    - `unzip file.zip` 解压zip
    - `tar –xvf file.tar` 解压tar包
    - `tar -xzvf file.tar.gz` 解压tar.gz
    - `tar -xjvf file.tar.bz2` 解压tar.bz2
    - `tar –xZvf file.tar.Z` 解压tar.Z
    - `unrar e file.rar` 解压rar
2. 压缩
    - `tar –cvf jpg.tar *.jpg` 将目录里所有jpg文件打包成tar.jpg
    - `tar –czf jpg.tar.gz *.jpg` 将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
    - `tar –cjf jpg.tar.bz2 *.jpg` 将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
    - `tar –cZf jpg.tar.Z *.jpg` 将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z
    - `rar a jpg.rar *.jpg` rar格式的压缩，需要先下载rar for linux
    - `zip jpg.zip *.jpg` zip格式的压缩，需要先下载zip for linux


## 权限系统
1. 设置文件或目录的所有者 **`chown [-R]` 账号名称 文件或目录**
  - `-R` 递归设置子目录下所有文件和目录


## ssh [^2]

### ssh介绍

1. SSH是建立在传输层和应用层上面的一种安全的传输协议。SSH目前较为可靠，专为远程登录和其他网络提供的安全协议。在主机远程登录的过程中有两种认证方式：
    - `基于口令认证`：只要你知道自己帐号和口令，就可以登录到远程主机。所有传输的数据都会被加密，但是不能保证你正在连接的服务器就是你想连接的服务器。可能会有别的服务器在冒充真正的服务器，也就是受到“中间人”这种方式的攻击。
    - `基于秘钥认证`：需要依靠秘钥，也就是你必须为自己创建一对秘钥，并把公用的秘钥放到你要访问的服务器上，客户端软件就会向服务器发出请求，请求用你的秘钥进行安全验证。服务器收到请求之后，现在该服务器你的主目录下寻找你的公用秘钥，然后吧它和你发送过来的公用秘钥进行比较。弱两个秘钥一致服务器就用公用秘钥加密“质询”并把它发送给客户端软件，客户端软件收到质询之后，就可以用你的私人秘钥进行解密再把它发送给服务器。
2. 用基于秘钥认证，你必须要知道自己的秘钥口令。但是与第一种级别相比，这种不需要再网络上传输口令。第二种级别不仅加密所有传送的数据，而且“中间人”这种攻击方式也是不可能的（因为他没有你的私人密匙）。但是整个登录的过程可能需要10秒。

### 查看SSH服务

CentOS 7.1安装完之后默认已经启动了ssh服务我们可以通过以下命令来查看ssh服务是否启动

1. 查看开放的端口 `netstat -tnl` ssh默认端口为22
2. 查看服务是否启动 `systemctl status sshd.service` 查看ssh服务是否启动

### SSH客户端连接服务器（口令认证）

1. 直接连接到对方的主机，这样登录服务器的默认用户
    - `ssh 192.168.1.1` 回车输入密码即可
    - `exit` 退出登录
2. 使用账号登录对方主机aezocn用户
    - `ssh aezocn@192.168.1.1`

### SSH客户端连接服务器（秘钥认证）

1. 生成公钥和私钥
    - `ssh-keygen` 运行命令后再按三次回车会看到`RSA`（生成的秘钥默认路径为`/root/.ssh/`，会包括`id_rsa`、`id_rsa.pub`、`known_hosts` 3 个文件）
2. 把生成的公钥发送到对方的主机上去
    - `ssh-copy-id -i /root/.ssh/id_rsa.pub root@192.168.1.1` （自动保存在对方主机的`/root/.ssh/authorized_keys`文件中去）
    - 输入该服务器密码实现发送
3. 登录该服务器：`ssh 192.168.1.1` 此时不需要输入密码（默认生成密钥的服务器已经有了私钥）






---

参考文章

[^1]: [文件压缩与解压](http://www.jb51.net/LINUXjishu/43356.html)
[^2]: [ssh登录](http://www.linuxidc.com/Linux/2016-03/129204.htm)
