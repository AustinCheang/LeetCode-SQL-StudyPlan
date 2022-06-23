# M1783 - Grand Slam Titles

## Description

Table: `Players`

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| player_id      | int     |
| player_name    | varchar |
+----------------+---------+
player_id is the primary key for this table.
Each row in this table contains the name and the ID of a tennis player.
```

 

Table: `Championships`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| year          | int     |
| Wimbledon     | int     |
| Fr_open       | int     |
| US_open       | int     |
| Au_open       | int     |
+---------------+---------+
year is the primary key for this table.
Each row of this table contains the IDs of the players who won one each tennis tournament of the grand slam.
```

 

Write an SQL query to report the number of grand slam tournaments won by each player. Do not include the players who did not win any tournament.

Return the result table in **any order**.

The query result format is in the following example.

 

**Example 1:**

```
Input: 
Players table:
+-----------+-------------+
| player_id | player_name |
+-----------+-------------+
| 1         | Nadal       |
| 2         | Federer     |
| 3         | Novak       |
+-----------+-------------+
Championships table:
+------+-----------+---------+---------+---------+
| year | Wimbledon | Fr_open | US_open | Au_open |
+------+-----------+---------+---------+---------+
| 2018 | 1         | 1       | 1       | 1       |
| 2019 | 1         | 1       | 2       | 2       |
| 2020 | 2         | 1       | 2       | 2       |
+------+-----------+---------+---------+---------+
Output: 
+-----------+-------------+-------------------+
| player_id | player_name | grand_slams_count |
+-----------+-------------+-------------------+
| 2         | Federer     | 5                 |
| 1         | Nadal       | 7                 |
+-----------+-------------+-------------------+
Explanation: 
Player 1 (Nadal) won 7 titles: Wimbledon (2018, 2019), Fr_open (2018, 2019, 2020), US_open (2018), and Au_open (2018).
Player 2 (Federer) won 5 titles: Wimbledon (2020), US_open (2019, 2020), and Au_open (2019, 2020).
Player 3 (Novak) did not win anything, we did not include them in the result table.
```



## Solution

- We can use `UNION ALL` to join all the winners of each columns and `COUNT`
- We can also use `CASE WHEN` to match the `player_id` and perform `SUM`, then use `WHERE` to filter the count less than 0

```sql
SELECT player_id, player_name , COUNT(*) AS grand_slams_count 
FROM
    (
    SELECT p.player_id, p.player_name  
    FROM players AS p 
    RIGHT JOIN championships AS c 
    on p.player_id = c.Wimbledon 

    UNION ALL

    SELECT p.player_id, p.player_name  
    FROM players AS p 
    RIGHT JOIN championships AS c 
    ON p.player_id = c.Fr_open 

    UNION ALL

    SELECT p.player_id, p.player_name  
    FROM players AS p 
    RIGHT JOIN championships AS c 
    ON p.player_id = c.US_open 

    UNION ALL

    SELECT p.player_id, p.player_name  
    FROM players AS p 
    RIGHT JOIN championships AS c 
    ON p.player_id = c.Au_open ) AS t 
GROUP BY player_name; 
```

```sql
SELECT * 
FROM (
	SELECT p.player_id, p.player_name, 
		SUM(CASE WHEN c.Wimbledon=p.player_id THEN 1 ELSE 0 END) +
		SUM(CASE WHEN c.Fr_open=p.player_id THEN 1 ELSE 0 END) +
		SUM(CASE WHEN c.US_open=p.player_id THEN 1 ELSE 0 END) +
		SUM(CASE WHEN c.Au_open=p.player_id THEN 1 ELSE 0 END) 
		AS grand_slams_count
    FROM Players p, Championships AS c
	GROUP BY p.player_id, p.player_name
) AS a
WHERE grand_slams_count > 0
```

