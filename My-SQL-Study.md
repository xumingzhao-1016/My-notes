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
### WHERE = Condition
- **=** - Equal
- **>** - Greater than
- **<** - Less than
- **>=** - greater than or equal
- **<=** - less than or equal
- **<> or !=** - not equal
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

---

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

## Group by
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

## CASE
```sql
CASE
    WHEN condition 1 THEN result 1
    WHEN condition 2 THEN result 2
    WHEN condition N THEN result N
    ELSE result
END (AS Groupname);
### if no condition is true in a CASE expression and there is no ELSE clause
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
