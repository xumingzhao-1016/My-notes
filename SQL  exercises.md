# 执行顺序
## FROM - WHERE - GROUP BY - HAVING - SELECT - ORDER BY- LIMIT
Table: Delivery

+-----------------------------+---------+
| Column Name                 | Type    |
+-----------------------------+---------+
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
+-----------------------------+---------+
delivery_id is the column of unique values of this table.
The table holds information about food delivery to customers that make orders at some date and specify a preferred delivery date (on the same order date or after it).
 

If the customer's preferred delivery date is the same as the order date, then the order is called immediate; otherwise, it is called scheduled.

The first order of a customer is the order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.

Write a solution to find the **percentage of immediate orders in the first orders of all customers（在第一笔订单中immediate的比例）**, rounded to 2 decimal places.

解法一：
```sql
SELECT ROUND(
    SUM(CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE 0 END) * 100.0 
    / COUNT(*), 2
) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date) IN (
    SELECT customer_id, MIN(order_date)
    FROM Delivery
    GROUP BY customer_id -- WHERE + 子查询不用给表起别名
);
```
解法二（窗口函数）：
```sql
SELECT ROUND(
    SUM(CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE 0 END) * 100.0 
    / COUNT(*), 2
) AS immediate_percentage
FROM (SELECT (*),
                ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date) AS rn
      FROM Delivery
) AS sub -- FROM + 子查询要起别名
WHERE rn = 1

```

---
# CASE WHEN 

## 配对计算值
Table: Activity

+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
The table shows the user activities for a factory website.
(machine_id, process_id, activity_type) is the primary key (combination of columns with unique values) of this table.
machine_id is the ID of a machine.
process_id is the ID of a process running on the machine with ID machine_id.
activity_type is an ENUM (category) of type ('start', 'end').
timestamp is a float representing the current time in seconds.
'start' means the machine starts the process at the given timestamp and 'end' means the machine ends the process at the given timestamp.
The 'start' timestamp will always be before the 'end' timestamp for every (machine_id, process_id) pair.
It is guaranteed that each (machine_id, process_id) pair has a 'start' and 'end' timestamp.
 

There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.

The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.

The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.

Return the result table in any order.
``` sql
SELECT machine_id, ROUND(AVG(start_time-end_time,3) AS processing_time
FROM（
      SELECT machine_id,
             process_id,
             MAX(CASE WHEN activity_type = 'start', then timestamp END) AS start_time
             MAX(CASE WHEN activity_type = 'end', then timestamp END) AS end_time -- start & end 分别提取出来
       FROM Activity
       GROUP BY machine_id, process_id
) AS sub
GROUP BY machine_id
```
---
## 按条件统计
Table: Queries

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| query_name  | varchar |
| result      | varchar |
| position    | int     |
| rating      | int     |
+-------------+---------+
This table may have duplicate rows.
This table contains information collected from some queries on a database.
The position column has a value from 1 to 500.
The rating column has a value from 1 to 5. Query with rating less than 3 is a poor query.
 

We define query quality as:

The average of the ratio between query rating and its position.

We also define poor query percentage as:

The percentage of all queries with rating less than 3.

Write a solution to find each query_name, the quality and poor_query_percentage.

Both quality and poor_query_percentage should be rounded to 2 decimal places.

Return the result table in any order.

```sql
SELECT query_name,
       ROUND(AVG(rating/position),2) AS quality
       ROUND(SUM(CASE WHEN rating < 3 THEN 1 ELSE O END) *100.0/COUNT(*),2) AS poor_query_percentage
-- 按条件筛选出来进行统计
FROM Queries
GROUP BY query_name
```

## 标签分类输出
表结构：Orders

order_id	customer_id	total_amount
1	101	200
2	102	80
3	103	350
4	104	20
5	105	120

❓问题：
请写一个 SQL 查询，输出每个订单的：

order_id

customer_id

total_amount

order_level：

如果金额 ≥ 300，显示为 'High'

如果金额 ≥ 100 且 < 300，显示为 'Medium'

否则为 'Low'

