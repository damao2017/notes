### mysql 安装和初始化配置

按顺序安装以下包

> sudo dpkg -i libaio1
> sudo dpkg -i libmecab2
> sudo dpkg -i mysql-common_5.7.17-1ubuntu16.04_amd64.deb
> sudo dpkg -i libmysqlclient20_5.7.17-1ubuntu16.04_amd64.deb 
> sudo dpkg -i libmysqlclient-dev_5.7.17-1ubuntu16.04_amd64.deb 
> sudo dpkg -i libmysqld-dev_5.7.17-1ubuntu16.04_amd64.deb 
> sudo dpkg -i mysql-community-client_5.7.17-1ubuntu16.04_amd64.deb 
> sudo dpkg -i mysql-client_5.7.17-1ubuntu16.04_amd64.deb 
> sudo dpkg -i mysql-community-source_5.7.17-1ubuntu16.04_amd64.deb 
> sudo dpkg -i mysql-community-server_5.7.17-1ubuntu16.04_amd64.deb 

登录测试

```
1mysql -u root -p
```

查看mysql进程

```
1ps -aux | grep mysql
```

查看端口

```
1netstat -ntlp
```

查看版本

```
1mysql -V
```

修改端口和访问ip

找到配置文件：/etc/mysql/mysql.conf.d/mysqld.cnf 

```
[mysqld]
#1.默认没有这个配置，需要自己添加
port		= 13306
#2.注释掉这句，要不然不允许远程访问
#bind-address	= 127.0.0.1
```

注意mysql在不同系统中，对于大小写敏感默认值是不一样的。

windows默认大小写不敏感，linux默认敏感

修改找到配置文件：/etc/mysql/mysql.conf.d/mysqld.cnf 

```
[mysqld]
#1.默认没有这个配置，需要自己添加
port = 13306
#2.注释掉这句，要不然不允许远程访问
#bind-address = 127.0.0.1﻿​

#数据库和表大小写不敏感,0大小写不敏感，1大小写敏感
lower-case-table-names=0
```

### 重启mysql

```
service mysql stop
service mysql start
```

### 增加用户并授权

```
#本机登录
mysql -u root -p

#选择系统数据库
use mysql;

#查看用户信息
select  user,host,authentication_string from user;

#增加远程访问的root用户
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '11';  

#刷新权限
flush privileges;

#增加用户
create user 'test'@'%' identified by 'test1';​
​﻿​
#刷新权限
flush privileges;

#增加数据库
create database testDB DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
create database openfire DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_general_ci;

#配置用户对数据库的权限
grant all privileges on `testDB`.* to 'test'@'%' ;

flush privileges;
```

### 修改数据库字符集

```
mysql> show variables like '%char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec) 
```

找到配置文件：/etc/mysql/mysql.conf.d/mysqld.cnf ，在[mysqld]中增加一行

```
character-set-server=utf8
```

重新查看字符集

```
mysql> show variables like '%char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)
```

 

========================================================

 