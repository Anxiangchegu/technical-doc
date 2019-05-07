## 检查

#### 数据维度（行列）
```sql
# 查看table_name的记录条数
SELECT COUNT(*) FROM table_name;

# 查看table_name的字段数量
SELECT COUNT(*) as column_num FROM information_schema.`COLUMNS` 
WHERE TABLE_NAME='table_name';

```
#### 表信息
DESC命令查看数据表的整体信息
```sql
# 查看table_name表信息
DESC table_name;
# 显示信息：Field、Type、Null、Key、Default、Extra
```
#### 查找空值
Mysql中可以使用IS NULL来判断空值。
```sql
# 查看price为空的数据
SELECT * FROM table_name WHERE price IS NULL;

# 查看price为0的数据
SELECT * FROM table_name WHERE price=0;
```
#### 查看唯一值
```sql
# 查询单列唯一值
SELECT DISTINCT (city)FROM table_name;
``` 
#### 查看列名称 
Mysql中使用COLUMNS函数用来单独查看数据表中的列名称。(与DESC查询结果一致)
```sql
# 查看数据表列名称
SHOW COLUMNS FROM table_name;
```
#### 查看前N行
```sql
# 查看数据表前5行
SELECT * FROM table_name LIMIT 3;
```

## 清洗
#### 处理缺失值(填充)
&emsp;&emsp;选择填充的方式来处理空值，使用price列的均值来填充0值字段。
具体分为两个步骤：第一步计算table_name数据表中price列的均值，并保留两位小数。第二步使用price列的均值更新price列中0值的字段。
```sql
# 计算price列的均值
SELECT ROUND(AVG(price),2) AS avg_price FROM table_name;

# 使用均值填充0值
UPDATE table_name SET price=2199.67 WHERE price=0;
```
#### 清理空格
```sql
# 清理字符中的空格
UPDATE table_name SET city = TRIM(city);
```
> 清理空格还有其它函数，详情列表请查看体系文档。

#### 数值修改及替换
```sql
# 在table_name表中，将city字段中含有的“成都”替换为“成都市”。
UPDATE table_name SET city = REPLACE(city,'成都','成都市');
```
#### 百分比
```sql
-- CONCAT('a/b','%')代表拼接%
SELECT CONCAT(CAST(round((1/2)*100,2) AS CHAR),'%') AS percentage FROM table_name; 
```

## 预处理
#### 数据表匹配合并
&emsp;&emsp;类似Excel中的`VLOOKUP()`函数和Power Query的合并查询功能。
其中与Power Query的“左外部、右外部、完全外部等”有类似功能，SQL表连接JOIN匹配常用的模式有INNER JOIN，LEFT JOIN，和RIGHT JOIN。
```sql
/* 为便于理解 */
# 内连接
SELECT 学员信息表.*, 学员成绩表.* 
FROM 学员信息表 INNER JOIN 学员成绩表 ON 学员信息表.学号 = 学员成绩表.学号;

# 左连接
SELECT 学员信息表.*, 学员成绩表.* 
FROM 学员信息表 LEFT JOIN 学员成绩表 ON 学员信息表.学号 = 学员成绩表.学号;

# 右连接
SELECT 学员信息表.*, 学员成绩表.* 
FROM 学员信息表 RIGHT JOIN 学员成绩表 ON 学员信息表.学号 = 学员成绩表.学号;
```
#### 排序
```sql
# 按price 升序排列
SELECT * FROM table_name ORDER BY price;

# 按price 降序排列
SELECT * FROM table_name ORDER BY price DESC;
```
#### 数组分组
```sql
/* 为了便于理解，贴一个我曾经的写过的语句  */
-- 通过日期按星期分类汇总
SELECT `区域城市`,`宴会编码`,`跟单员`,`日期`,
	CASE DAYNAME(日期) 
		WHEN 'Monday'   THEN '星期一'
		WHEN 'Tuesday'   THEN '星期二'  
		WHEN 'Wednesday'   THEN '星期三'  
		WHEN 'Thursday'   THEN '星期四'
		WHEN 'Friday'   THEN '星期五'  
		WHEN 'Saturday'   THEN '星期六'  
		WHEN 'Sunday'   THEN '星期日' 				
	ELSE '其他' END 
	as 星期,时间,`桌数`,`购酒数`,`宴会类型`
FROM `宴会基础信息表`

-- 直接分组查询并汇总
SELECT COUNT(id) AS id_count, SUM(price)AS total_price,
CASE
    WHEN age<30 THEN 'A'
    WHEN age>=30 AND age<50 THEN 'B'
    WHEN age>=50 THEN 'C'
    ELSE 'D' END AS age_type
FROM table_name GROUP BY age_type ORDER BY id_count;
```
#### 数据分列

