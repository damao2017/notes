### oracle新建用户和权限管理

#### oracle管理账户

System：oracle数据库管理系统管理员

Sys: 超级用户

#### 用system用户创建

打开plsql，身份选择sysdba，使用System登录，命令

创建用户和密码

```
create user ntkd--用户名 identified by test111--密码
```

账户的锁定和解锁

```
alert user ntkd account lock;
alert user ntkd account unlock;
```

用户自己查看自己权限

```
select * from session_privs;
```

赋予权限

```
--赋予连接权限
grant connect to ntkd;
--赋予资源操作权限
grant resource to ntkd;

--延伸 with admin option , with grant option
--with admin option    A->B->C A回收B权限，C还有权限
--with grant option    A->B->C A回收B权限，C权限也被回收了

grant connect to ntkd with admin option;
```

 收回权限

```
revoke resource from ntkd;
```

收回默认的无限表空间权限并分配表空间，缺省表空间都在users里

```
##查看自己的表空间
select username,default_tablespace from user_users;

revoke unlimited tablespaces from ntkd;
alert user ntkd quota 500m on users;
```

将表赋给其他用户

```
grant select on ntkd.emp to xiaoming;
grant select,update(column_name) on ntkd.emp to xiaoming;
--修改权限能精确到字段
```

 删除账户

```
drop user ntkd;
```

 

 

 

 