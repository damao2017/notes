### oracle PIVOT 行转列

```sql
-- Create table
create table A_TEMP
(
 NAME VARCHAR2(30),
 IN_DATE VARCHAR2(10),
 IN_CLASS VARCHAR2(20)
)

-- insert data
insert into A_TEMP (NAME, IN_DATE, IN_CLASS)
values ('jack', '15', 'Day');
insert into A_TEMP (NAME, IN_DATE, IN_CLASS)
values ('helen', '15', 'Night');
insert into A_TEMP (NAME, IN_DATE, IN_CLASS)
values ('tom', '16', 'Day');
insert into A_TEMP (NAME, IN_DATE, IN_CLASS)
values ('jack', '16', 'Night');
insert into A_TEMP (NAME, IN_DATE, IN_CLASS)
values ('helen', '17', 'Day');
insert into A_TEMP (NAME, IN_DATE, IN_CLASS)
values ('tom', '17', 'Night');
commit;

-- get data
select * from(
select t.name,t.in_date,t.in_class from a_temp t
)
PIVOT ( 
max(in_class) 
for in_date 
in('15' day15 ,'16' day16 ,'17' day17)
)
```

