# E197 - Rising Temperature

## Description

Table: `Weather`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id is the primary key for this table.
This table contains information about the temperature on a certain day.
```

 

Write an SQL query to find all dates' `Id` with higher temperatures compared to its previous dates (yesterday).

Return the result table in **any order**.

The query result format is in the following example.

 

**Example 1:**

```
Input: 
Weather table:
+----+------------+-------------+
| id | recordDate | temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+
Output: 
+----+
| id |
+----+
| 2  |
| 4  |
+----+
Explanation: 
In 2015-01-02, the temperature was higher than the previous day (10 -> 25).
In 2015-01-04, the temperature was higher than the previous day (20 -> 30).
```



## Solution

- We first compute the date difference in order to know which two temperatures to compare using `TO_DATE` , `DATEIFF` or `DATE_ADD`

```sql
SELECT w1.id
FROM Weather w1 
JOIN Weather w2
WHERE TO_DAYS(w1.recordDate) - TO_DAYS(w2.recordDate) = 1 AND w1.temperature > w2.temperature;
```

```sql
SELECT w1.id
FROM Weather AS w1 , Weather AS w2
WHERE DATEDIFF(w1.recordDate , w2.recordDate) = 1 AND w1.Temperature > w2.Temperature;
```

```sql
SELECT w1.id
FROM weather w1 
JOIN weather w2 
ON w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)
WHERE w1.temperature > w2.temperature;
```

