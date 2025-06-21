# 逻辑流程图
**FROM        → 连接/抽取原始数据  
  ↓
WHERE       → 行筛选（原始字段）  
  ↓
GROUP BY    → 分组准备聚合  
  ↓
HAVING      → 分组后筛选（支持聚合函数）  
  ↓
WINDOW      → 定义窗口别名（可选）  
  ↓
SELECT      → 列选择、计算、窗口函数应用  
  ↓
DISTINCT    → 去重（如果写了）  
  ↓
ORDER BY    → 排序输出结果  
  ↓
LIMIT       → 控制返回的行数**

# 📘 Most Important SQL Commands

## 🗃️ 常用语句说明

- **SELECT** — extracts data from a database  
- **UPDATE** — updates data in a database  
- **DELETE** — deletes data from a database  
- **INSERT INTO** — inserts new data into a database  
- **CREATE DATABASE** — creates a new database  
- **ALTER DATABASE** — modifies a database  
- **CREATE TABLE** — creates a new table  
- **ALTER TABLE** — modifies a table  
- **DROP TABLE** — deletes a table  
- **CREATE INDEX** — creates an index (search key)  
- **DROP INDEX** — deletes an index  

---

# COALESCE: 返回参数中第一个非null的值
```sql
COALESCE(value1, value2, value3...,default_val)
### 返回从左到右第一个不是NULL的值。如果所有值都是null，就返回最后的default_val
ex.
SELECT COALESCE(NULL,NULL,'HELLO')
-- 'HELLO'
```
---
# CAST()强制转换数据类型
```sql
CAST(expression AS target_type)
```
| 类型                         | 用途         |
| -------------------------- | ---------- |
| `SIGNED`                   | 整数         |
| `UNSIGNED`                 | 非负整数       |
| `CHAR`                     | 字符串        |
| `DECIMAL(p,s)`             | 定点小数（精度控制） |
| `DATE`, `DATETIME`, `TIME` | 时间类型       |

---

## SQL Comments
- **--** - single line comments
- **start with/*,end with */**  - Multi-line comments
---
## BACKUP DTABASE
备份数据库（完整备份）
```sql
BACKUP DATABASE My Database
TO DISK = 'D:\Backup\MyDatabase_diff.bak'
WITH DIFFERENTIAL
###差异备份

###remane
ALTER TABLE table_name
RENAME COLUMN old_name to new_name
(MySQL)

EXEC sp_rename'table_name.old_name', 'new_name', 'COLUMN'
```
---

### Create Constraints
```sql
CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    column3 datatype constraint,
    ....
);

```
## COMMON CONSTRAINTS
- **NOT NULL**
- **UNIQUE** - all values in a column are different
- **PRIMARY KEY** - A Combination of a NOT NULL & UNIQUE
- **FOREIGN KEY** - Prevents actions that would destroy links between tables
- **CHECK**- Ensures that the values in a column satisfies a specific condition
- **DEFAULT** - sets a default value for a column if no value is specifies
- **CREATE INDEX** - Used to creare and retrive data from the database very quickly
---

## ALTER TABLE
add, delete or modify columns in an existing table
```sql
ALTER TABLE table_name
ADD colunm_name datatype
```
### ALTER/MODIFY - change the data type 
---

## Wildcard
- **%** - represents zero, one, or multiple characters
- **_** - represents one, single character
  - **'s%'** - 所有以s开头的字符串
  - **'%n'** - 所有以n结尾的字符串
  - **'%or%'** - 所有包含or的字符串
  - **'_a'** - 所有第三个字母是a且总共3个字符的字符串
  - **IN** - To specify multiple possible values for a column

- **[]** - any single character within the brackets
### WHERE CustomerName LIKE [bsp]%- all customers starting with either "b","p", or "s"

- **^** - any character not in the brachets
- **-** -any single character within the specific range
- **{}** - any escaped character
- * - zero or more characters - bl* = bl black, blue
- **?** - a single character - h?t = hot, hat and hit
- **[]** - any single character within the brackets - h[oa]t = hot, hat but not hit
- **!** - any character not in the brackets - h[!oa]t = hit
- **-** - any single character within the specified range - c[a-b]t = cat and cbt
- **#** - any single numeric character - 2#5 = 205,215,225,235,245.....

