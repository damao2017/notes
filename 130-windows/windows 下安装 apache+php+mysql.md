# windows 下安装 apache+php+mysql

## 一、PHP下载

首先从官网上下载php5.6 http://windows.php.net/download

选VC15 x64 Thread Safe (2019-May-29 15:24:36)

## 二、 Apache服务器下载

首先从官网上下载Apache2.4 http://httpd.apache.org/download.cgi

Apache HTTP Server 2.4.39 (httpd): 2.4.39 is the latest available version 2019-04-01

选Files for Microsoft Windows

Downloading Apache for Windows

选Apache Lounge

先确保运行环境下载

Be sure !! that you have installed the latest (14.21.27702.2) Visual C++ Redistributable for Visual Studio 2015-2019 : vc_redist_x64 or vc_redist_x86.

然后下载

Apache 2.4.39 Win64

[Apache VC15 Binary] httpd-2.4.39-win64-VC15.zip               28 May '19 17.326k

## 三、MySQL 5.6下载

我使用的是mysql5.7.26这个版本 官网下载地址：https://dev.mysql.com/downloads/mysql/5.7.html#downloads

## 四、Apache安装配置

1.先装vc_redist_x64

2.解压到根目录，打开apache解压目录conf文件夹下的httpd.conf文件

Define SRVROOT "E:/Apache24" 修改指向文件目录

Listen 9500 修改指定的端口

DocumentRoot "${SRVROOT}/htdocs" 可以指定web根目录

3.打开cmd命令行进bin目录，输入httpd -k install，输出如下：

E:\Apache24\bin>httpd -k install

Installing the 'Apache2.4' service

The 'Apache2.4' service is successfully installed.

Testing httpd.conf....

Errors reported here must be corrected before the service can be started.

4.打开apache bin目录下的ApacheMonitor.exe，start，输入localhost:9500即可

## 五、配置php模块到apache服务器

打开apache的httpd.conf文件，找到#LoadModule后添加

\#php

PHPIniDir E:/php-7.3.6

LoadModule php7_module E:/php-7.3.6/php7apache2_4.dll

<IfModule mime_module>后添加

\#php

AddType application/x-httpd-php .php

创建测试页面phpinfo.php

<?php

phpinfo();

?>

启动apache，访问网页

修改默认访问的页面

DirectoryIndex index.html index.php index.htm

禁止列目录

Options Indexes FollowSymLinks改为Options -Indexes +FollowSymLinks

## 六、Mysql安装

1.解压到E:\mysql-5.7.26-winx64

2.添加环境变量

MYSQL_HOME

E:\mysql-5.7.26-winx64

在Path中添加%MYSQL_HOME%\bin;（注意结尾处有分号）

3.创建data文件和创建my.ini

创建data：mysqld --initialize-insecure --user=mysql

创建my.ini：在E:\mysql-5.7.26-winx64创建my.ini

[client]

port=3306

default-character-set=utf8

[mysqld] 

\# 设置为自己MYSQL的安装目录 

basedir=D:\mysql-5.7.20-winx64

\# 设置为MYSQL的数据目录 

datadir=D:\mysql-5.7.20-winx64\data

port=3306

character_set_server=utf8

sql_mode=NO_ENGINE_SUBSTITUTION,NO_AUTO_CREATE_USER

\#开启查询缓存

explicit_defaults_for_timestamp=true

skip-grant-tables

4.安装windows服务

mysqld -install 

Service successfully installed

5.启动服务

net start mysql

net stop mysql

6.配置root账户

net stop mysql

mysqld --skip-grant-tables

打开一个新的“命令提示符”，执行mysql -u root登陆 MySQL Server。

执行flush privileges;刷新权限。

执行grant all privileges on *.* to 'root'@'localhost' identified by '你想设置的密码' with grant option;。

执行flush privileges;刷新新的 root 用户密码。

执行exit退出 MySQL。

在任务管理器下手动结束mysqld.exe

net start mysql重新开启MySQL Server

使用mysql -u root -p 即可安全登陆 MySQL

创建独立的用户和数据库并赋权

CREATE DATABASE IF NOT EXISTS discuz DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_general_ci;

create user 'discuz'@'localhost' identified by 'wersdf234';

flush privileges;

创建一个数据库discuz

grant all privileges on discuz.* to 'discuz'@'localhost';

收回用户权限

REVOKE ALL PRIVILEGES FROM 用户名；

## 七、为php添加mysql支持

; extension_dir = "ext"，去掉前面的“;”，并改为extension_dir ="E:\php-7.3.6\ext"打开php的扩展支持

打开php的mysql扩展

; extension=mysqli

去服务里面重启Apache24

访问phpinfo.php即可看到

mysqli

MysqlI Support enabled

Client API library version mysqlnd 5.0.12-dev - 20150407 - $Id: 7cc7cc96e675f6d72e5cf0f267f48e167c2abb23 $

Active Persistent Links 0

Inactive Persistent Links 0

Active Links 0


 