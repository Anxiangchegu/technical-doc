## 思维导图
`因不同数据库操作语句有差异，本页语句以MySql 8.0为例`

![SQL导图](../_images/Sql_mind_map.png "导图")
##  数据库查询语言（DQL）

### 1. 顺序
##### 1.1 语法顺序
```sql
# SELECT 语句由子句构成， 最重要的子句有：  
SELECT columns_name       --查找一列或多列，多列之间用逗号隔开
FROM Table                --目标表
WHERE condition           --过滤条件
GROUP BY columns_name     --按列值分组，可以1个或多个列
HAVING condition          --分组后的筛选条件，HAVING与WHERE区别在于前者表达式中可包含函数
ORDER BY columns_name     --按列排序
LIMIT start, row_count    --对结果进行限定，start表示从哪行开始，row_count表示结果行数

这些总是使用这个顺序。
```
##### 1.2 执行顺序
```sql
FROM
WHERE
GROUP BY
HAVING
SELECT
DISTINCT
UNION
ORDER BY
```
### 2. 语法及示例
> SELECT语句  

- 多列查询  

```sql
SELECT [DISTINCT] <目标列组>
    FROM <数据源>
    [WHERE <元组选择条件>]
    [GROUP BY <分列组> [HAVING <组选择条件>]]
    [ORDER BY <排序1列> <排序2列>]
    

# 例子
SELECT FirstName, LastName, OrderDate
FROM Orders WHERE OrderDate > '10/10/2010'

# 去重
SELECT DISTINCT FirstName FROM Orders
```

`SELECT * 会增加很多不必要的消耗（CPU、IO、内存、带宽）`  

- 从第一行开始选择一共五行数据
  
```sql
SELECT FirstName FROM Orders  limt 5 offset 1;
```
- 排序  

```sql
# 语法
SELECT co_name1,col_name2 
FROM table_name 
ORDER BY col_name1 DESC,col_name2 ASC

#例子
SELECT ,FirstName
FROM Orders 
ORDER BY OrderDate DESC
```
- 常用条件过滤

```sql
# 范围内检查
SELECT col_name FROM table_name WHERE col_name BETEWEEN 5 AND 10;

# 空值检查
SELECT col_name FROM table_name WHERE col_name IS null;

# in操作符
SELECT col_name FROM table_name WHERE col_name IN (....);

# NOT操作符
SELECT col_name FROM table_name WHERE NOT (条件);

# AND 和 OR 注意优先级 and 优先级高于or
SELECT col_name FROM table_name 
WHERE (condi_1 OR condi_2) AND condi_3;

# 使用通配符进行过滤
SELECT col_name FROM table_name WHERE col_name like '..%..';

SELECT col_name FROM table_name WHERE col_name like '.._..';

SELECT col_name FROM table_name WHERE col_name like '[]%';
```
- 分组并按条件筛选

```sql
# 语法
SELECT <Column List>, <Aggregate Function>(<Column Name>)
FROM <Table Name>
WHERE <Search Condition>
GROUP BY <Column List>

# 例子
SELECT LastName, SUM(OrderValue)
FROM Orders
WHERE OrderDate > '10/10/2010'
GROUP BY LastName

注意： 
GROUP BY 后面也可以接条件。
WHERE 语句和HAVING配合的使用。WHERE在HAVING之前。
WHERE 过滤针对的是行，HAVING过滤针对的是组。
```
- 分组和排序的顺序

```sql
/*
SELECR 
FROM 
WHERE
GROUP BY 
HAVING 
ORDER BY
*/
SELECT  col_name FROM table_name
WHERE <condition_1>
GROUP BY <col_name>
HAVING <condition_2>
ORDER BY <col_name>
```

> 子查询

&emsp;&emsp;写在()中，把内层查询结果当做外层查询参照的数据表来用。   
```sql
SELECT col_name FROM table_name 
WHERE  col_name  = 
(SELECT  col_name  FROM table_name WHERE .... );
```
- 一些小例子

