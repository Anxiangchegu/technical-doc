## 薪水中位数
Employee 表包含所有员工。Employee 表有三列：员工Id，公司名和薪水。
```sql
+-----+------------+--------+
|Id   | Company    | Salary |
+-----+------------+--------+
|1    | A          | 2341   |
|2    | A          | 341    |
|3    | A          | 15     |
|4    | A          | 15314  |
|5    | A          | 451    |
|6    | A          | 513    |
|7    | B          | 15     |
|8    | B          | 13     |
|9    | B          | 1154   |
|10   | B          | 1345   |
|11   | B          | 1221   |
|12   | B          | 234    |
|13   | C          | 2345   |
|14   | C          | 2645   |
|15   | C          | 2645   |
|16   | C          | 2652   |
|17   | C          | 65     |
+-----+------------+--------+
```
请编写SQL查询来查找每个公司的薪水中位数。挑战点：你是否可以在不使用任何内置的SQL函数的情况下解决此问题。
```sql
+-----+------------+--------+
|Id   | Company    | Salary |
+-----+------------+--------+
|5    | A          | 451    |
|6    | A          | 513    |
|12   | B          | 234    |
|9    | B          | 1154   |
|14   | C          | 2645   |
+-----+------------+--------+
```
#### 参考答案
```sql
SELECT
    Employee.Id, Employee.Company, Employee.Salary
FROM
    Employee,
    Employee alias
WHERE
    Employee.Company = alias.Company
GROUP BY Employee.Company , Employee.Salary
HAVING SUM(CASE
    WHEN Employee.Salary = alias.Salary THEN 1
    ELSE 0
END) >= ABS(SUM(SIGN(Employee.Salary - alias.Salary)))
ORDER BY Employee.Id
;
```

---
## 工资前三高的所有员工
Employee 表包含所有员工信息，每个员工有其对应的工号 Id，姓名 Name，工资 Salary 和部门编号 DepartmentId 。
```sql
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
+----+-------+--------+--------------+
```
Department 表包含公司所有部门的信息。
```sql
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```
编写一个 SQL 查询，找出每个部门获得前三高工资的所有员工。例如，根据上述给定的表，查询结果应返回：
```sql
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```
解释：IT 部门中，Max 获得了最高的工资，Randy 和 Joe 都拿到了第二高的工资，Will 的工资排第三。销售部门（Sales）只有两名员工，Henry 的工资最高，Sam 的工资排第二。
#### 参考答案
```sql
SELECT
    d.Name AS 'Department', e1.Name AS 'Employee', e1.Salary
FROM
    Employee e1
        JOIN
    Department d ON e1.DepartmentId = d.Id
WHERE
    3 > (SELECT
            COUNT(DISTINCT e2.Salary)
        FROM
            Employee e2
        WHERE
            e2.Salary > e1.Salary
                AND e1.DepartmentId = e2.DepartmentId
        )
;
```
---
## 行程和用户
表：Trips
```sql
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| Id          | int      |
| Client_Id   | int      |
| Driver_Id   | int      |
| City_Id     | int      |
| Status      | enum     |
| Request_at  | date     |     
+-------------+----------+
Id 是这张表的主键。
这张表中存所有出租车的行程信息。每段行程有唯一 Id ，其中 Client_Id 和 Driver_Id 是 Users 表中 Users_Id 的外键。
Status 是一个表示行程状态的枚举类型，枚举成员为(‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’) 。
```
表：Users
```sql
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| Users_Id    | int      |
| Banned      | enum     |
| Role        | enum     |
+-------------+----------+
Users_Id 是这张表的主键。
这张表中存所有用户，每个用户都有一个唯一的 Users_Id ，Role 是一个表示用户身份的枚举类型，枚举成员为 (‘client’, ‘driver’, ‘partner’) 。
Banned 是一个表示用户是否被禁止的枚举类型，枚举成员为 (‘Yes’, ‘No’) 。
```
写一段 SQL 语句查出 "2013-10-01" 至 "2013-10-03" 期间非禁止用户（乘客和司机都必须未被禁止）的取消率。非禁止用户即 Banned 为 No 的用户，禁止用户即 Banned 为 Yes 的用户。

取消率 的计算方式如下：(被司机或乘客取消的非禁止用户生成的订单数量) / (非禁止用户生成的订单总数)。

返回结果表中的数据可以按任意顺序组织。其中取消率 Cancellation Rate 需要四舍五入保留 两位小数 。

 

