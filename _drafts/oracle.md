---
layout: "post"
title: "oracle"
date: "2016-10-12 21:06"
categories: db
tags: [oracle, dba]
---

## oracle相关名词和原理

1. 数据库名(DB_NAME)、实例名(INSTANCE_NAME)、以及操作系统环境变量(ORACLE_SID) [^1]
    - `DB_NAME`: 在每一个运行的oracle数据库中都有一个数据库名(如: orcl)，如果一个服务器程序中创建了两个数据库，则有两个数据库名。
    - `INSTANCE_NAME`: 数据库实例名则用于和操作系统之间的联系，用于对外部连接时使用。在操作系统中要取得与数据库之间的交互，必须使用数据库实例名(如: orcl)。与数据库名不同，在数据安装或创建数据库之后，实例名可以被修改。例如，要和某一个数据库server连接，就必须知道其数据库实例名，只知道数据库名是没有用的。用户和实例相连接。
    - `ORACLE_SID`: 在实际中，对于数据库实例名的描述有时使用实例名(instance_name)参数，有时使用ORACLE_SID参数。这两个都是数据库实例名。instance_name参数是ORACLE数据库的参数，此参数可以在参数文件中查询到，而ORACLE_SID参数则是操作系统环境变量，用于和操作系统交互，也就是说在操作系统中要想得到实例名就必须使用ORACLE_SID。此参数与ORACLE_BASE、`ORACLE_HOME`等用法相同。在数据库安装之后，ORACLE_SID被用于定义数据库参数文件的名称。如：$ORACLE_BASE/admin/DB_NAME/pfile/init$ORACLE_SID.ora。
2. `SERVICE_NAME`：是网络服务名(如：local_orcl)，可以随意设置。相当于某个数据库实例的别名方便记忆和访问。`tnsnames.ora`文件中设置的名称（如：`local_orcl=(...)`），也是登录pl/sql是填写的Database。

## oracle及pl/sql安装和使用

- oracle_home为`D:/java/oracle/product/11.2.0/dbhome_1`，`%oracle_home%/bin`中为一些可执行程序（如：导入imp.exe、导出exp.exe）

### Net Manager的使用

1. `本地-监听程序-LISTENER`中的主机要为计算机全名(如：ST-008)
2. `本地-服务命名`下的都为`网络服务名`

## 创建表空间 [^2]

oracle和mysql不同，此处的创建表空间相当于mysql的创建数据库

1. 创建表空间：`create tablespace aezocn datafile 'd:/tablespace/aezo' size 800m extent management local segment space management auto;` ，要先建好路径 d:/tablespace ，最终会在该目录下建一个 aezo 的文件
    - 删除表空间：`drop tablespace aezocn including contents and datafiles`
2. 创建用户：`create user aezo identified by aezo default tablespace aezocn;`
3. 授权
    - `grant create session to aezo;`
    - `grant unlimited tablespace to aezo;`
    - `grant dba to aezo;`

## 导入导出

`.dmp`适合导数据导出，`.sql`适合小数据导出(表中含有CLOB类型字段则不能导出)

### 命令行 [^4]

输入 `imp/exp 用户名/密码` 可根据提示导入导出。直接cmd运行

1. 导出
    - **用户模式**：`exp aezo/aezo file=d:/exp.dmp owner=scott`
        - 导出 scott 的所有对象
        - 加上 `compress=y` 表示压缩数据
        - 加上 `rows=n` 表示不导出数据行，只导出结构
    - 表模式：`exp scott/tiger file=d:/exp.dmp tables=emp` 导出scott的emp表
        - 导出其他用户的表：`exp system/manager file=d:/exp.dmp tables=scott.emp, scott.dept` 导出scott的emp、dept表，用户system需要相关权限
        - 导出部分表数据：`exp scott/tiger file=d:/exp.dmp tables=emp query=\" where ename like '%AR%'\"`
        - 常见错误(EXP-00011)：原因为11g默认创建一个表时不分配segment，只有在插入数据时才会产生。 [^3]
    - 导出全部：`exp system/manager file=d:/exp.dmp full=y`
        - 用户 system/manager 必须具有相关权限
        - 导出的是整个数据库，包括所有的表空间

2. 导入
    - 用户模式：`imp system/manager file=d:/exp.dmp fromuser=scott touser=aezo ignore=y`
        - `ignore=y`忽略创建错误
        - 不少情况下要先将表彻底删除，然后导入
    - 表模式：`imp system/manager file=d:/exp.dmp fromuser=scott tables=emp, dept touser=aezo ignore=y`
        - 将scott的表emp、dept导入到用户aezo
        - 此处 file/fromuser/touser 都可以指定多个
    - 导入全部：`imp system/manager file=d:/exp.dmp full=y ignore=y`
        - 用户 system/manager 必须具有相关权限
        - 导入的是整个数据库，包括所有的表空间

### pl/sql

- pl/sql提供dmp、sql、pde(pl/sql提供)格式的数据导入导出
- 方法：`Tools - Export Tables/Import Tablse - 选择表导出`
- 其中Executable路径为 `%ORACLE_HOME%/BIN/exp.exe` 和 `%ORACLE_HOME%/BIN/imp.exe` 如：`D:/java/oracle/product/11.2.0/dbhome_1/BIN/exp.exe`

## 常用命令

1. 系统
    - 查看服务是否启动：`tnsping local_orcl` cmd直接运行
        - 远程查看：`tnsping 192.168.1.1:1521/orcl`
        - 如果能够ping通，则说明客户端能解析listener的机器名，而且lister也已经启动，但是并不能说明数据库已经打开，而且tsnping的过程与真正客户端连接的过程也不一致。但是如果不能用tnsping通，则肯定连接不到数据库
1. 管理员
    - sysdba管理员登录：`sqlplus / as sysdba;`，以sys登录。sys为系统管理员，拥有最高权限；system为本地管理员，次高权限
2. 用户相关
    - 创建用户：` create user aezo identified by aezo;`
        - 默认使用的表空间是`USERS`，使用`create user aezo identified by aezo default tablespace aezocn;`可设定默认表空间
        - 删除用户：`drop user aezo cascade;`
    - 修改用户密码：`alter user scott identified by tiger;`
    - 解锁用户：`alter user scott account unlock;` (新建数据库scott默认未解锁)
3. 授权
    - `grant create session to aezo;` 授予aezo用户创建session的权限，即登陆权限
    - `grant unlimited tablespace to aezo;` 授予aezo用户使用表空间的权限
    - `grant dba to aezo;` 授予管理权限(有dba角色就有建表等权限)
4. 其他
    - 查看所有用户：`select name, password from user$;`















[^1]: http://www.cnblogs.com/advocate/archive/2010/08/20/1804063.html
[^2]: http://blog.csdn.net/starnight_cbj/article/details/6792364
[^3]: http://www.cnblogs.com/yzy-lengzhu/archive/2013/03/11/2953500.html
[^4]: http://blog.csdn.net/studyvcmfc/article/details/5679235

-----------------------