## 🎯 SELECT 示例

```sql
SELECT * FROM table_name；
### * - all culumns
SELECT DISTINCT column1,column2 FROM table_name;
### Inside a table, a column often contains many duplicate values, sometimes only want to list the different(distinct) values
SELECT COUNT(DISTINCT Country) FROM Customers;
### get the number of different countries
SELECT MIN(Price) AS SmallestPrice
###命名
```
---

## SELECT INTO
```sql
SELECT * INTO newtable
FROM oldtable
WHERE condition
###copies data from one table into a new table

```
---

## INSERT INTO SELECT
```SQL
INSERT INTO table2
SELECT * FROM table1
WHERE condition
###data types in source and target tables match
```
---

## WHERE

```sql
SELECT * FROM Customers
WHERE Country='Mexico'
```
## OPERATORS
- **=** - Equal
- **>** - Greater than
- **<** - Less than
- **>=** - greater than or equal
- **<=** - less than or equal
- **<> or !=** - not equal
- 
**MOD()**
- MOD(X,2)=0 判断偶数
- MOD(X,2)=1 判断奇数
- MOD(row_number,N) 每N行为一组
- CASE WHEN MOD(id,2) = 0 隔行标记


  
- **BETWEEN** - Between a certain range
- **LIKE** - Search for a pattern （模糊匹配）

### SELECT * FROM Customers WHERE Country IN ('France', 'Germany', 'Italy') = WHERE Country = "France" OR Country = 'Germany' OR Country = 'Italy'

---

## ORDER BY

```sql
SELECT * FROM Products
ORDER BY Price
### ORDER BY is used to sort the result-set in ascending or descending order
ORDER BY Country, CustomerName
###按国家排，国家相同的再按客户名排
ORDER BY Country ASC, Customer Name DESC
###先按国家升序排，国家相同的再按客户名降序排
```
- **ASC** - 从小到大
- **DESC** - 从大到小
- **default sorting order** - Ascending

**ORDER BY 是一个优先级排序规则列表，依次判断：
先比第一个字段（最主要排序标准）；
如果相同，再比第二个字段；
如果还相同，再比第三个字段（依此类推）**

---
## ROUND 四舍五入
**ROUND(expression,n)** - expression数学表达式，不能放一整个子查询进去
```sql
CORRECT:
SELECT ROUND(100*(SELECT COUNT(*) FROM Queries WHERE rating<3)/SELECT COUNT(*) FROM Queries, 2) AS poor_query_percentage

FAUX:
SELECT
      ROUND(SELECT COUNT(rating)<100/COUNT(rating) FROM Queries WHERE rating<3,2)
```
## FLOOR()向下取整（不四舍五入）

## AND OR

- **AND** - all the conditions are TRUE
- **OR** - any of the conditions are TRUE
---
  
## Not
- Not放在表达式之前
---

## INSERT INTO
```SQL
INSERT INTO table_name(column1, column2, column3,...)
VALUES (value1, value2, value3,...), (value1, value2, value3,...);
```
---

## Null value

- we have to use **IS NULL** OR **IS NOT NULL**

---

## UPDATE
  ```SQL
  UPDATE table_name
  SET column1 = value1, column2 = value2, ...
  WHERE condition
  
  UPDATE Customers
  SET ContactName = 'Henry', City = 'Paris'
  WHERE CustomerID = 1
  ###讲客户编号为1的联系人名字改成Henry，城市改成巴黎
  ```
  ---

## DELETE
  ```SQL
  DELETE FROM table_name WHERE condition

  DROP TABLE Customers
  ### delete the table completely
  ```
  ---

## SELECT TOP, LIMIT
  ```SQL
  SELECT TOP 3 * FROM Customers
  
  SELECT * FROM Customers
  LIMIT 3

  SELECT * FROM Customers
  FETCH FIRST 3 ROWS ONLY
  ### select only the first 3 records of the customers table
  ```
---

## aggregate functions

  - **MIN()** - returns the smallest value within the selected column
  - **MAX()**- returns the largest value within the selected colum
  - **COUNT()**- returns the number of rows in a set - NULL values will not be counted
  - **SUM()** - returns the total sum of a numerical column
  - **AVG()** - returns the average value of a numerival column