```sql
-- 用and操作符查询s_id为101并且f_id为a1的水果记录
select * from fruits
where s_id = 101 and f_id = 'a1';

-- 用or操作符查询苹果或者橙子的相关记录
select * from fruits
where f_name = 'apple' or f_name = 'orange';

-- 用in操作符查询苹果和橙子的相关记录
select * from fruits
where f_name in('apple','orange');

-- 用not in操作符查询苹果和橙子之外的水果的相关记录
select * from fruits
where f_name not in('apple','orange');

-- 用between...and操作符查询f_price在10元到20元之间的水果记录
select * from fruits
where f_price between 10 and 20;

-- 用like操作符查询所有f_name由a开始的水果记录
select * from fruits
where f_name like 'a%';

-- 用like操作符查询所有f_id由b开始且字符长度为两位的水果记录
select * from fruits
where f_id like 'b_';

-- 用is null操作符查询所有f_name为空的水果记录
select * from fruits
where f_name is null;

-- 查询fruits表中所有不重复的s_id
select distinct s_id from fruits;

-- 用any操作符与子查询语句来查询所有f_id对应的f_price在10元到20元之间的水果记录
select * from fruits where f_id =  any
(select f_id from fruits where f_price between 10 and 20);


-- 用all操作符与子查询语句来查询所有f_price大于20元的水果记录
select * from fruits where f_price > all
(select f_price from fruits where f_price <= 20);

-- 用exists操作符与子查询语句来查询是否存在f_price大于30元的水果记录
select * from fruits where exists
(select * from fruits where f_price > 30);

-- 用as将fruits表名重命名为f后使用
select f.* from fruits as f;

-- 显示f_price金额最大的前三名水果记录
select * from fruits
order by f_price desc
limit 3;
```
> 表的连接查询

```sql
# 语法
SELECT <Column List>
FROM <Table1> JOIN <Table2>
ON <Table1>.<Column1> = <Table2>.<Column1>、

# 示例
SELECT Orders.LastName, Countries.CountryName
FROM Orders INNER JOIN Countries ON
Orders.CountryID = Countries.ID

除此之外还有：
左联结 LEFT JOIN
右联结 RIGHT JOIN
全联结 FULL JOIN
```
> 联合查询

```sql
# 语法
SELECT <Column List> FROM <Table1>
UNION
SELECT <Column List> FROM <Table2>

# 示例
SELECT <Column List> FROM <Table1>
UNION
SELECT <Column List> FROM <Table2>
```
## 数据库操作语言（DML）

##### 忠告
?> 如果直接操作数据库，INSERT、UPDATE、DELETE前一定要先SELECT一下看看是不是目标数据，这是血的经验教训。

> INSERT 插入

```sq1
INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```

> UPDATE 更新

```sql
UPDATE table_name SET 列名称 = 新值 WHERE 列名称 = 某值
```

> DELETE 删除

&emsp;&emsp;delete语句是数据库操作语言(dml)，这个操作会放到 rollback segement 中，事务提交之后才生效；如果有相应的 trigger，执行的时候将被触发。
```sql
# delete 语句用于删除表中的行。
DELETE FROM 表名称 WHERE 列名称 = 值
```

## 数据库定义语言（DDL）

> CREATE 创建表

```sql
# 语法
CREATE TABLE <Table Name>
( Column1 DataType,
 Column2 DataType,
 Column3 DataType)
 
# 示例
create table table_name
(
    ExamNo      int     identity(1,1) primary key,
    stuNo       char(6) not null,
    writtenExam int     not null,
    LabExam     int     not null
)
```
> ALTER 更改表

```sql
# 语法
CREATE TABLE <Table Name>
ADD col_name Datatype;

CREATE TABLE <Table Name>
DROP COLUMN <col_name>
 
# 例子
ALTER TABLE Vendors 
ADD vend_phone CHAR(20); 

ALTER TABLE Vendors 
DROP COLUMN vend_phone; 
```

> DROP 删除表

&emsp;&emsp;truncate、drop 是数据库定义语言(ddl)，操作立即生效，原数据不放到 rollback segment 中，不能回滚，操作不触发 trigger。 
```sql
# 删除内容和定义，释放空间。
DROP TABLE table_name;

# 例子
DROP TABLE CustCopy; 
```

> TRUNCATE 清空表

