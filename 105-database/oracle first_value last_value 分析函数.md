### oracle first_value last_value 分析函数

```sql
select t.suspend_id,t.suspend_end_time,
last_value(t.suspend_end_time ) over(partition by t.bill_id order by t.suspend_end_time rows BETWEEN unbounded preceding AND unbounded following) lastval,
first_value(t.suspend_end_time ) over(partition by t.bill_id order by t.suspend_end_time rows BETWEEN unbounded preceding AND unbounded following) firstval
from js_ida.t_pub_bill_suspend_his t
 where t.suspend_status='BOOKING'
 and t.bill_id = '7209048373'
```

rows BETWEEN unbounded preceding AND unbounded following

显示声明窗口子句,让窗口的上界无界，下界也是无界这样，该组的数据将共享一个窗口数据。

默认的窗口子句：RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW。

即窗口的上界是无界，下界是当前行。