---

## LENGTH()/CHAR_LENGTH() vs COUNT()
- **LENGTH** - 返回字节数（英文字符占1个字节，中文可能占2个或3个）
- **CHAR_LENGTH** -返回字符数（不论中英都算一个字符）

- **COUNT()** - 统计行数
---

## JOIN
```sql
(INNER) JOIN
### returns records that have matching values in both tables

LEFT (OUTER) JOIN
### returns all records from the left table, and the matched records from the right table

RIGHT (OUTER) jOIN
### returns all records from right table, and the matched records from the left table

SELECT ...
FROM A
LEFT/RIGHT JOIN B ON (combination)
### A is left table, B is right table

FULL ( OUTER) JOIN
### returns all records when there is a match in either left or right table

### Insert the missing parts in the JOIN clause to join the two tables Orders and Customers, using the CustomerID field in both tables as the relationship between the two tables.
SELECT *
FROM Orders
LEFT JOIN Customers
ON Orders.CustomerID = Customers.Customer ID
### 元素相等要指出相对应的表才能链接两个表

CROSS JOIN --两个表格的内容互相匹配

JOIN ON --连接两个表的主键=外键 & 加入附加的筛选条件
ex.
 LEFT JOIN UnitsSold u
   ON p.product_id = u.product_id
   AND u.purchase_date BETWEEN p.start_date AND p.end_dat

```
---

## Union
```sql
SELECT column_name FROM table1
UNION
SELECT column_name FROM table2
returns: one column name
### every SELECT statement within UNION must have the same number of columns
### the column must have similar data types
### the columns in every SELECT statement must also be in the same order
### UNION - eliminate duplicate value automatically

UNION ALL - allow duplicate values

```
---

## Group by - 必须对非GROUP BY的字段做聚合处理
```sql
SELECT 分组字段，聚合函数
FROM table name
GROUP BY 分组字段
### 要以某一个类别看数据必须用group by， 比如在一张表中看每个产品的销量
```
---

## HAVING
```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);
### having筛选分组聚合后的结果，where不能使用聚合函数
```
---

## EXISTS
- used to test for the existence of any record in a subquery
- 用在 WHERE中
- Returns TRUE or FALSE

## ANY/ALL
```SQL
SELECT ProductName
FROM Products
WHERE ProductID = ANY/ALL
  (SELECT ProdyctID
   FROM OrderDetails
WHERE Quantity = 10
### ANY & All前可以跟运算符号
```
---

## CASE - 在聚合函数中必须嵌套函数
### 筛选条件
```sql
CASE
    WHEN condition 1 THEN result 1
    WHEN condition 2 THEN result 2
    WHEN condition N THEN result N
    ELSE result
END (AS Groupname);
### if no condition is true in a CASE expression and there is no ELSE clause
```
### 增加查询新列,仅限此次查询，不会改变表格结构
Table: Triangle

+-------------+------+
| Column Name | Type |
+-------------+------+
| x           | int  |
| y           | int  |
| z           | int  |
+-------------+------+
In SQL, (x, y, z) is the primary key column for this table.
Each row of this table contains the lengths of three line segments.
 

Report for every three line segments whether they can form a triangle.

Return the result table in any order.

The result format is in the following example.

```sql
SELECT x,y,z,
      CASE
          WHEN x+y>z and x+z>y and y+z>x THEN 'Yes'
          ELSE 'No'
          END AS triangle
FROM Triangle
```
---

## CREATE PROCEDURE
If you have an SQL query that you write over and over again, save it as a stored procedure, nd then just call it to execute it.
```sql
CREATE PROCEDURE SelectAllCustomers @City nvarchar(30)
AS
SELECT * FROM Customers WHERE City = @City
GO

EXEC Select AllCustomers @City = 'London'

### @City nvarchar(30) - 定义一个输入参数，名字叫@City， 是长度最多30 的字符串
```
---
# 时间
TIMEFIFF VS DATEDIFF
- **TIMESTAMPDIFF(单位，strat, end)**
- **TIMEDIFF(hh:mm:ss)**
- **DATEDIFF(YYYY-MM-DD, YYYY-MM-DD)** - 返回整天数
- **DATE_SUB & INTERVAL**日期减去过去的一段时间（天月日）
  WHERE order_date BETWEEN DATE_SUB('2025_05-11', INTERVAL 30 DAY) AND '2025-05-11'