&emsp;&emsp;如果有ROLLBACK命令Delete将被撤销，而TRUNCATE则不会被撤销。 
```sql
# 删除内容、释放空间但不删除定义(保留表的数据结构)。
TRUNCATE TABLE table_name;
```

## 数据库控制语言（DCL）

> GRANT 授权

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypasswd' WITH GRANT OPTION;
```
> ROLLBACK [WORK] TO [SAVEPOINT] 回退到某一点

> COMMIT [WORK] 提交

## 常用函数公式
### 1. 数学函数

| 函数 | 	说明 | 
| :---         |     :---:      |
| ABS(x) | 返回x的绝对值 |
| BIN(x) | 返回x的二进制（OCT返回八进制，HEX返回十六进制） |
| CEILING(x) | 返回大于x的最小整数值 |
| EXP(x) | 返回值e（自然对数的底）的x次方|    
| FLOOR(x) | 返回小于x的最大整数值 |
| GREATEST(x1,x2,...,xn) | 返回集合中最大的值 |
| LEAST(x1,x2,...,xn) | 返回大于x的最小的值 |
| LN(x) | 返回x的自然对数 |    
| LOG(x,y) | 返回x的以y为底的对数 |
| MOD(x,y) | 返回x/y的模（余数） |
| PI() | 返回pi的值（圆周率） |
| RAND() | 返回０到１内的随机值,可以通过提供一个参数(种子)使RAND()随机数生成器生成一个指定的值。|    
| ROUND(x,y) | 返回参数x的四舍五入的有y位小数的值 |    
| SIGN(x)  | 返回代表数字x的符号的值 |
| SQRT(x)  | 返回一个数的平方根 |
| TRUNCATE(x,y)  | 返回数字x截短为y位小数的结果 |
 
### 2. 文本/字符串函数

| 函数 | 	说明 | 
| :---         |     :---:      |
| ASCII(char) | 返回字符的ASCII码值 |
| BIT_LENGTH(str) | 返回字符串的比特长度 |
| CONCAT(s1,s2...,sn) | 将s1,s2...,sn连接成字符串 |
| CONCAT_WS(sep,s1,s2...,sn) | 将s1,s2...,sn连接成字符串，并用sep字符间隔|    
| INSERT(str,x,y,instr) | 将字符串str从第x位置开始，y个字符长的子串替换为字符串instr，返回结果 |
| FIND_IN_SET(str,list) | 分析逗号分隔的list列表，如果发现str，返回str在list中的位置 |
| LCASE(str)或LOWER(str)  | 返回将字符串str中所有字符改变为小写后的结果 |
| UCASE(str)或UPPER(str)  | 返回将字符串str中所有字符改变为大写后的结果 |
| LEFT(str,x) | 返回字符串str中最左边的x个字符 |    
| LTRIM(str)  | 去掉字符串str中开头的空格 |
| RIGHT(str,x) | 返回字符串str中最右边的x个字符 |   
| RTRIM(str)  | 去掉字符串str尾部的空格 |   
| LENGTH(s) | 返回字符串str中的字符数 |
| POSITION(substr,str) | 返回子串substr在字符串str中第一次出现的位置 |    
| QUOTE(str)  |  用反斜杠转义str中的单引号 |    
| REPEAT(str,srchstr,rplcstr)  | 返回字符串str重复x次的结果 |
| STRCMP(s1,s2)  | 比较字符串s1和s2 | 
| SUBSTRING（str, pos） | SUBSTRING（被截取字段，从第几位开始截取，截取长度） | 
| SUBSTRING_INDEX（str,delim,count） | SUBSTRING_INDEX（被截取字段，关键字，关键字出现的次数） |  

### 3. 日期及时间函数

| 函数 | 	说明 | 
| :---         |     :---:      |
| CURDATE()或CURRENT_DATE()  | 返回当前的日期 |
| CURTIME()或CURRENT_TIME() | 返回当前的时间 |
| DATE_ADD(date,INTERVAL int keyword) | 返回日期date加上间隔时间int的结果(int必须按照关键字进行格式化),如：SELECTDATE_ADD(CURRENT_DATE,INTERVAL 6 MONTH) |
| DATE_FORMAT(date,fmt)  | 依照指定的fmt格式格式化日期date值 |    
| DATE_SUB(date,INTERVAL int keyword) | 返回日期date加上间隔时间int的结果(int必须按照关键字进行格式化),如：SELECTDATE_SUB(CURRENT_DATE,INTERVAL 6 MONTH) |
| DAYOFWEEK(date) |  返回date所代表的一星期中的第几天(1~7) |
| DAYOFMONTH(date) | 返回date是一个月的第几天(1~31) |
| DAYOFYEAR(date) | 返回date是一年的第几天(1~366) |
| DAYNAME(date) | 返回date的星期名，如：SELECT DAYNAME(CURRENT_DATE) |    
| FROM_UNIXTIME(ts,fmt) | 根据指定的fmt格式，格式化UNIX时间戳ts |
| HOUR(time) | 返回time的小时值(0~23) |   
| MINUTE(time) | 返回time的分钟值(0~59) |   
| MONTH(date) | 返回date的月份值(1~12) |
| MONTHNAME(date) | 返回date的月份名，如：SELECT MONTHNAME(CURRENT_DATE) |    
| NOW() | 返回当前的日期和时间 |    
| QUARTER(date) | 返回date在一年中的季度(1~4)，如SELECT QUARTER(CURRENT_DATE) |
| WEEK(date) | 返回日期date为一年中第几周(0~53) |  
| YEAR(date) | 返回日期date的年份(1000~9999) |  

### 4. 聚合函数
> 常用于GROUP BY从句的SELECT查询中

| 函数 | 	说明 | 
| :---         |     :---:      |
| AVG(col) | 返回指定列的平均值 |
| COUNT(col) | 返回指定列中非NULL值的个数 |
| MIN(col) | 返回指定列的最小值 |
| MAX(col) | 返回指定列的最大值 |    
| SUM(col) | 返回指定列的所有值之和 |

### 5. 其它函数
| 函数 | 	说明 | 
| :---         |     :---:      |
| GROUP_CONCAT(col) | 返回由属于一组的列值连接组合而成的结果 |
| CAST() | 将一个值转换为指定的数据类型 |
| JSON_EXTRACT | 从json对象中取值 |
> GROUP_CONCAT()函数： 常与关键字 GROUP BY 一起使用，能够将分组后的指定字段值都显示出来。

```sql
-- 使用abs函数求所有水果平均值与最大值差值的绝对值
select abs(avg(f_price)-max(f_price)) from fruits;

