### oracle merge into,start with,over()

### **merge into合并** 

对数据先进行查询，如果存在就更新，如果不存在就插入。

```sql
merge into table a
use (select 10 id,'jack' name from dual) temp
on (a.id=temp.id)
when matched then 
    update set a.name=temp.name
when not matched then 
    insert (a.id,a.name) values(temp.id,temp.name)
```

### **start with connect by prior 递归**

```sql
select * from test
where ....
start with id=9980      --开始节点，可以写更多条件
connect by prior pid=id --向上递归查询
connect by prior id=pid --向下递归查询
order by id desc 
```

### **over(partition by ... order by ... ) 分析函数**

```sql
select deptno,ename,sal,
sum(sal) over () 总和,---相当于sum(sal),但是sum()需要结合group使用
sum(sal) over (order by ename) ename连续总和, --按ename分组连续求和
--sum(sal) over (order by deptno) deptno连续总和, --按deptno分组连续求和,这两个只能同时使用一个，谁在后面谁正常显示
sum(sal) over (partition by deptno) deptno总和,
row_number() over (order by sal) 排名,

from emp
```

 

 

```sql
--sum(sal) over (order by ename) ename连续总和
--按ename排序，相同的emane显示一样的值，不同的ename连续求和

   	DEPTNO	ENAME	SAL	    总和	ENAME连续总和	TEST
1	20  	ADAMS	1100.00	29025	1100	        测试数据
2	30	    ALLEN	1600.00	29025	2700	        测试数据
3	30  	BLAKE	2850.00	29025	5550        	测试数据
4	10	    CLARK	2450.00	29025	8000        	测试数据
5	20	    FORD	3000.00	29025	11000       	测试数据
6	30	    JAMES	950.00	29025	11950       	测试数据
7	20	    JONES	2975.00	29025	14925       	测试数据
8	10  	KING	5000.00	29025	19925	        测试数据
9	30  	MARTIN	1250.00	29025	21175       	测试数据
10	10  	MILLER	1300.00	29025	22475       	测试数据
11	20  	SCOTT	3000.00	29025	25475	        测试数据
12	20  	SMITH	800.00	29025	26275	        测试数据
13	30  	TURNER	1500.00	29025	27775	        测试数据
14	30  	WARD	1250.00	29025	29025	        测试数据


--sum(sal) over (order by deptno) deptno连续总和
--按deptno排序，相同的deptno显示一样的值，不同的deptno连续求和

   	DEPTNO	ENAME	SAL	    总和	DEPTNO连续总和	TEST
1	10	    CLARK	2450.00	29025	8750	        测试数据
2	10	    KING	5000.00	29025	8750        	测试数据
3	10	    MILLER	1300.00	29025	8750        	测试数据
4	20  	JONES	2975.00	29025	19625       	测试数据
5	20	    FORD	3000.00	29025	19625       	测试数据
6	20	    ADAMS	1100.00	29025	19625       	测试数据
7	20  	SMITH	800.00	29025	19625       	测试数据
8	20	    SCOTT	3000.00	29025	19625       	测试数据
9	30	    WARD	1250.00	29025	29025       	测试数据
10	30	    TURNER	1500.00	29025	29025       	测试数据
11	30	    ALLEN	1600.00	29025	29025       	测试数据
12	30  	JAMES	950.00	29025	29025       	测试数据
13	30	    BLAKE	2850.00	29025	29025	        测试数据
14	30  	MARTIN	1250.00	29025	29025	        测试数据

--sum(sal) over (order by deptno,ename) deptnoename连续总和,
--按deptno,ename分组连续求和
--先按deptno排序，再按ename排序，然后连续求和

   	DEPTNO	ENAME	SAL	    总和	DEPTNOENAME连续总和	TEST
1	10	    CLARK	2450.00	29025	2450            	测试数据
2	10	    KING	5000.00	29025	7450	            测试数据
3	10	    MILLER	1300.00	29025	8750	            测试数据
4	20	    ADAMS	1100.00	29025	9850	            测试数据
5	20	    FORD	3000.00	29025	12850           	测试数据
6	20	    JONES	2975.00	29025	15825	            测试数据
7	20	    SCOTT	3000.00	29025	18825	            测试数据
8	20	    SMITH	800.00	29025	19625	            测试数据
9	30  	ALLEN	1600.00	29025	21225	            测试数据
10	30  	BLAKE	2850.00	29025	24075	            测试数据
11	30  	JAMES	950.00	29025	25025           	测试数据
12	30  	MARTIN	1250.00	29025	26275           	测试数据
13	30  	TURNER	1500.00	29025	27775           	测试数据
14	30  	WARD	1250.00	29025	29025	            测试数据

```

 

