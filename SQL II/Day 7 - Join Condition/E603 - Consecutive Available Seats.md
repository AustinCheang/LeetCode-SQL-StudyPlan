# E603 - Consecutive Available Seats

## Description

Table: `Cinema`

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| seat_id     | int  |
| free        | bool |
+-------------+------+
seat_id is an auto-increment primary key column for this table.
Each row of this table indicates whether the ith seat is free or not. 1 means free while 0 means occupied.
```

 

Write an SQL query to report all the consecutive available seats in the cinema.

Return the result table **ordered** by `seat_id` **in ascending order**.

The test cases are generated so that more than two seats are consecutively available.

The query result format is in the following example.

 

**Example 1:**

```
Input: 
Cinema table:
+---------+------+
| seat_id | free |
+---------+------+
| 1       | 1    |
| 2       | 0    |
| 3       | 1    |
| 4       | 1    |
| 5       | 1    |
+---------+------+
Output: 
+---------+
| seat_id |
+---------+
| 3       |
| 4       |
| 5       |
+---------+
```



## Solution

- We can join its table and use `ABS(a.seat_id - b.seat_id) = 1` to check for consectutive

```sql
SELECT DISTINCT a.seat_id
FROM cinema a 
JOIN cinema b
ON abs(a.seat_id - b.seat_id) = 1
AND a.free = true AND b.free = true
ORDER BY a.seat_id;
```