-- 使用length函数求每个f_name的名字与他们的字符长度
select f_name, length(f_name) from fruits group by f_name;

-- 使用now函数求当前的日期和时间
select now();

-- 使用group_concat函数查询不同s_id下对应的所有f_name信息
SELECT s_id, GROUP_CONCAT(f_name) FROM fruits
GROUP BY s_id;

--  使用concat函数在f_name字段值前添加'fruit_'信息
update fruits set f_name = concat('fruit_',f_name);
```
### 6. 比较运算符

| 符号 | 说明 | 备注|
| :---         |     :---:      |		:---:      |
|=	|等于	||
|<>, != |	不等于||	
|>	|大于|	|
|<	|小于|	|
|<=	|小于等于||	
|>=	|大于等于||	
|BETWEEN	|在两值之间|	>=min&&<=max |
|NOT BETWEEN	|不在两值之间||	
|IN|	在集合中	||
|NOT IN	|不在集合中|	
|<=>	|严格比较两个NULL值是否相等 |	两个操作码均为NULL时，其所得值为1；而当一个操作码为NULL时，其所得值为0 |
|LIKE	| 模糊匹配|	|
|REGEXP 或 RLIKE	| 正则式匹配||	
|IS NULL	|为空|	|
|IS NOT NULL	|不为空||	



## like 操作符
查询name字段中以“王”开头的所有员工：
```text
SELECT * FROM employee WHERE name LIKE '%王';
```
## SQL 通配符
SQL 通配符必须与 LIKE 运算符一起使用

| 通配符 | 	说明 | 
| :---         |     :---:      |
| % | 匹配一个或多个字符 |
| _ | 仅匹配一个字符 |
| [charlist] | 字符列中的任何单一字符 |
| [^charlist] 或者 [!charlist] | 不在字符列中的任何单一字符 |

示例表示
```sql
'%a'    //以a结尾的数据
'a%'    //以a开头的数据
'%a%'    //含有a的数据
'_a_'    //三位且中间字母是a的
'_a'    //两位且结尾字母是a的
'a_'    //两位且开头字母是a的
```
## TOP
```sql
SELECT column_name(s)
FROM table_name
LIMIT number
```


## 其它操作符
速查列表，一看就懂。

| 通配符 | 	说明 | 
| :---         |     :---:      |
| IN | SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,...) |
| BETWEEN ... AND | SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2 |
| 别名（Alias） | SELECT column_name AS alias_name FROM table_name |
| INNER JOIN | 在表中存在至少一个匹配时，INNER JOIN 关键字返回行。 |
| LEFT JOIN | SELECT column_name(s) FROM table_name1 LEFT JOIN table_name2 ON table_name1.column_name=table_name2.column_name |

> 一些示例

```sql
-- 创建大气质量表
create table Monthly_Indicator(
	city_name varchar(20) not null,
    month_key date not null,
    aqi int(4) not null default 0,
    aqi_range varchar(20) not null default '-',
    air_quality varchar(20) not null default '-',
    pm25 float(6,2) not null default 0,
    pm10 float(6,2) not null default 0,
    so2 float(6,2) not null default 0,
    co float(6,2) not null default 0,
    no2 float(6,2) not null default 0,
    o3 float(6,2) not null default 0,
    ranking int(4) not null default 0,
    primary key(city_name,month_key)
    );
    