```sql
SELECT 
    order_id,
    customer_id,
    total_amount,
    CASE 
        WHEN total_amount >= 300 THEN 'High'
        WHEN total_amount >= 100 AND total_amount < 300 THEN 'Medium'
        ELSE 'Low'
    END AS order_level
FROM Orders;

```
## 根据客户类型排序
表结构：Customers

customer_id	name	level
1	Alice	Gold
2	Bob	Silver
3	Charlie	Bronze
4	Diana	Gold
5	Eva	Silver

❓问题：
请写一个 SQL 查询，输出所有客户的：

customer_id

name

level

并按照以下规则进行排序：

Gold 最优先

Silver 其次

Bronze 最后
```sql
SELECT customer_id,
       name,
       level
FROM Customers
ORDER BY
       CASE
           WHEN level = 'Gold' THEN 1
           WHEN level = 'Silver' THEN 2
           WHEN level = 'Bronze' THEN 3
           END

```
## 聚合求多条件指标
表结构：Sales

sale_id	product	amount
1	Apple	100
2	Banana	50
3	Apple	200
4	Orange	150
5	Banana	30
6	Orange	120
7	Apple	80

❓问题：
请写一个 SQL 查询，输出一行结果，包含：

apple_sales: 所有 Apple 的总销售额

banana_sales: 所有 Banana 的总销售额

orange_sales: 所有 Orange 的总销售额
``` sql
SELECT SUM(CASE WHEN PRODUCT = 'Apple' THEN amount ELSE 0 END) AS apple_sales,
       SUM(CASE WHEN PRODUCT = 'Banana' THEN amount ELSE 0 END) AS banana_sales,
       SUM(CASE WHEN PRODUCT = 'Orange' THEN amount ELSE 0 END) AS orange_sales
FROM Sales
```

## 分组后输出等级标签
表结构：CustomerOrders

customer_id	order_amount
1	300
1	200
2	50
2	100
3	800
4	120
4	130

❓问题：
请写一个 SQL 查询，计算每个客户的总订单金额，并根据总金额打等级标签 level：

High：总金额 ≥ 500

Medium：总金额 ≥ 200 且 < 500

Low：总金额 < 200

```sql
SELECT customer_id,
       SUM(order_amount) AS total_amount,
       CASE
          WHEN SUM(order_amount)>=500 THEN 'High'
          WHEN SUM(order_amount) >= 200 AND SUM(order_amount) <500 THEN 'Medium'
          ELSE 'Low'
          END AS level --case when不用加括号，直接END AS 别名就行
FROM CustomerOrders
GROUP BY customer_id
```
## 数据清洗（替换NULL值）
表结构：Employees

emp_id	name	department	bonus
1	Alice	HR	1000
2	Bob	Marketing	NULL
3	Charlie	HR	800
4	Diana	Finance	NULL
5	Eva	Marketing	500

❓问题：
请写一个 SQL 查询，返回每位员工的：

emp_id

name

department

bonus（原始值）

clean_bonus：如果 bonus 是 NULL，则替换为 0；否则保留原值。

```sql
SELECT emp_id, name, department, bonus,
       CASE
           WHEN bonus is NULL THEN 0
           ELSE bonus
           END AS clean_bonus
FROM Employees

```
## CASE WHEN + 窗口函数
表结构：Employees

emp_id	name	department	bonus
1	Alice	HR	1000
2	Bob	Marketing	1500
3	Charlie	HR	800
4	Diana	Finance	1800
5	Eva	Marketing	2000
6	Frank	Finance	1000

❓目标：
查询所有员工，并额外生成一个字段 is_top_bonus_in_dept，规则如下：

如果该员工是本部门中奖金最高的，显示为 'Yes'

否则为 'No'
```sql
SELECT emp_id,
    name,
    department,
    bonus, --最好不要写（*）
       CASE WHEN bonus = MAX(bonus) OVER(PARTITION BY department ORDER BY bonus) THEN 'Yes'
            ELSE 'No' -- case when 需要一个条件判断式
            END AS is_top_bonus_in_dept
FROM Employees
```


