# mysql主从同步

mysql连接
mysql -h192.168.116.128 -p -uroot -P13306


主机操作

server-id = 1 指定id，主从不同
log-bin = mysql-bin 写了这句，下面的主show master status才会有东西。
二进制日志，也成为二进制日志，记录对数据发生或潜在发生更改的SQL语句，并以二进制的形式保存在磁盘中；
\#lower_case_table_names=1 这句不能要

查看当前连接
show processlist;

查看主机状态 记录File，Position 带会在从上面要配置
SHOW MASTER STATUS;

查看从复制账号的权限
SHOW GRANTS FOR sync;

给从复制账号开权限
GRANT REPLICATION SLAVE ON *.* TO 'sync'@'%' IDENTIFIED BY 'sync5805681';

刷新权限
FLUSH PRIVILEGES;

从机操作

主从的配置文件
server-id = 12 指定id，主从不同

查看slave状态
SHOW SLAVE STATUS;

先停止slave
STOP SLAVE;

增加主机配置
CHANGE MASTER TO MASTER_HOST='192.168.81.10',MASTER_USER='sync',MASTER_PASSWORD='sync5805681',MASTER_LOG_FILE='mysql-bin.000003',MASTER_LOG_POS=657,MASTER_PORT=13306;

开启slave
START SLAVE;

character_set_client、character_set_connection、character_set_results这3个参数值是由客户端每次连接进来设置的，和服务器端没关系。

从实际上可以看到，当客户端连接服务器的时候，它会将自己想要的字符集名称发给mysql服务器，然后服务器就会使用这个字符集去设置character_set_client、character_set_connection、character_set_results这三个值。如cmd是用gbk，而SQLyog是用utf8.

如果我们想告诉mysql server自己本次连接想使用latin1，则命令行下可以如下写法：

mysql -uroot -h 192.168.2.11 -pAbcd@1234 --default-character-set=latin1

此外，要修改上面的3个字符集的话，

还可以在my.cnf的[mysql]段里面增加：

default-character-set=latin1

也可以登录进去后，执行set names latin1的效果相同。


character_set_database

这个是当前所在的数据库字符集。如果没有切换到其他数据库，则character_set_database显示的和character_set_server一致。

例：切换到一个默认是gbk的数据库里，执行showvariables like 'character_set_database';看到的就是gbk

 

查看mysql数据文件路径：show variables like '%dir%'; datadir
查看mysql字符集：show variables like 'character%';