查询结果格式如下例所示：
```SQL
Trips 表：
+----+-----------+-----------+---------+---------------------+------------+
| Id | Client_Id | Driver_Id | City_Id | Status              | Request_at |
+----+-----------+-----------+---------+---------------------+------------+
| 1  | 1         | 10        | 1       | completed           | 2013-10-01 |
| 2  | 2         | 11        | 1       | cancelled_by_driver | 2013-10-01 |
| 3  | 3         | 12        | 6       | completed           | 2013-10-01 |
| 4  | 4         | 13        | 6       | cancelled_by_client | 2013-10-01 |
| 5  | 1         | 10        | 1       | completed           | 2013-10-02 |
| 6  | 2         | 11        | 6       | completed           | 2013-10-02 |
| 7  | 3         | 12        | 6       | completed           | 2013-10-02 |
| 8  | 2         | 12        | 12      | completed           | 2013-10-03 |
| 9  | 3         | 10        | 12      | completed           | 2013-10-03 |
| 10 | 4         | 13        | 12      | cancelled_by_driver | 2013-10-03 |
+----+-----------+-----------+---------+---------------------+------------+

Users 表：
+----------+--------+--------+
| Users_Id | Banned | Role   |
+----------+--------+--------+
| 1        | No     | client |
| 2        | Yes    | client |
| 3        | No     | client |
| 4        | No     | client |
| 10       | No     | driver |
| 11       | No     | driver |
| 12       | No     | driver |
| 13       | No     | driver |
+----------+--------+--------+

Result 表：
+------------+-------------------+
| Day        | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 | 0.33              |
| 2013-10-02 | 0.00              |
| 2013-10-03 | 0.50              |
+------------+-------------------+

2013-10-01：
  - 共有 4 条请求，其中 2 条取消。
  - 然而，Id=2 的请求是由禁止用户（User_Id=2）发出的，所以计算时应当忽略它。
  - 因此，总共有 3 条非禁止请求参与计算，其中 1 条取消。
  - 取消率为 (1 / 3) = 0.33
2013-10-02：
  - 共有 3 条请求，其中 0 条取消。
  - 然而，Id=6 的请求是由禁止用户发出的，所以计算时应当忽略它。
  - 因此，总共有 2 条非禁止请求参与计算，其中 0 条取消。
  - 取消率为 (0 / 2) = 0.00
2013-10-03：
  - 共有 3 条请求，其中 1 条取消。
  - 然而，Id=8 的请求是由禁止用户发出的，所以计算时应当忽略它。
  - 因此，总共有 2 条非禁止请求参与计算，其中 1 条取消。
  - 取消率为 (1 / 2) = 0.50
```

#### 参考答案
```sql
SELECT T.request_at AS `Day`, 
	ROUND(
			SUM(
				IF(T.STATUS = 'completed',0,1)
			)
			/ 
			COUNT(T.STATUS),
			2
	) AS `Cancellation Rate`
FROM trips AS T
WHERE 
T.Client_Id NOT IN (
	SELECT users_id
	FROM users
	WHERE banned = 'Yes'
)
AND
T.Driver_Id NOT IN (
	SELECT users_id
	FROM users
	WHERE banned = 'Yes'
)
AND T.request_at BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY T.request_at
```

---
## 分类统计  
表1  

|  	year	  |  	month	  |  	price	  |  
| :---         |     :---:      |     :---:      |  
|  	2019	  |  	1	  |  	50	  |  
|  	2019	  |  	2	  |  	30	  |  
|  	2019	  |  	3	  |  	40	  |  
|  	2019	  |  	4	  |  	70	  |  
|  	2020	  |  	1	  |  	35	  |  
|  	2020	  |  	2	  |  	35	  |  
|  	2020	  |  	3	  |  	44	  |  
|  	2020	  |  	4	  |  	67	  |  

```sql
/* Insert 语句 */
INSERT INTO ``(`year`, `month`, `price`) VALUES ('2019', '1', 50);
INSERT INTO ``(`year`, `month`, `price`) VALUES ('2019', '2', 30);
INSERT INTO ``(`year`, `month`, `price`) VALUES ('2019', '3', 40);
INSERT INTO ``(`year`, `month`, `price`) VALUES ('2019', '4', 70);
INSERT INTO ``(`year`, `month`, `price`) VALUES ('2020', '1', 35);
INSERT INTO ``(`year`, `month`, `price`) VALUES ('2020', '2', 35);
INSERT INTO ``(`year`, `month`, `price`) VALUES ('2020', '3', 44);
INSERT INTO ``(`year`, `month`, `price`) VALUES ('2020', '4', 67);
INSERT INTO ``(`year`, `month`, `price`) VALUES ('2019', '4', 67);
```
### 分类统计
```sql
SELECT year,
	COUNT( `month` = '1' OR NULL) AS 'm1',
	COUNT( `month` = '2' OR NULL) AS 'm2',
	COUNT( `month` = '3' OR NULL) AS 'm3',
	COUNT( `month` = '4' OR NULL) AS 'm4',
price
FROM `表1` GROUP BY `year`
```
结果    

|  	year	  |  	m1	  |  	m2	  |  	m3	  |  	m4	  |    	price   |    
| :---         |     :---:      |     :---:      |      :---:      |      :---:    |      :---:    |  
|  	2019	  |  	1	  |  	1	  |  	1	  |  	2	  |    	50     |  
|  	2020	  |  	1	  |  	1	  |  	1	  |  	1	  |    	35     |  


##### 分类求和统计
```sql
/* 涉及行列如何互换 */
SELECT year,
    sum(case `month` when '1' then `price` else 0 end) as 'm1',
    sum(case `month` when '2' then `price` else 0 end) as  'm2',
    sum(case `month` when '3' then `price` else 0 end) as 'm3',
sum(price) as price_sum
FROM `表1` GROUP BY `year`;
```
结果   

|  	year |  m1	  |  m2  | 	m3  | price_sum  |   
| :---   |     :---:      |     :---:      |      :---:      |     :---:      |  
|  	2019 | 50 | 30 | 40  | 257  |   
|  	2020 | 35 | 35 | 44 |   181  |   