---
# SELF JOIN
## 时间序列比对
Table: Weather

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id is the column with unique values for this table.
There are no different rows with the same recordDate.
This table contains information about the temperature on a certain day.
 

Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).

Return the result table in any order.

```sql
SELECT w1.id
FROM Weather w1
JOIN Weather w2
ON DATEDIFF(w1.recordDate,w2.recordDate)=1 -- w1表就是比w2表早一天
WHERE w1.temperature > w2.temperature
```
---
## 父子关系结构
Table: Employee

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the name of an employee, their department, and the id of their manager.
If managerId is null, then the employee does not have a manager.
No employee will be the manager of themself.
 

Write a solution to find managers with at least five direct reports.

Return the result table in any order.

```sql
SELECT e1.name
FROM Employee e1
JOIN Employee_e2
ON e1.id = e2.managerId
GROUP BY e2.managerId
HAVING COUNT(e1.name)>=5
```
---
## 同组配对/同组比较
找出在同一部门中工资比任何同部门员工都高的员工名字
表名：Employee
列名	类型	描述
id	int	主键，唯一员工编号
name	varchar	员工名字
department	varchar	员工所在部门
salary	int	员工工资
```sql
SELECT e1.name
FROM Employee e1
JOIN Employee e2
ON e1.department = e2.department 
WHERE e1.salary > e2.salary
```
---

## 数据差异查找/去重辅助
找出那些 名字相同但手机号不同 的客户的名字（name）

表名：Customer
列名	类型	含义
id	int	主键
name	varchar	客户名字
phone	varchar	客户手机号
address	varchar	客户地址

```sql
SELECCT c1.name
FROM Customer c1
JOIN Customer c2
ON c1.name = c2.name AND c1.phone <> c2.phone
```
---

## 双向匹配
给定一张 Friendship 表，记录用户之间的“好友请求”（单向），请找出所有互为好友的用户配对。

表名：Friendship
列名	类型	含义
user_id	int	发起好友请求的用户 ID
friend_id	int	被添加为好友的用户 ID
```sql
SELECT f1.user_id, f1.friend_id
FROM Friendship f1
JOIN Friendship f2
ON f1.user_id = f2.friend_id AND f1.friend_id = f2.user_id
WHERE f1.user_id < f1.friend_id -- 去重，只保留一对中的一条
```
---
# 子查询
## 根据公式需要分别建立子表
Table: Activity

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+
(player_id, event_date) is the primary key (combination of columns with unique values) of this table.
This table shows the activity of players of some games.
Each row is a record of a player who logged in and played a number of games (possibly 0) before logging out on someday using some device.
 

Write a solution to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.

### 解法一： SELF JOIN
```SQL
SELECT ROUND(COUNT(a.count_again)*1.0/COUNT(b.total),2) AS fraction
FROM(SELECT COUNT(DISTINCT a1.player_id) AS count_again
     FROM Activity a1
     JOIN (SELECT player_id, MIN(event_date) AS min_date
           FROM Activity
           GROUP BY player_id) AS a2
      ON a1.player_id = a2.player_id AND DATEDIFF(a1.event_date, a2.event_date)=1 --self join在第一次登录的基础上的登录，自连接筛出来
) AS a, --分母
(SELECT COUNT(DISTINCT player_id) AS total
 FROM Activity
) AS b -- 分子

```
### 解法二：窗口函数
```sql
WITH ranked AS (SELECT player_id, event_date,
                       ROW_NUMBER() OVER(PARTITION BY player_id ORDER BY event date) AS rn
                FROM Activity
),
     first_login AS (SELECT player_id, event_date AS first_day
                     FROM ranked
                     Where rn = 1)
     second_login AS (SELECT player_id, event_date AS second_day
                      FROM ranked
                      Where rn = 2)
-- 将第一天登录和连续两天登录的数据拿出来各成一个子查询表
SELECT ROUND(
             COUNT(DISTINCT s.player_id)*1.0/(SELECT COUNT(DISTINCT player_id)
                                              FROM Activity),2
) AS fraction
FROM second_login s
JOIN first_login f
ON s.player_id = f.player_id
AND DATEDIFF(s.event_date,f.event_date) = 1
```
---
