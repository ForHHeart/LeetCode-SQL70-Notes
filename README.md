# LeetCode-SQL70-Notes
## Preparatory knowledge

### GROUP BY
- GROUP BY按column_name或column_name1,...,column_nameN所组成的唯一值对数据进行分组，压缩成一行数据
- GROUP BY搭配SELECT Aggregate_fuction(column_name)，根据Aggregate_fuction()输出一条记录
- GROUP BY不搭配SELECT Aggregate_fuction(column_name)，默认取分组结果的第一条记录
- GROUP BY筛选的不是完整的一行记录，WHERE筛选的是完整的一行记录

### 最大值问题的三种思路
```
- 内层：窗口排名函数 OVER(ORDER BY 字段 DESC) AS ranking，外层：WHERE ranking=1
- HAVING Aggregate_function(字段)>=ALL(SELECT Aggregate_function(字段))
- ORDER BY 字段 DESC，LIMIT 1
```

### 最小值问题的三种思路
```
- 内层：窗口排名函数 OVER(ORDER BY 字段 ASC) AS ranking，外层：WHERE ranking=1
- HAVING Aggregate_function()<=ALL(SELECT Aggregate_function())
- ORDER BY 字段 ASC，LIMIT 1
```

### SUM IF & COUNT IF
```
对条件进行计数：SUM(条件)等价于SUM(if(条件,1,0))
对条件进行计数：COUNT(IF(条件,1,null))

注：COUNT(条件)等价于COUNT(条件,true,null)，条件满足返回true，否则返回null
```

### DATEDIFF()
```
DATEDIFF(x1,x2) -- 返回x1-x2的时间差值，其中x1,x2为column_name或时间字符串
```
### IFNULL()
```
IFNULL(x,value) -- 如果x为NULL，返回value

注：指标计算中，需要外部加IFNULL()。例如IFNULL(COUNT()/COUNT(),0)
```
### ROUND()
```
ROUND(x,d)：四舍五入保留x的d位小数
```
### DISTINCT
```
使用范围：SELECT clause

用法一：SELECT DISTINCT column_name -- 返回一列不同名称
用法二：SELECT COUNT(DISTINCT column_name) -- 统计该列不同的名称个数
用法三：SELECT COUNT(DISTINCT column_name1,column_name2) -- 统计该两列不同的组合名称个数
```

1141. 查询近30天活跃用户数

597. 好友申请 I：总体通过率


### Window_function

**1.窗口函数的基本语法框架**
```
window_function(expression) OVER(PARTITION BY expr_list ORDER BY order_list frame_clause)
````
注1：frame_clause不适用于窗口排名函数，适用于窗口聚合函数。

注2：PARTITION BY在窗口排名函数和窗口聚合函数中都可省略。

**2.窗口排名函数中PARTITION BY的细节**
- 窗口排名函数，不可省略ORDER BY。
- 窗口排名函数不使用PARTITION BY进行不分组的总体排名，并打印排名。
- 窗口排名函数使用PARTITION BY进行分组的组内排名，并打印排名。
- 外部ORDER BY和窗口排名函数DENSE_RANK中不使用PARTITION BY的ORDER BY输出的总体数据排序相同，后者多了一列排名。
- 使用窗口函数后如需继续使用WHERE或HAVING筛选行，必须外层嵌套，因为窗口函数的操作顺序在WHERE和GROUP BY之后。

**3.窗口聚合函数中ORDER BY的细节**
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
