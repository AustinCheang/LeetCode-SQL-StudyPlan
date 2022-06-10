# E1667 - Fix Names in a Table

## Description

Table: `Users`

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| name           | varchar |
+----------------+---------+
user_id is the primary key for this table.
This table contains the ID and the name of the user. The name consists of only lowercase and uppercase characters.
```

 

Write an SQL query to fix the names so that only the first character is uppercase and the rest are lowercase.

Return the result table ordered by `user_id`.

The query result format is in the following example.

 

**Example 1:**

```
Input: 
Users table:
+---------+-------+
| user_id | name  |
+---------+-------+
| 1       | aLice |
| 2       | bOB   |
+---------+-------+
Output: 
+---------+-------+
| user_id | name  |
+---------+-------+
| 1       | Alice |
| 2       | Bob   |
+---------+-------+
```



## Solution

- We approach this question just like solving normal coding question. We used `SUBSTRING` (extracts some characters from a string - `SUBSTRING(*string*, *start*, *length*)`) to extract the first character for upper and the rest become lower

```sql
SELECT user_id, CONCAT(UPPER(SUBSTRING(name, 1, 1)) , LOWER(SUBSTRING(name, 2, LENGTH(name) -1))) AS name
FROM Users
ORDER BY user_id;
```

```sql
SELECT user_id, CONCAT(UPPER(LEFT(name, 1)), LOWER(SUBSTRING(name, 2))) AS name 
FROM Users 
ORDER BY user_id;
```

