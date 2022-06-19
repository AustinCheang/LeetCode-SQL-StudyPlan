# M1398 - Customers Who Bought Products A and B but Not C

## Description

Table: `Customers`

```
+---------------------+---------+
| Column Name         | Type    |
+---------------------+---------+
| customer_id         | int     |
| customer_name       | varchar |
+---------------------+---------+
customer_id is the primary key for this table.
customer_name is the name of the customer.
```

 

Table: `Orders`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| product_name  | varchar |
+---------------+---------+
order_id is the primary key for this table.
customer_id is the id of the customer who bought the product "product_name".
```

 

Write an SQL query to report the customer_id and customer_name of customers who bought products **"A"**, **"B"** but did not buy the product **"C"** since we want to recommend them to purchase this product.

Return the result table **ordered** by `customer_id`.

The query result format is in the following example.

 

**Example 1:**

```
Input: 
Customers table:
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 1           | Daniel        |
| 2           | Diana         |
| 3           | Elizabeth     |
| 4           | Jhon          |
+-------------+---------------+
Orders table:
+------------+--------------+---------------+
| order_id   | customer_id  | product_name  |
+------------+--------------+---------------+
| 10         |     1        |     A         |
| 20         |     1        |     B         |
| 30         |     1        |     D         |
| 40         |     1        |     C         |
| 50         |     2        |     A         |
| 60         |     3        |     A         |
| 70         |     3        |     B         |
| 80         |     3        |     D         |
| 90         |     4        |     C         |
+------------+--------------+---------------+
Output: 
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 3           | Elizabeth     |
+-------------+---------------+
Explanation: Only the customer_id with id 3 bought the product A and B but not the product C.
```



## Solution

- We can filter the conditions on by one using subquery
- Or we can `COUNT` the number of `A, B, C` which matches our requirement

```sql
SELECT DISTINCT c.customer_id, c.customer_name
FROM customers c
JOIN orders o
ON c.customer_id = o.customer_id
WHERE c.customer_id IN
    (SELECT o1.customer_id
    FROM orders o1
    WHERE o1.product_name ='A') 
    AND 
    c.customer_id IN
    (SELECT o2.customer_id
    FROM orders o2
    WHERE o2.product_name ='B') 
    AND
    c.customer_id NOT IN
    (SELECT o3.customer_id
    FROM orders o3
    WHERE o3.product_name ='C');
```

```sql
SELECT customer_id,customer_name 
FROM Customers
WHERE customer_id IN (
    SELECT customer_id 
    FROM Orders
    GROUP BY customer_id      
    HAVING SUM(product_name='A') > 0 
    AND SUM(product_name='B') > 0
    AND SUM(product_name='C') = 0);
```