```sql
--sum(sal) over () 总和,
--sum(sal) over (partition by deptno) deptno总和,
--sum(sal) over (partition by deptno order by ename) deptno内ename连续求和,
--总和和分组总和，部门分组再按ename连续求和

   	DEPTNO	ENAME	SAL	    总和	DEPTNO总和	DEPTNO连续求和	TEST
1	10  	CLARK	2450.00	29025	8750	    2450        	测试数据
2	10  	KING	5000.00	29025	8750    	7450        	测试数据
3	10  	MILLER	1300.00	29025	8750	    8750        	测试数据
4	20  	ADAMS	1100.00	29025	10875	    1100        	测试数据
5	20  	FORD	3000.00	29025	10875	    4100	        测试数据
6	20  	JONES	2975.00	29025	10875   	7075        	测试数据
7	20  	SCOTT	3000.00	29025	10875   	10075       	测试数据
8	20  	SMITH	800.00	29025	10875   	10875       	测试数据
9	30  	ALLEN	1600.00	29025	9400    	1600        	测试数据
10	30  	BLAKE	2850.00	29025	9400    	4450        	测试数据
11	30  	JAMES	950.00	29025	9400    	5400        	测试数据
12	30  	MARTIN	1250.00	29025	9400    	6650        	测试数据
13	30  	TURNER	1500.00	29025	9400    	8150        	测试数据
14	30  	WARD	1250.00	29025	9400    	9400        	测试数据
```

 

```sql
--row_number() over (order by sal) 排名,
--整体排名
--row_number() --如果有相同的，按12345这样排名
--rank()       --如果有相同的，按11345这样排名
--dese_rank()  --如果有相同的，按11234这样排名

   	DEPTNO	ENAME	SAL	    总和	排名	TEST
1	20	    SMITH	800.00	29025	1	    测试数据
2	30	    JAMES	950.00	29025	2	    测试数据
3	20	    ADAMS	1100.00	29025	3	    测试数据
4	30	    WARD	1250.00	29025	4	    测试数据
5	30	    MARTIN	1250.00	29025	5	    测试数据
6	10	    MILLER	1300.00	29025	6   	测试数据
7	30	    TURNER	1500.00	29025	7	    测试数据
8	30	    ALLEN	1600.00	29025	8	    测试数据
9	10  	CLARK	2450.00	29025	9	    测试数据
10	30  	BLAKE	2850.00	29025	10	    测试数据
11	20  	JONES	2975.00	29025	11	    测试数据
12	20  	SCOTT	3000.00	29025	12	    测试数据
13	20  	FORD	3000.00	29025	13	    测试数据
14	10  	KING	5000.00	29025	14  	测试数据


row_number() over (partition by deptno order by sal) 排名,
按部门分组后排名

   	DEPTNO	ENAME	SAL	    总和	排名	TEST
1	10  	MILLER	1300.00	29025	1	测试数据
2	10  	CLARK	2450.00	29025	2	测试数据
3	10  	KING	5000.00	29025	3	测试数据
4	20  	SMITH	800.00	29025	1	测试数据
5	20	    ADAMS	1100.00	29025	2	测试数据
6	20  	JONES	2975.00	29025	3	测试数据
7	20  	SCOTT	3000.00	29025	4	测试数据
8	20  	FORD	3000.00	29025	5	测试数据
9	30  	JAMES	950.00	29025	1	测试数据
10	30  	MARTIN	1250.00	29025	2	测试数据
11	30  	WARD	1250.00	29025	3	测试数据
12	30  	TURNER	1500.00	29025	4	测试数据
13	30	    ALLEN	1600.00	29025	5	测试数据
14	30	    BLAKE	2850.00	29025	6	测试数据
```



### **group by rollup()**

```sql
select earnmonth,deptno,
sum(sal) 总和
from emp
--group by rollup(earnmonth,deptno)
group by earnmonth,deptno
order by earnmonth,deptno
```

