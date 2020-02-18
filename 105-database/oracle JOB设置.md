### oracle JOB设置

```sql
--使用dba账户命令行先查看系统是否有可执行的进程
SQL> show  parameter  job_queue_process;
NAME                                 TYPE        VALUE
------------------------------------ ----------- ------------------------------
job_queue_processes                  integer     0
 
--job_queue_processes的参数设置问题：
--1. job_queue_processes取值范围为0到1000
--2. 当设定该值为0的时候则任意方式创建的job都不会运行。
--3. 当设定该值大于1时，且并行执行job时，至少一个为协调进程。其总数不会超出job_queue_processes的值。 

--重新设定
ALTER SYSTEM SET job_queue_processes = 10;

--查看job
select * from user_jobs t

select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') now,
        t.job,
        t.log_user,
        t.BROKEN,
        to_char(t.last_date,'yyyy-mm-dd hh24:mi:ss') last_date,
        to_char(t.next_date,'yyyy-mm-dd hh24:mi:ss') next_date,
        t.interval,
        t.what

from user_jobs t
where t.JOB=30;

NOW	        2019-02-28 11:33:48
JOB	        30
LOG_USER	NTKD
BROKEN	    N
LAST_DATE	2019-02-28 11:33:48
NEXT_DATE	2019-02-28 11:34:48
INTERVAL	sysdate+1/1440 
WHAT	    dbms_refresh.refresh('"NTKD"."TEST_STUDENT_VIEW_FAST_AUTO"');

 
```

