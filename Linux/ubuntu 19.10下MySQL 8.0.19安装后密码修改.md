# ubuntu 19.10下MySQL 8.0.19安装后密码修改

[toc]

## 本文背景

这个问题昨晚折腾了我一晚，原因是开始学习《Python3网络爬虫开发实战》这本书。在环境安装这一章一路畅通无阻，到了`mysql`之后就卡住了，因为安装过程没有提示输入密码。参考了CSDN各种的修改密码博客都没办法，方法都不对。有的跟我一样的版本号，给的语句在本机输入后显示格式错误。后面不得不查阅了官方文档。纪念一下第二次查官方。

## mysql的安装

系统为ubuntu19.10，mysql 8.0.19

```bash
sudo apt-get update
sudo apt-get install -y mysql-server mysql-client
```

安装过程未提示输入用户名和密码

## 相关命令

首先给出服务启动，停止，重启的命令，后面要用：

```bash
sudo service mysql start
sudo service mysql stop
sudo service mysql restart
```

## 查阅文档

[文档地址](https://dev.mysql.com/doc/refman/8.0/en/default-privileges.html)

### 第一步

>A password may already be assigned to the initial account under these circumstances:
>
>- On Windows, installations performed using MySQL Installer give you the option of assigning a password.
>- Installation using the macOS installer generates an initial random password, which the installer displays to the user in a dialog box.
>- Installation using RPM packages generates an initial random password, which is written to the server error log.
>- Installations using Debian packages give you the option of assigning a password.
>- For data directory initialization performed manually using [**mysqld --initialize**](https://dev.mysql.com/doc/refman/8.0/en/mysqld.html), [**mysqld**](https://dev.mysql.com/doc/refman/8.0/en/mysqld.html) generates an initial random password, marks it expired, and writes it to the server error log. See [Section 2.10.1, “Initializing the Data Directory”](https://dev.mysql.com/doc/refman/8.0/en/data-directory-initialization.html).

从上面我们可以看出，使用`apt`安装的`mysql`是有初始密码的。

> If you do not know the initial random password, look in the server error log.

接下来我们查阅系统分配的初始账号和密码

```bash
sudo gedit /etc/mysql/debian.cnf 
```

会显示：

```bash
# Automatically generated for Debian scripts. DO NOT TOUCH!
[client]
host     = localhost
user     = debian-sys-maint
password = JSHJAHJZBxsajxns
socket   = /var/run/mysqld/mysqld.sock
[mysql_upgrade]
host     = localhost
user     = debian-sys-maint
password = JSHJAHJZBxsajxns
socket   = /var/run/mysqld/mysqld.sock
```

`user`后面就是你的账户名`debian-sys-maint`

`password`后面就是你的密码`JSHJAHJZBxsajxns`

> Connect to the server as `root` using the password:
>
> ```terminal
> shell> mysql -u root -p
> Enter password: (enter the random root password here)
> ```

接下来我们用它登录

```bash
sudo -udebian-sys-maint -pJSHJAHJZBxsajxns
```

登录成功会显示：

```bash
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 12
Server version: 8.0.19-0ubuntu0.19.10.3 (Ubuntu)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

这样就登录进去了。

### 第二步

我们继续看文档

> Choose a new password to replace the random password:
>
> ```sql
> mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root-password';
> ```

按人家说的做在mysql中输入语句，把你自己的密码替换掉`root-password`

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root-password';
```

最后输入`exit;`退出即可

```sql
exit;
```

现在试试登录应该就可以了。

## 额外说明

如果登录出现下面报错：

```bash
ERROR 1698 (28000): Access denied for user 'mrcangye'@'localhost'
```

那么在登录时，语句前面加`sudo`就可以登录了

出现这个问题原因是`root`用户使用的是`auth_socket`，我们要把它改成`caching_sha2_password`

我们先进入`mysql`

```bash
sudo mysql -u root -p
```

接着输入你的密码

登录成功后输入以下以下语句：

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password AS 'root-password';
```

`root-password`用你的密码替换掉

最后`exit;`退出即可

```sql
exit;
```

再试试登录`mysql`应该就正常了。

