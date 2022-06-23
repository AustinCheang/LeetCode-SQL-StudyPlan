# M1164 - Product Price at a Given Date

## Description

Table: `Products`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |
+---------------+---------+
(product_id, change_date) is the primary key of this table.
Each row of this table indicates that the price of some product was changed to a new price at some date.
```

 

Write an SQL query to find the prices of all products on `2019-08-16`. Assume the price of all products before any change is `10`.

Return the result table in **any order**.

The query result format is in the following example.

 

**Example 1:**

```
Input: 
Products table:
+------------+-----------+-------------+
| product_id | new_price | change_date |
+------------+-----------+-------------+
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |
+------------+-----------+-------------+
Output: 
+------------+-------+
| product_id | price |
+------------+-------+
| 2          | 50    |
| 1          | 35    |
| 3          | 10    |
+------------+-------+
```



## Solution

- We first get the `MAX(change_date)` to see if the latest date has changed the price before `2019-08-16` or not
- For the products that has not changed any price, we use `MIN(change_date)> 2019-08-16` can get the products.

```sql
SELECT T_2.product_id,P.new_price AS price 
FROM Products P
JOIN
    (SELECT product_id,MAX(change_date) AS change_date 
     FROM Products
     WHERE change_date<='2019-08-16'
     GROUP BY product_id) AS T_2
ON T_2.product_id=P.product_id AND T_2.change_date=P.change_date

UNION

SELECT T_3.product_id,10 AS price 
FROM
    (SELECT T_1.product_id,T_1.min_date 
     FROM
        (SELECT product_id,MIN(change_date) AS min_date 
         FROM Products
         GROUP BY product_id) AS T_1
    WHERE T_1.min_date>'2019-08-16') AS T_3
```

