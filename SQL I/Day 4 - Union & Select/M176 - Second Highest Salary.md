# E176 - Second Highest Salary

## Description

Table: `Employee`

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
id is the primary key column for this table.
Each row of this table contains information about the salary of an employee.
```

 

Write an SQL query to report the second highest salary from the `Employee` table. If there is no second highest salary, the query should report `null`.

The query result format is in the following example.

 

**Example 1:**

```
Input: 
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
Output: 
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

**Example 2:**

```
Input: 
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
+----+--------+
Output: 
+---------------------+
| SecondHighestSalary |
+---------------------+
| null                |
+---------------------+
```



## Solution

- We approach this question by sorting the salary by `DESC` and use `LIMIT` `OFFSET` to limit only second result to be shown.
- We have to wrap it as sub-query to solve if there is no such second highest salary since there might be only one record in this table.

```sql
SELECT (
    SELECT e.salary 
    FROM Employee e
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1 ) 
    AS SecondHighestSalary;
```

```sql
SELECT IFNULL(
      (SELECT DISTINCT Salary
       FROM Employee
       ORDER BY Salary DESC
       LIMIT 1 OFFSET 1),
    NULL) 
    AS SecondHighestSalary;
```

```SQL
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee)
```

