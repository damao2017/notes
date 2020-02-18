### oracle取消账号密码限定180天必须修改

oracle使用profile来管理这个策略

#### 一、修改默认权限的时间限制

##### 1.查看用户的profile
```sql
select username, profile from dba_users;
```

##### 2.查看profile配置
```sql
SELECT * FROM dba_profiles WHERE profile='DEFAULT' AND resource_name='PASSWORD_LIFE_TIME'
```

##### 3.修改
```sql
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED ;
```


#### 二、创建新的profile并赋值给用户

