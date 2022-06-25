# M1459 - Rectangles Area

## Description

Table: `Points`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| x_value       | int     |
| y_value       | int     |
+---------------+---------+
id is the primary key for this table.
Each point is represented as a 2D coordinate (x_value, y_value).
```

 

Write an SQL query to report all possible **axis-aligned** rectangles with a **non-zero area** that can be formed by any two points from the `Points` table.

Each row in the result should contain three columns `(p1, p2, area)` where:

- `p1` and `p2` are the `id`'s of the two points that determine the opposite corners of a rectangle.
- `area` is the area of the rectangle and must be **non-zero**.

Return the result table **ordered** by `area` **in descending order**. If there is a tie, order them by `p1` **in ascending order**. If there is still a tie, order them by `p2` **in ascending order**.

The query result format is in the following table.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/12/rect.png)

```
Input: 
Points table:
+----------+-------------+-------------+
| id       | x_value     | y_value     |
+----------+-------------+-------------+
| 1        | 2           | 7           |
| 2        | 4           | 8           |
| 3        | 2           | 10          |
+----------+-------------+-------------+
Output: 
+----------+-------------+-------------+
| p1       | p2          | area        |
+----------+-------------+-------------+
| 2        | 3           | 4           |
| 1        | 2           | 2           |
+----------+-------------+-------------+
Explanation: 
The rectangle formed by p1 = 2 and p2 = 3 has an area equal to |4-2| * |8-10| = 4.
The rectangle formed by p1 = 1 and p2 = 2 has an area equal to |2-4| * |7-8| = 2.
Note that the rectangle formed by p1 = 1 and p2 = 3 is invalid because the area is 0.
```



## Solution

- We first calculate the area of points after getting all the combinations (`P1.id != P2.id AND P1.id < P2.id`)

- If the area is 0 then it is invalid, therefore we remove the points where `area < 0`

```sql
SELECT T_1.p1 AS P1,T_1.p2 AS P2,T_1.area AS AREA 
FROM
    (SELECT DISTINCT P1.id AS p1,
     P2.id AS p2,
     abs(P1.x_value - P2.x_value) * abs(P1.y_value - P2.y_value) AS area
    FROM Points P1, Points P2
    WHERE P1.id != P2.id AND P1.id < P2.id) AS T_1
WHERE T_1.area > 0
ORDER BY AREA DESC, P1 ASC, P2 ASC
```

