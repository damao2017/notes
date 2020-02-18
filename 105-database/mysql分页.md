### mysql分页

```sql
--从第6行开始，取10行。
SELECT * FROM table LIMIT 5,10;

--和java数据的转换
select * from t_user limit #{Start},#{Size}

//每页要显示的条数
 int pageSize = 10;
 //页码
 int pageNumber = 2;
 //转换为limit x,y
 Map<String, Object> map = new HashMap<String, Object>();
 map.put("Start",pageSize*(pageNumber-1));
 map.put("Size",pageSize);﻿​

```

