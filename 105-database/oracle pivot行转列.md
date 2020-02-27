# oracle pivot行转列

```sql
select t.query_date, t.rep_type, t.c1
   from zwrb_data t
where t.region = '合计'
   and t.rep_type in ('装移机日报', '障碍日报')
   and t.query_date = date '2016-04-14'
   
--使用povit行转列
select *
  from (
        select t.query_date, t.rep_type, t.c1
          from zwrb_data t
         where t.region = '合计'
           and t.rep_type in ('装移机日报', '障碍日报')
           and t.query_date = date '2016-04-14'
        ) 
  pivot(
  max(c1) 
  for rep_type in('装移机日报' as zyjrb,'障碍日报' as zarb))   
   
```

