# LeetCode-SQL70-Notes
## Preparatory knowledge

**GROUP BY**
- GROUP BY按column_name或column_name1,...,column_nameN所组成的唯一值对数据进行分组，压缩成一行数据
- GROUP BY搭配SELECT Aggregate_fuction(column_name)，根据Aggregate_fuction()输出一条记录
- GROUP BY不搭配SELECT Aggregate_fuction(column_name)，默认取分组结果的第一条记录
- GROUP BY筛选的不是完整的一行记录，WHERE筛选的是完整的一行记录

**最大值问题的三种思路**
```
- 内层：窗口排名函数 AS ranking，外层：WHERE ranking=1
- HAVING Aggregate_function(字段)>=ALL(SELECT Aggregate_function(字段))
- ORDER BY 字段 DESC，LIMIT 1
```

**最小值问题的三种思路**
```
- 内层DENSE_RANK() OVER(ORDER BY 字段 ASC) AS ranking，外层WHERE ranking=1
- HAVING Aggregate_function()<=ALL(SELECT Aggregate_function())
- ORDER BY 字段 ASC，LIMIT 1
```

**SUM IF & COUNT IF**
```
对条件进行计数：SUM(条件)=SUM(if(条件,1,0))
对条件进行计数：COUNT(IF(条件,1,null))

注：COUNT(条件)=COUNT(条件,true,null)，条件满足返回true，否则返回null
```

**DATEDIFF()**
```
DATEDIFF(x1,x2) -- 返回x1-x2的时间差值，其中x1,x2为column_name或时间字符串
```

**DISTINCT**
```
使用范围：SELECT clause

用法一：SELECT DISTINCT column_name -- 返回一列不同名称
用法二：SELECT COUNT(DISTINCT column_name) -- 统计该列不同的名称个数
用法三：SELECT COUNT(DISTINCT column_name1,column_name2) -- 统计该两列不同的组合名称个数
```

**Window_function**

1.窗口函数的基本语法框架
```
window_function(expression) OVER(PARTITION BY expr_list ORDER BY order_list frame_clause)
````
注：frame_clause不适用于窗口排名函数，适用于窗口聚合函数。

2.窗口排名函数中PARTITION BY的细节

3.窗口聚合函数中ORDER BY的细节
- 无ORDER BY，无frame_clause：窗口聚合范围 默认组内所有行
- 有ORDER BY，无frame_clause：窗口聚合范围 默认从第一行到当前行
- 有ORDER BY，有frame_clause：窗口聚合范围 frame_clause

i.窗口聚合范围：从第一行到当前行
```
“累计”：累计求和、累计计数、累计平均数、累计最小值、累计最大值
<窗口聚合函数> OVER(ORDER BY order_list)
<窗口聚合函数> OVER(ORDER BY order_list ROWS UNBOUNDED PRECEDING AND CURRENT ROW）
```
ii.窗口聚合范围：组内所有行
```
<窗口聚合函数> OVER()
<窗口聚合函数> OVER(ORDER BY order_list ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
```
iii.窗口聚合范围：frame_clause
```
<窗口聚合函数> OVER(ORDER BY order_list *frame_clause*)
```
