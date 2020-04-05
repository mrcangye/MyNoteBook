# MySQL学习历程(一)基本操作

## MySQL的安装

系统为ubuntu19.10，mysql 8.0.19

```bash
sudo apt-get update
sudo apt-get install -y mysql-server mysql-client
```

安装过程如果未提示输入用户名和密码，且版本号与上述的8.0.19一致。请参考[https://blog.csdn.net/CANGYE0504/article/details/105196963](https://blog.csdn.net/CANGYE0504/article/details/105196963)

### 相关命令

首先给出服务启动，停止，重启的命令：

```bash
sudo service mysql start
sudo service mysql stop
sudo service mysql restart
```

## MySQL基本操作

### 进入MySQL

```bash
mysql -u root -p
```

输入密码后，会以`root`用户的身份进入mysql

### 查看数据库

```sql
show databases;
```

这个命令会显示当前所有的数据库

```bash
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.02 sec)

```

### 连接数据库

如果想连接其中一个数据库，可以使用`use <数据库名>`。注意这个命令末尾没有分号`;`

```bash
use mysql
```

```bash
mysql> use information_schema
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
```

### 查看表

```sql
show tables;
```

可以查看数据库中有哪些表

```sql
mysql> show tables;
+---------------------------------------+
| Tables_in_information_schema          |
+---------------------------------------+
| ADMINISTRABLE_ROLE_AUTHORIZATIONS     |
| APPLICABLE_ROLES                      |
| CHARACTER_SETS                        |
| CHECK_CONSTRAINTS                     |
| COLLATIONS                            |
......
```

### 退出

想退出MySQL，可以使用`exit`

## 新建数据库

进入到`MySQL`中

```bash
mysql -uroot -p
```

输入密码，以`root`用户名进入

新建数据库的命令为：

```sql
CREATE DATABASE <数据库名>;
```

注意，`SQL`语句是不区分大小写的，虽然小写也可以。不过还是建议大写

在对这个新建的数据库进行操作前，要先告诉` MySQL`，你要使用这个数据库

```bash
use <数据库名>
```

例如，我们创建一个`mysql_test`数据库，并使用它

```sql
mysql> CREATE DATABASE mysql_test;
Query OK, 1 row affected (0.02 sec)

mysql> use mysql_test
Database changed
```

这样我们才以对新建的数据库进行操作

例如查看数据库里的表

```sql
mysql> show tables;
Empty set (0.00 sec)
```

新建的数据库，里面的表肯定是空的

