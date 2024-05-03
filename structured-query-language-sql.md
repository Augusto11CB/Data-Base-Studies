# Structured Query Language (SQL)

### `COALESCE`

#### What is `COALESCE` in SQL?

`COALESCE` is a function in SQL that returns the first non-null value in a list of arguments. It is often used to substitute a default value for any null values within a table column during select queries. The syntax for using `COALESCE` is as follows:

```sql
COALESCE(expression1, expression2, ..., expressionN)
```

* **expressions**: These are the values to be tested for non-null in the order they are listed.

If all the expressions evaluate to null, `COALESCE` will return null. This function is very handy for dealing with optional data in SQL databases, ensuring that a fallback value can be provided in place of nulls when presenting or using the data.

#### Example

Table: `Customer`

Fonte: [https://leetcode.com/problems/find-customer-referee/](https://leetcode.com/problems/find-customer-referee/)

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+


Customer table:
+----+------+------------+
| id | name | referee_id |
+----+------+------------+
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |
+----+------+------------+

```

```sql

select * from customer where  COALESCE(referee_id, 0) <> 2
-- toda coluna referee_id com valor null, será tratada como se tivesse o valor ZERO.


Output:

| id | name | referee_id |
| -- | ---- | ---------- |
| 1  | Will | null       |
| 2  | Jane | null       |
| 4  | Bill | null       |
| 5  | Zack | 1          |
```



### `UNION` works as `OR`

#### What is UNION in SQL?

The `UNION` operator in SQL is used to combine the result sets of two or more `SELECT` statements. It eliminates duplicate rows between the combined datasets. Each `SELECT` statement within the `UNION` must have the same number of columns, and the columns must have similar data types. The columns in each `SELECT` statement must also be in the same order.

**Usage:**

```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

**Key Points:**

* Removes duplicates (use `UNION ALL` to include duplicates).
* Each `SELECT` statement within the `UNION` must have the **same number of columns.**
* The columns must also have similar data types.
* The columns in each SELECT statement must be in the same order.

#### Example

Table: `World`

Font: [https://leetcode.com/problems/big-countries/](https://leetcode.com/problems/big-countries/description)

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |
+-------------+---------+
name is the primary key (column with unique values) for this table.
Each row of this table gives information about the name of a country, the continent to which it belongs, its area, the population, and its GDP value.


World table:
+-------------+-----------+---------+------------+--------------+
| name        | continent | area    | population | gdp          |
+-------------+-----------+---------+------------+--------------+
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |
| Andorra     | Europe    | 468     | 78115      | 3712000000   |
| Angola      | Africa    | 1246700 | 20609294   | 100990000000 |
+-------------+-----------+---------+------------+--------------+
```



```sql
-- UNION

SELECT name, population, area
FROM World
WHERE area >= 3000000 

UNION

SELECT name, population, area
FROM World
WHERE population >= 25000000

-- OR 

SELECT name,
       population,
       area
FROM   world
WHERE  area >= 3000000 OR population >= 25000000

```



### `SELECT DISTINCT`

The `SELECT DISTINCT` statement retrieves distinct values from a database table \[1].

The syntax for using `SELECT DISTINCT` is as follows \[1]:

```
SELECT DISTINCT column1, column2 ...
FROM table;
```



### `LEFT JOIN`

The SQL `LEFT JOIN` operation merges two tables by matching rows based on a shared column. It includes all matching records from both tables and also **those rows from the left table that don't have a match in the right table \[2]**.



#### Example

Table: `Employees`

Font: [https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table contains the id and the name of an employee in a company.


Employees table:
+----+----------+
| id | name     |
+----+----------+
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
+----+----------+
```



Table: `EmployeeUNI`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
(id, unique_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id and the corresponding unique id of an employee in the company.

EmployeeUNI table:
+----+-----------+
| id | unique_id |
+----+-----------+
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
+----+-----------+
```



The LEFT JOIN is used; this will return ALL rows from Employees, regardless of whether or not there is a matching row in EmployeeUNI.

```pgsql
SELECT eUni.unique_id,e.name
FROM Employees e
LEFT JOIN EmployeeUNI eUni ON e.id = eUni.id

-- Output

| unique_id | name     |
| --------- | -------- |
| null      | Alice    |
| 1         | Jonathan |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
```





### `DATEDIFF`

a



### `LAG and LEAD`

a



### `DATE_ADD`

a



### `Common Table Expression (CTE) (WITH Clause)`

a



### References

\[1] [https://www.programiz.com/sql/select-distinct](https://www.programiz.com/sql/select-distinct)

\[2] [https://www.programiz.com/sql/left-join](https://www.programiz.com/sql/left-join)
