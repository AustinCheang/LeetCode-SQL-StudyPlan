# E182 - Duplicate Emails

## Description

Table: `Person`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id is the primary key column for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.
```

 

Write an SQL query to report all the duplicate emails.

Return the result table in **any order**.

The query result format is in the following example.

 

**Example 1:**

```
Input: 
Person table:
+----+---------+
| id | email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
Output: 
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
Explanation: a@b.com is repeated two times.
```



## Solution

- We approached this question by using `GROUP BY` and `COUNT(EMAIL) > 1` to find the duplicated emails

```sql
SELECT email
FROM Person
GROUP BY email
HAVING COUNT(email) > 1;
```

