### oracle物化视图

```sql
create materialized view test_materialized_view﻿​
--条件部分
--是否在创建视图时生成数据
--IMMEDIATE，立刻生成，默认
--DEFERRED，延期生成
BUILD IMMEDIATE | DEFERRED 
--刷新方式
--快速刷新（FAST）采用增量刷新的机制，只将自上次刷新以后对基表进行的所有操作刷新到物化视图中去。
--FAST必须创建基于主表的视图日志。
--完全刷新（COMPLETE）会删除表中所有的记录（如果是单表刷新，可能会采用TRUNCATE的方式），
--然后根据物化视图中查询语句的定义重新生成物化视图。
--采用FORCE方式，Oracle会自动判断是否满足快速刷新的条件，如果满足则进行快速刷新，否则进行完全刷新。
--NEVER，不刷新，一般不使用
REFRESH FAST | COMPLETE | FORCE | NEVER
--刷新触发
--DEMAND，按需刷新，人为的刷新
--COMMIT，提交刷新，一旦有基表事务提交，立即刷新
ON DEMAND | COMMIT
--ON DEMAND附加条件
START WITH 第一次开始时间
NEXT 下一次刷新时间
--是否支持查询重写,默认disable
--查询重写就是，如果对基表的查询语句和创建物化视图的as下面的语句一样，是否直接从物化视图取数，不从基表取数
ENABLE | DISABLE QUERY REWRITE
as
select * from temp
```

```sql
--测试数据

create table test_class (
  clsid int primary key,
  name varchar2(10)
);

create table test_student(
  id int primary key,
  name varchar2(10),
  clsid int,
  constraint fk_cls_id foreign key (clsid) references test_class(clsid)
);

insert into test_class values(1,'一班');
insert into test_class values(2,'二班');

insert into test_student values(1,'张三',1);
insert into test_student values(2,'李四',1);
insert into test_student values(3,'王五',2);

commit;


--关联查询

select a.id,a.name,a.clsid,b.name clsname
from test_student a,test_class b
where a.clsid=b.clsid;

   	ID	NAME	CLSID	CLSNAME
1	1	张三	1   	一班
2	2	李四	1    	一班
3	3	王五	2   	二班


--物化视图的删除必须使用命令
drop materialized view test_student_view_fast;

--创建一般的物化视图
--FORCE这里缺少增量刷新的条件，就是全表刷新
create materialized view test_student_view
REFRESH FORCE
ON COMMIT
as
select a.id,a.name,a.clsid,b.name clsname
from test_student a,test_class b
where a.clsid=b.clsid;

--权限不足，授权
--登录system账户
grant create materialized view to ntkd;
--收回
revoke ...

再次执行创建物化视图，成功

select * from test_student_view t

   	ID	NAME	CLSID	CLSNAME
1	1	张三	1   	一班
2	2	李四	1	    一班
3	3	王五	2   	二班

--每日刷新的物化视图

create materialized view test_student_view_demand
refresh FORCE
ON DEMAND
-- 每天更新
START WITH sysdate
NEXT sysdate+1
as
select a.id,a.name,a.clsid,b.name clsname
from test_student a,test_class b
where a.clsid=b.clsid;

--按需刷新的快速物化视图

--快速物化视图必须在表建立log
create materialized view log on test_student with rowid;
create materialized view log on test_class with rowid;

多出两张表
mlog$_test_class
mlog$_test_student

--快速物化视图,手工刷新

drop materialized view test_student_view_fast;

create materialized view test_student_view_fast
refresh fast
ON DEMAND
WITH ROWID 
as
select 
	a.rowid as arowid,b.rowid as browid,
a.id,a.name,a.clsid,b.name clsname
from test_student a,test_class b
where a.clsid=b.clsid;

--修改基表数据后

--使用以下命令，在命令行执行手工刷新
--F fast C COMPLETE
EXEC DBMS_MVIEW.REFRESH('test_student_view_fast');
EXEC DBMS_MVIEW.REFRESH('test_student_view_fast','F'(选填));


--快速物化视图,自动定时刷新，会创建job，详见oracle job设置

create materialized view test_student_view_fast_auto
refresh fast
ON DEMAND
WITH ROWID 
START WITH sysdate+1/1440
NEXT sysdate+1/1440
as
select 
	a.rowid as arowid,b.rowid as browid,
    a.id,a.name,a.clsid,b.name clsname
from test_student a,test_class b
where a.clsid=b.clsid;
```