-- 为Monthly_Indicator表导入外部txt文件
load data local infile 'D:/liwork/CDA/MySQL - Li Qi/data/all.txt' 
	into table Monthly_Indicator 
    fields terminated by '\t'
    ignore 1 lines;
    
-- SQL语句单表查询练习
-- 查询不同月份PM2.5的最大值
select month_key, max(pm25) from monthly_indicator
group by month_key;

-- 降序查询不同城市PM10的平均值
select city_name, avg(pm10) from monthly_indicator
group by city_name
order by avg(pm10) desc;

-- 对大气质量表进行有选择的查询
select city_name, avg(pm25), avg(pm10) from Monthly_Indicator
where pm25 > 50
group by city_name, month_key having city_name <> '北京'
order by avg(pm25) desc;

-- 多表连接练习
-- 创建学员信息表
create table 学员信息表(
	学号 varchar(5),
    学员姓名 varchar(10),
    年龄 int
);

-- 创建学员成绩表
create table 学员成绩表(
	学号 varchar(5),
    成绩 int
);

-- 为学员成绩表导入数据
load data local infile 'D:/liwork/SQL/data/xycj.csv' 
	into table 学员成绩表
    fields terminated by ','
    ignore 1 lines;

-- 内连接
select 学员信息表.*, 学员成绩表.* 
from 学员信息表 inner join 学员成绩表 on 学员信息表.学号 = 学员成绩表.学号;

-- 左连接
select 学员信息表.*, 学员成绩表.* 
from 学员信息表 left join 学员成绩表 on 学员信息表.学号 = 学员成绩表.学号;

-- 右连接
select 学员信息表.*, 学员成绩表.* 
from 学员信息表 right join 学员成绩表 on 学员信息表.学号 = 学员成绩表.学号;

-- 纵向合并练习
create table t1(
	key1 varchar(20),
    v1 int(4)
    );
    
load data local infile 'D:/liwork/SQL/data/t1.csv' 
	into table t1
    fields terminated by ','
    ignore 1 lines;
    
create table t2(
	key2 varchar(20),
    v2 int(4)
    );

load data local infile 'D:/liwork/SQL/data/t2.csv' 
	into table t2
    fields terminated by ','
    ignore 1 lines;

-- 用union合并t1与t2表
select t1.* from t1
union
select t2.* from t2;

