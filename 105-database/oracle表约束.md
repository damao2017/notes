### oracle表约束

```sql
--简单的表创建和字段类型
--简单的创建语句：
create table student(
      sno number(10) ,--primary key
      sname varchar2(100) ,--not null
      sage number(3), --check(sage<150 and sage>0)
      ssex char(4) ,--check(ssex='男' or ssex='女')
      sfav varchar2(500),
      sbirth date,
      sqq varchar2(30) --unique
      --constraints pk_student_sno primary key(sno)--添加主键约束
      --constraints ck_student_sname check(sname is not null)--非空约束
      --constraints ck_student_sage check(sage<150 and sage>0)--检查约束
      --constraints ck_student_ssex check(ssex='男' or ssex='女')--检查约束
      --constraints un_student_sqq unique(sqq)--唯一约束
      )   
      
--添加主键约束
alter table student add  constraints pk_student_sno primary key(sno); 
alter table student drop  constraints pk_student_sno;
     
--添加非空约束
alter table student add  constraints ck_student_sname check(sname is not null);
alter table student drop  constraints ck_student_sname; 
     
--添加检查约束
alter table student add constraints ck_student_sage check(sage<150 and sage>0)
alter table student drop  constraints ck_student_sage; 
     
--添加检查约束校验性别
alter table student add constraints ck_student_ssex check(ssex='男' or ssex='女')
alter table student drop  constraints ck_student_ssex; 
      
--添加唯一约束
alter table student add constraints un_student_sqq unique(sqq)
```

