# oracle REGEXP_REPLACE 数字处理

```sql
--去掉字符串中的数字
SELECT(REGEXP_REPLACE('LSS12345', '[0-9]+','')) FROM DUAL


--留下字符串中的数字
SELECT(REGEXP_REPLACE('LSS12345', '[^0-9]','')) FROM DUAL
```