-- 用union all合并t1与t2表
select t1.* from t1
union all
select t2.* from t2;
```

## 复杂语句示例
```sql
/*数据统计查询 分类 排序*/
select 区域城市,COUNT(0)as 宴会场次,SUM(桌数) as 桌数, SUM(购酒数) as 购酒数, SUM(实际回收瓶盖) as 瓶盖回收数,
	COUNT(终审结果 = '通过' OR NULL) as 通过,
	COUNT(终审结果 = '不通过' OR NULL) as 不通过,
	COUNT(终审结果 ='审核中' OR NULL) as 审核中,
	COUNT(终审结果 = '降档通过' OR NULL) as 降档通过,
	COUNT(星期 = '星期一' OR NULL) as 星期一,
	COUNT(星期 = '星期二' OR NULL) as 星期二,
	COUNT(星期 = '星期三' OR NULL) as 星期三,
	COUNT(星期 = '星期四' OR NULL) as 星期四,
	COUNT(星期 = '星期五' OR NULL) as 星期五,
	COUNT(星期 = '星期六' OR NULL) as 星期六,
	COUNT(星期 = '星期日' OR NULL) as 星期日,
	COUNT(时间 = '早上' OR NULL) as 早上,
	COUNT(时间 = '中午' OR NULL) as 中午,
	COUNT(时间 = '晚上' OR NULL) as 晚上,
	COUNT(时间 = '中午和晚上' OR NULL) as 中午和晚上
FROM
(SELECT 区域城市,宴会编码,终审结果,跟单员,日期,
 CASE DAYNAME(日期) 
		WHEN 'Monday'     THEN '星期一'
		WHEN 'Tuesday'     THEN '星期二'  
		WHEN 'Wednesday'     THEN '星期三'  
		WHEN 'Thursday'     THEN '星期四'
		WHEN 'Friday'     THEN '星期五'  
		WHEN 'Saturday'     THEN '星期六'  
		WHEN 'Sunday'     THEN '星期日' 				
	ELSE '其他' END
	星期,时间,桌数,购酒数,实际回收瓶盖,宴会类型1
FROM
 (SELECT 区域城市,宴会编码,终审结果,跟单员, SUBSTRING_INDEX(宴会时间,' ',1)as 日期,  
SUBSTRING_INDEX(宴会时间,' ',-1)as 时间,桌数,购酒数,实际回收瓶盖,宴会类型1
FROM export_828) as t1) as t2 GROUP BY 区域城市 ORDER BY 宴会场次 DESC

/*按日期统计 排序 convert*/
SELECT 日期,COUNT(1)as 当天宴会数量,区域城市,宴会编码,终审结果,跟单员,桌数,购酒数,实际回收瓶盖,宴会类型1
FROM 
(SELECT 区域城市,宴会编码,终审结果,跟单员, SUBSTRING_INDEX(宴会时间,' ',1)as 日期,
SUBSTRING_INDEX(宴会时间,' ',-1)as 时间,桌数,购酒数,实际回收瓶盖,宴会类型1
FROM export_828) as t1
GROUP BY 日期
ORDER BY CONVERT(日期,date) ASC

/* 按日期、城市、宴会类型 查询统计*/
SELECT 日期,COUNT(1)as 当天宴会数量,
	COUNT(`区域城市` = '成都' OR NULL) AS 成都,
	COUNT(`区域城市` = '德阳' OR NULL) AS 德阳,
	COUNT(`区域城市` = '嘉兴' OR NULL) AS 嘉兴,
	COUNT(`区域城市` = '遂宁' OR NULL) AS 遂宁,
	COUNT(`区域城市` = '盐城' OR NULL) AS 盐城,
	COUNT(`宴会类型1` = '升学宴' OR NULL) AS 升学宴数量
FROM 
(SELECT 区域城市,宴会编码,终审结果,跟单员, SUBSTRING_INDEX(宴会时间,' ',1)as 日期,
SUBSTRING_INDEX(宴会时间,' ',-1)as 时间,桌数,购酒数,实际回收瓶盖,宴会类型1
FROM export_828) as t1
GROUP BY 日期
ORDER BY CONVERT(日期,date),`区域城市`,`宴会类型1` ASC

/* 查询不同颜色下的产品销售金额 */
select colornote as 颜色, goodsid,sum(goodsprice * amount) as 销售额
from orderdetail left join goodscolor on orderdetail.colorid = goodscolor.colorid
group by 颜色, goodsid
order by 颜色, 销售额 desc;
```