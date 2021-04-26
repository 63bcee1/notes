# MySQL读书笔记

## 使用MySQL

### 主键

一列或者一组列，用于区分表中的每个行

必须满足的条件：

1，任意两行不能具有相同的主键值

2，每行都必须具有主键值（不能为NULL）

### 连接

连接到MySQL需要如下信息：

1，主机名

2，端口

3，合法的用户名

4，用户口令

### 查看数据库

```sql
show databases;
```

### 选择数据库

```
use databasename;
```

> 需要先选择数据，才能读取其中的数据

### 查看表

```
show tables;
```

### 显示数据库的列

```
`show columns from tablename;
```

> show columns 要求给出一个表名，返回每个字段的的字段名，数据类型，是否允许为空，键信息，默认值以及其他信息
>
> describe 可以作为show columns 的快捷方式

### 其他信息

```
show status # 显示服务器状态信息

show create # database 显示创建数据库信息

show create table #显示创建表信息

show grants 显示授权用户

show errors #显示错误信息

show warnings #显示警告信息
```

## 检索数据

### SELECT 语句

> SQL语句必须以分号结尾（;）
>
> SQL语句不区分大小写
>
> 检索多个列的时候，列名之间使用逗号分开
>
> 一般情况下。不要使用通配符*，这会降低程序性能

### 检索不同行

DISTINCT

```
SELECT DISTINCT vender_id from products;

# 只会返回不同的值
```

### 限制结果

LIMIT

```
SELECT prod_name from products limit 5;

# 只返回不多于五条
```

### 排序数据

ORDER BY

> ORDER BY 子句可以取一个或者多个列对输出进行排序
>
> 多列排序时，先考虑第一列的顺序，其次时下一列，也就是第一列出现相同的数值。就按第二列的规则排序

### 排序方向

DESC 降序排列

> DESC 只作用在位于其前面的列名

ASC 升序排列

> 升序是默认的

### 过滤数据

WHERE

> 在同时使用WHERE和ORDER BY时，ORDER BY应该位于WHERE之后

WHERE 操作符

```
= 等于
<> 不等于
!= 不等于
< 小于
> 大于
>= 大于等于
<= 小于等于
BETWEEN 在指定的两个值之间
```

空值

> 空值NULL 与0，空字符串，仅包含空格不同

检查是否为空

IS NULL

```
SELECT prod_name from products WHERE prod_price IS NULL;
```

### 数据过滤

AND操作符

> AND 可以给WHERE 子句附加条件

OR操作符

> 用于组合条件，匹配任一条件

圆括号

> 在任何石化使用具有AND 和OR的操作符时，都应该使用圆括号进行分组

IN操作符

> IN在多个选项时，更加直观
>
> 次序更容易管理
>
> 比OR快
>
> 可以包含其他SELECT 语句，动态创建SQL语句

NOT操作符

> 否定之后所跟的任何条件

```sql
SELECT id,prod_name,qrcode_url,qr_code,create_time FROM `ind_product` WHERE DATE(create_time) BETWEEN DATE('2021-04-13 00:00:00') AND DATE('2021-04-14 00:00:00');
```

/usr/share/nginx/html

