# M1440 - Evaluate Boolean Expression

## Description

Table `Variables`:

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| name          | varchar |
| value         | int     |
+---------------+---------+
name is the primary key for this table.
This table contains the stored variables and their values.
```

 

Table `Expressions`:

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| left_operand  | varchar |
| operator      | enum    |
| right_operand | varchar |
+---------------+---------+
(left_operand, operator, right_operand) is the primary key for this table.
This table contains a boolean expression that should be evaluated.
operator is an enum that takes one of the values ('<', '>', '=')
The values of left_operand and right_operand are guaranteed to be in the Variables table.
```

 

Write an SQL query to evaluate the boolean expressions in `Expressions` table.

Return the result table in **any order**.

The query result format is in the following example.

 

**Example 1:**

```
Input: 
Variables table:
+------+-------+
| name | value |
+------+-------+
| x    | 66    |
| y    | 77    |
+------+-------+
Expressions table:
+--------------+----------+---------------+
| left_operand | operator | right_operand |
+--------------+----------+---------------+
| x            | >        | y             |
| x            | <        | y             |
| x            | =        | y             |
| y            | >        | x             |
| y            | <        | x             |
| x            | =        | x             |
+--------------+----------+---------------+
Output: 
+--------------+----------+---------------+-------+
| left_operand | operator | right_operand | value |
+--------------+----------+---------------+-------+
| x            | >        | y             | false |
| x            | <        | y             | true  |
| x            | =        | y             | false |
| y            | >        | x             | true  |
| y            | <        | x             | false |
| x            | =        | x             | true  |
+--------------+----------+---------------+-------+
Explanation: 
As shown, you need to find the value of each boolean expression in the table using the variables table.
```



## Solution

- We first join the `Variables` to `Expressions` twice and then use `CASE` to perform comperations for each `operator`

```sql
SELECT left_operand, operator, right_operand,
	CASE 
	WHEN e.operator = '>' AND v1.value > v2.value THEN 'true' 
	WHEN e.operator = '<' AND v1.value < v2.value THEN 'true' 
	WHEN e.operator = '=' AND v1.value = v2.value THEN 'true' 
	ELSE 'false' END AS value
FROM Expressions e 
LEFT JOIN VARIABLES v1 
    on e.left_operand = v1.name
LEFT JOIN VARIABLES v2 
    on e.right_operand = v2.name
```
