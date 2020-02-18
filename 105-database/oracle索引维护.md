### oracle索引维护

索引只能优化查询，但是会增加DML的开销。

索引会开辟一块空间保存。常用索引是Btree。

常用的场景：

1.大表，返回行数<5%

2.经常使用where查询的子列

3.离散度高的列

4.更新键值代价低

5.逻辑and or代价高

查看索引：

```sql
select * from user_indexes;
select * from user_ind_columns;
```

 索引碎片 

```sql
--制造索引碎片场景
create table temp(
    id int
    );

create index ind_1 on temp(id);

begin
    for i in 1..1000000 loop
        insert into temp values (i);
        if mod(i,1000)=0 
        then commit;
        end if;
    end loop;
end;
/


--查看索引分析数据
select name,height,pct_used,del_lf_rows/lf_rows from index_stats;
--分析索引
analyze index ind_1 validate structure;
--再次查看索引分析数据
SQL> select name,height,pct_used,del_lf_rows/lf_rows from index_stats;
 
NAME                               HEIGHT   PCT_USED DEL_LF_ROWS/LF_ROWS
------------------------------ ---------- ---------- -------------------
IND_1                                   3        100                   0

如果满足下面任意一个
HEIGHT>=4 
PCT_USED<50 
DEL_LF_ROWS/LF_ROWS>0.2 
进行索引碎片整理

--删除数据
SQL> delete from temp where rownum<=700000;
700000 rows deleted
--再次分析索引
SQL> analyze index ind_1 validate structure;
Index analyzed
--查看状态
SQL> select name,height,pct_used,del_lf_rows/lf_rows from index_stats;
 
NAME                               HEIGHT   PCT_USED DEL_LF_ROWS/LF_ROWS
------------------------------ ---------- ---------- -------------------
IND_1                                   3        100                 0.7

--满足DEL_LF_ROWS/LF_ROWS>0.2 

--在线修复索引
SQL> alter index ind_1 rebuild online;
Index altered
 
SQL> analyze index ind_1 validate structure;
Index analyzed
 
SQL> select name,height,pct_used,del_lf_rows/lf_rows from index_stats;
 
NAME                               HEIGHT   PCT_USED DEL_LF_ROWS/LF_ROWS
------------------------------ ---------- ---------- -------------------
IND_1                                   2         90                   0

```

### 特殊索引列的设计思路

将日期、区域、类型、等常用查询字段经过编码放到一个字段里

日期放到最后 分类放到前面

如日期20190101 地区：海门1100 通州1101 

id列组合为

110020190101

110120190101

 

110020190102

110120190102

取海门的数据

\>=110020190101 and <110020190102

 

 

 