OR WHERE order_date>=DATE_SUB('2025-05-11', INTERVAL 30 DAY)
   AND order_date<''2025-05-12
- **MONTH('date')=** - 指明月份
- **YEAR ('date'）=** - 指明年份
- **DATE_ADD(date, INTERVAL n DAY)** - 给指定日期加上n天
- **DATE_SUB('DATE', INTERVAL n DAY)** -给指定日期减去n天

## DATE_FORMAT(date,format_string)
| 格式符  | 含义         | 示例       |
| ---- | ---------- | -------- |
| `%Y` | 年（四位）      | `2025`   |
| `%y` | 年（两位）      | `25`     |
| `%m` | 月（两位数字）    | `05`     |
| `%c` | 月（不补零）     | `5`      |
| `%M` | 月（英文全称）    | `May`    |
| `%b` | 月（英文简写）    | `May`    |
| `%d` | 日（两位数字）    | `19`     |
| `%e` | 日（不补零）     | `19`     |
| `%W` | 星期（英文全称）   | `Monday` |
| `%w` | 星期（数字0=周日） | `1`（周一）  |
| `%H` | 小时（24小时制）  | `14`     |
| `%h` | 小时（12小时制）  | `02`     |
| `%i` | 分钟         | `30`     |
| `%s` | 秒          | `45`     |

---
# 窗口函数
## common functions
```sql
Aggregation function () OVER (
        PARTITION BY XXX
        ORDER BY XXX
)
```
- **ROW_NUMBER()** - 给每一行分配一个唯一编号，从1开始
- **RANK()** - 给每一行排名，有并列名次，会跳过数字
- **DENSE_RANK()** - 排名，但不会跳过名次
- **NTILE(n)** - 把数据平均分成n分（分位数），每行分配到第几组
- **LAG() & LEAD()** - 访问同组中上一行（LAG）或下一行（LEAD）的值
- **WINDOW alias** -定义窗口
  Before:
  SELECT start_terminal,
       duration_seconds,
       NTILE(4) OVER **(PARTITION BY start_terminal ORDER BY duration_seconds)** AS quartile,
       NTILE(5) OVER (PARTITION BY start_terminal ORDER BY duration_seconds) AS quintile,
       NTILE(100) OVER (PARTITION BY start_terminal ORDER BY duration_seconds) AS percentile
FROM tutorial.dc_bikeshare_q1_2012
WHERE start_time < '2012-01-08'
ORDER BY start_terminal, duration_seconds;

After:
SELECT start_terminal,
       duration_seconds,
       NTILE(4) OVER ntile_window AS quartile,
       NTILE(5) OVER ntile_window AS quintile,
       NTILE(100) OVER ntile_window AS percentile
FROM tutorial.dc_bikeshare_q1_2012
WHERE start_time < '2012-01-08'
**WINDOW ntile_window AS (
    PARTITION BY start_terminal
    ORDER BY duration_seconds
)**
ORDER BY start_terminal, duration_seconds;

## Advanced functions 写在over()里
- **ROWS BETWEEN 2 PRECEDING AND CURRENT ROW** - 从当前行往上数2行，再加上当前行本身，总共最多3行，做聚合
- **ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW** - 当前组从最开始到当前行
- **RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW** - 当前值及更早的所在值（基于排序值）
- **RANGE BETWEEN INTERVAL 6 DAY AND CURRENT ROW** - 要是前面ORDER BY 后面跟的是时间形式DATETIME，必须用interval
- **FIRST_VALUE()/LAST_VALUE** - 当前窗口的第一个值/最后一个值
- **NTH_VALUE** - 取窗口中的第n个值

SELECT 
  visited_on,
  SUM(amount) OVER customer_window AS amount,
  ROUND(AVG(amount) OVER customer_window,2) AS average_amount
FROM Customer
WHERE visited_on >= DATE_ADD(
    (SELECT MIN(visited_on) FROM Customer), 
    INTERVAL 6 DAY)
WINDOW customer_window AS (
    ORDER BY visited_on
    RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW)
ORDER BY visited_on ASC