```sql
# category依据 “-” 分列
SELECT SUBSTRING_INDEX(category,'-',1)AS size,SUBSTRING_INDEX(category,'-',-1)AS colour 
FROM table_name;

# 按分列后的结果进行单列数据汇总
SELECT SUBSTRING_INDEX(category,'-',1) AS size,COUNT(id) 
FROM table_name GROUP BY size;

# 按分列后的结果进行多列数据汇总
SELECT 
    SUBSTRING_INDEX(category,'-',1) AS size,
    COUNT(id) AS id_count,ROUND(SUM(price),2) AS total_price 
FROM table_name GROUP BY size;

# 更新分列后的字段内容
UPDATE table_name SET size = SUBSTRING_INDEX(category,'-',1),colour =SUBSTRING_INDEX(category,'-',-1);
```
## 提取
#### 按列提取
```sql
SELECT city FROM table_name;
```
#### 按行提取
```sql
SELECT * FROM table_name WHERE city='成都';
```
#### 按行号提取
```sql
# 提取2到5行（4行）
SELECT * FROM table_name LIMIT 2,5;
```
#### 按条件提取
```sql
# 按条件提取并计算
SELECT AVG(price) FROM table_name WHERE city='成都' AND price < 19.88;
```
#### 从JSON中提取
```sql
SELECT JSON_EXTRACT('{"name":"沐之杰","age":"13240133388"}',"$.name");
```
#### 复杂提取
```sql
-- 38°珍藏级剑南春（名烟酒）[500ml]
# 假设table_name表中的col_name列有这样一个值：38°珍藏级剑南春（名烟酒）[500ml]，需要分别提取38°、珍藏级剑南春、名烟酒、500ml
SELECT 
    SUBSTRING_INDEX(col_name,'°',1) as 度数,  -- 38
    SUBSTRING_INDEX(SUBSTRING_INDEX(col_name,'（',1),'°',-1) as 品名,  -- 珍藏级剑南春
    SUBSTRING_INDEX(col_name,'）',-1) as 规格,  -- [500ml]
    SUBSTRING_INDEX(SUBSTRING_INDEX(col_name,"（",-1),'）',1) as 版本,  -- 名烟酒
    SUBSTRING_INDEX(SUBSTRING_INDEX(col_name,"[",-1),']',1) as 容量  -- 500ml
FROM table_name
```

## 筛选
&emsp;&emsp;按条件筛选需要用到算术运算、逻辑运算符以及LIKE通配符等。
```sql
# 筛选city不等于成都的
SELECT * FROM table_name WHERE city !='成都';

# 筛选name(模糊筛选)中以“王”开头的
SELECT * FROM table_name WHERE city LIKE '王%';

#筛选后计数
SELECT COUNT(id) AS id_count FROM table_name WHERE city='成都'AND price < 19.88;
```
## 统计汇总
&emsp;&emsp;使用分类汇总可以按特定维度对数据进行统计分析，mysql中使用的主要函数是
GROUP BY、CASE WHEN以及GROUP_CONCAT。
```sql
/* 来个复杂点儿的 */
# 查询不同尺码下的不同颜色的产品销售金额
SELECT sizenote,colornote,goodsid,sum(goodsprice * amount) AS 销售额
FROM orderdetail LEFT JOIN goodssize ON orderdetail.sizeid = goodssize.sizeid
LEFT JOIN goodscolor ON orderdetail.colorid = goodscolor.colorid
GROUP BY sizenote, colornote, goodsid
ORDER BY sizenote, colornote, 销售额 DESC;

# 查询销售大区对应哪些办事处
SELECT `销售大区`,GROUP_CONCAT(`办事处`) FROM `2018年某牌销售表` GROUP BY `销售大区`;
```
