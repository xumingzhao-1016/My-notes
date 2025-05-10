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
