### apache derby 数据库使用

官网下载文件db-derby-10.14.2.0-bin，注意jdk版本对应关系 

 配置环境变量 

```ini
DERBY_HOME: E:\db-derby-10.14.2.0-bin
PATH: %DERBY_HOME%\bin
CLASSPATH: %DERBY_HOME%\lib \derby.jar;%DERBY_HOME%\lib\derbyclient.jar;%DERBY_HOME%\lib\derbytools.jar;%DERBY_HOME%\lib\derbynet.jar
```

 测试，在cmd下执行 

```
E:\db-derby-10.14.2.0-bin\bin>sysinfo.bat
```

输出一些信息

 连接工具ij 

```
E:\db-derby-10.14.2.0-bin\bin>ij.bat
ij 版本 10.14
ij>help   #查看帮助文档
```

在MySQL中，Schema和Database是等价的概念，Database之下直接就是数据库对象，既没有像Oracle那样按User区分，也没有像SQL Server那样按Schema区分。

PostgreSQL在一个database下可以有多个schema，schema为User名下所有数据库对象的总和，如果user名下没有东西，则不存在schema。

derby的schema应该也是User名下所有数据库对象的总和

 数据库操作

连接数据库
找到derby.log的那一层目录，进入ij

```bash
#db1即为数据文件夹的名字，不指定用户，默认使用APP schema。
ij> connect 'jdbc:derby:db1;';  

或者指定全路径
ij> connect 'jdbc:derby:E:\nacos\data\db1;';

#指定schema，创建的表都放在ntkd schema下。
ij> connect 'jdbc:derby:db1;user=ntkd';  

#如果不存在数据库，则创建数据库,如果存在数据库则，连接数据库
ij> connect 'jdbc:derby:db1;user=ntkd;create=true';

ij> create table test (string char(20));
ij> insert into test values('hello');
ij> show tables in ntkd;    #查看schema下的
ij> describe test;  #查看表
```

 设置数据库密码 

```bash
CALL SYSCS_UTIL.SYSCS_SET_DATABASE_PROPERTY('derby.authentication.provider','BUILTIN');

CALL SYSCS_UTIL.SYSCS_SET_DATABASE_PROPERTY('derby.connection.requireAuthentication','true');

CALL SYSCS_UTIL.SYSCS_SET_DATABASE_PROPERTY('derby.user.ntkd','ntkd');
#这里username和password改成你的就可以了，下次登录时则要输入这里设置的。

CALL SYSCS_UTIL.SYSCS_SET_DATABASE_PROPERTY('derby.database.fullAccessUsers','ntkd');
#这个username和前面那个一样

CALL SYSCS_UTIL.SYSCS_SET_DATABASE_PROPERTY('derby.database.defaultConnectionMode','noAccess');


ij> connect 'jdbc:derby:derbytest;user=ntkd;';
错误08004：发生连接验证失败。原因：验证无效。。
ij> connect 'jdbc:derby:derbytest;user=ntkd;password=ntkd';﻿​
ij> 
```

调用系统内置数据

```
values CURRENT_DATE;
#相当于oracle select xxx from dual;
```

Derby db 在10.5版本后有重要更新
含分页的排序语句如：

select * FROM (select ROW_NUMBER() OVER() AS R, customer_id, zip from CUSTOMER) AS T where R>0 and R<5 order by customer_id desc;

 