# Structured Query Language (SQL)

### `AVG` and `ROUND`

aggregation&#x20;

#### Example

Question: **Write a solution to find the average time each machine takes to complete a process.**

Font: [https://leetcode.com/problems/average-time-of-process-per-machine](https://leetcode.com/problems/average-time-of-process-per-machine)

```
Table: Activity

+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+ 
 
There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.

The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.

The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.


Activity table:
+------------+------------+---------------+-----------+
| machine_id | process_id | activity_type | timestamp |
+------------+------------+---------------+-----------+
| 0          | 0          | start         | 0.712     |
| 0          | 0          | end           | 1.520     |
| 0          | 1          | start         | 3.140     |
| 0          | 1          | end           | 4.120     |
| 1          | 0          | start         | 0.550     |
| 1          | 0          | end           | 1.550     |
| 1          | 1          | start         | 0.430     |
| 1          | 1          | end           | 1.420     |
| 2          | 0          | start         | 4.100     |
| 2          | 0          | end           | 4.512     |
| 2          | 1          | start         | 2.500     |
| 2          | 1          | end           | 5.000     |
+------------+------------+---------------+-----------+
```

In this approach, the original table is referenced twice, once as the table that stores the start timestamps and once as the table that stores the end timestamps. To create the table alias, the original table Activity is given two different names and filtered by the activity\_type.&#x20;

Furthermore, the two tables are joined on the machine\_id and process\_id fields, ensuring that the output contains the start and end timestamps for each machine and process. This allows us to update the calculation for processing\_time by subtracting all the timestamps in table a (start timestamps) from the end timestamps in table b.&#x20;

In order to calculate the average processing time at the machine ID level, the processing time calculation is augmented with the AVG() function and rounded to three decimal places using the ROUND() function.

```sql
SELECT A.machine_id, ROUND(AVG(B.timestamp - A.timestamp)::numeric, 3) as processing_time
FROM Activity A, Activity B
WHERE A.machine_id = B.machine_id
AND A.activity_type = 'start' 
AND B.activity_type = 'end'
GROUP BY A.machine_id;
```

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

```sql
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

```sql
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

```sql
SELECT DISTINCT column1, column2 ...
FROM table;
```



### `DISTINCT`

The COUNT(DISTINCT column\_name) function in SQL is used to count the number of distinct (unique) values in a specified column.



#### Example

font: [https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/)

```sql
SELECT t.teacher_id, COUNT(DISTINCT t.subject_id)
FROM Teacher t
GROUP BY t.teacher_id
ORDER BY t.teacher_id ASC;

// This query will return each teacher_id along with the count of distinct subject_id associated with that teacher_id
```



### `LEFT JOIN`

The SQL `LEFT JOIN` operation merges two tables by matching rows based on a shared column. It includes all matching records from both tables and also **those rows from the left table that don't have a match in the right table \[2]**.



#### Example

Table: `Employees`

Font: [https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)

```sql
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

```sql
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

```sql
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



### `CASE WHEN`

* `CASE WHEN` is a way to introduce if-else like conditions into your SQL queries.



### `Common Table Expression (CTE) (WITH Clause)`



#### Example 1

**Question:** Write a solution to find the **average time** each machine takes to complete a process.

Font: [https://leetcode.com/problems/average-time-of-process-per-machine/description/](https://leetcode.com/problems/average-time-of-process-per-machine/description/)

```sql
Table: Activity

+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+ 
 
There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.

The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.

The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.


Activity table:
+------------+------------+---------------+-----------+
| machine_id | process_id | activity_type | timestamp |
+------------+------------+---------------+-----------+
| 0          | 0          | start         | 0.712     |
| 0          | 0          | end           | 1.520     |
| 0          | 1          | start         | 3.140     |
| 0          | 1          | end           | 4.120     |
| 1          | 0          | start         | 0.550     |
| 1          | 0          | end           | 1.550     |
| 1          | 1          | start         | 0.430     |
| 1          | 1          | end           | 1.420     |
| 2          | 0          | start         | 4.100     |
| 2          | 0          | end           | 4.512     |
| 2          | 1          | start         | 2.500     |
| 2          | 1          | end           | 5.000     |
+------------+------------+---------------+-----------+
```

:bulb: In SQL, you can’t directly store the result of a query in a variable like in other programming languages. However, you can achieve a similar effect by using subqueries or Common Table Expressions (CTEs).

```sql
SELECT 
    machine_id,
    ROUND( (SUM(CASE WHEN activity_type='start' THEN timestamp*-1 ELSE timestamp END) *1.0
    / (SELECT COUNT(DISTINCT process_id)))::numeric ,3) AS processing_time
FROM 
    Activity
GROUP BY machine_id

-- can we store the {CASE WHEN activity_type='start' THEN timestamp*-1 ELSE timestamp END) *1.0} in a variable?
-- can we store the {SELECT COUNT(DISTINCT process_id} in a variable?
-- can we use the variable in the query above instead of the long queries?

-- Yes, with CTE
```

The `activity_time` is a **CTE** that stores the result of the `CASE` statement and `process_count` is another **CTE** that stores the count of distinct `process_id`. These are then used in the main query to calculate `processing_time`

<pre class="language-sql"><code class="lang-sql">

<strong>WITH activity_time AS (
</strong>  SELECT 
    machine_id,
    CASE
      WHEN activity_type='start' THEN timestamp*-1
      ELSE timestamp * 1.0
    END as activity_timestamp,
    process_id
  FROM Activity
),
process_count AS (
  SELECT COUNT(DISTINCT process_id) as distinct_process_count
  FROM Activity
)
SELECT 
  machine_id,
  ROUND( (SUM(activity_timestamp) / (SELECT distinct_process_count FROM process_count))::numeric, 3) AS processing_time
FROM 
  activity_time
GROUP BY machine_id

</code></pre>

### Selecting Same Table Twice In a Single Query

#### Example

Table: `Employee`

Font: [https://leetcode.com/problems/employees-earning-more-than-their-managers/description/](https://leetcode.com/problems/employees-earning-more-than-their-managers/description/)

```sql
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| salary      | int     |
| managerId   | int     |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the ID of an employee, their name, salary, and the ID of their manager.


Employee table:
+----+-------+--------+-----------+
| id | name  | salary | managerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | Null      |
| 4  | Max   | 90000  | Null      |
+----+-------+--------+-----------+

-- Write a solution to find the employees who earn more than their managers.

SELECT EMP.name AS Employee FROM Employee EMP, Employee MGR
WHERE EMP.managerId = MGR.id AND EMP.salary > MGR.salary


```



### References

\[1] [https://www.programiz.com/sql/select-distinct](https://www.programiz.com/sql/select-distinct)

\[2] [https://www.programiz.com/sql/left-join](https://www.programiz.com/sql/left-join)