```sql
group by earnmonth,deptno

   	EARNMONTH	DEPTNO	总和
1	201812	    10	    6300
2	201812	    20  	4100
3	201812	    30	    2450
4	201901	    10  	2450
5	201901	    20	    6775
6	201901	    30  	6950


group by rollup(earnmonth,deptno)
对上面的结果集再进行分组统计，对deptno进行求和，再对总的进行求和

   	EARNMONTH	DEPTNO	总和
1	201812	    10	    6300
2	201812	    20	    4100
3	201812	    30	    2450
4	201812		        12850
5	201901	    10	    2450
6	201901	    20	    6775
7	201901	    30	    6950
8	201901		        16175
9			            29025


select earnmonth,
sum(sal) 总和
from emp
group by rollup(earnmonth)
order by earnmonth

   	EARNMONTH	总和
1	201812  	12850
2	201901  	16175
3		        29025
```

 

### **group by cube()**

```sql
select earnmonth,deptno,
sum(sal) 总和
from emp
group by cube(earnmonth,deptno)
order by earnmonth,deptno

比rollup结果更多

   	EARNMONTH	DEPTNO	总和
1	201812	    10	    6300
2	201812	    20  	4100
3	201812	    30	    2450
4	201812  		    12850
5	201901  	10	    2450
6	201901  	20	    6775
7	201901	    30	    6950
8	201901  		    16175
9	        	10	    8750
10	        	20  	10875
11	        	30  	9400
12	        		    29025
                29025
```



### **grouping() 对rollup(),cube()的null进行优化显示**

**grouping(column)对列判断，如果是计算产生的null，反返回1**

```sql
select earnmonth,
case when (grouping(earnmonth)=0 and grouping(deptno1)=1) then '部门小计'
     when (grouping(earnmonth)=1 and grouping(deptno1)=1) then '总计'
       else deptno1 end deptno1,
sum(sal) 总和
from emp
group by rollup(earnmonth,deptno1)
order by earnmonth,deptno1


   	EARNMONTH	DEPTNO1	    总和
1	201812	    10	        6300
2	201812	    20      	4100
3	201812	    30      	2450
4	201812	    部门小计	12850
5	201901	    10	        2450
6	201901	    20	        6775
7	201901	    30	        6950
8	201901	    部门小计	16175
9		        总计	    29025
```

 

### **max(),min(),sum(),avg()**

```sql
select deptno,ename,sal,
max(sal) over () 最大值,
max(sal) over (partition by deptno) 部门最大值,
min(sal) over () 最小值,
avg(sal) over () 平均值,
sum(sal) over () 总和,
'测试数据' test
from emp


   	DEPTNO	ENAME	SAL	    最大值	部门最大值	最小值	平均值	            总和	TEST
1	10  	CLARK	2450.00	5000	5000	    800	    2073.21428571429	29025	测试数据
2	10  	KING	5000.00	5000	5000	    800	    2073.21428571429	29025	测试数据
3	10  	MILLER	1300.00	5000	5000	    800	    2073.21428571429	29025	测试数据
4	20	    JONES	2975.00	5000	3000	    800	    2073.21428571429	29025	测试数据
5	20	    FORD	3000.00	5000	3000	    800	    2073.21428571429	29025	测试数据
6	20	    ADAMS	1100.00	5000	3000	    800	    2073.21428571429	29025	测试数据
7	20  	SMITH	800.00	5000	3000	    800	    2073.21428571429	29025	测试数据
8	20	    SCOTT	3000.00	5000	3000	    800	    2073.21428571429	29025	测试数据
9	30	    WARD	1250.00	5000	2850	    800	    2073.21428571429	29025	测试数据
10	30	    TURNER	1500.00	5000	2850	    800	    2073.21428571429	29025	测试数据
11	30	    ALLEN	1600.00	5000	2850	    800	    2073.21428571429	29025	测试数据
12	30	    JAMES	950.00	5000	2850	    800	    2073.21428571429	29025	测试数据
13	30	    BLAKE	2850.00	5000	2850	    800	    2073.21428571429	29025	测试数据
14	30	    MARTIN	1250.00	5000	2850	    800	    2073.21428571429	29025	测试数据


```

 

 

 