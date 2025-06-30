
## 1. [Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)

```sql
# Write your MySQL query statement below

SELECT      c.id, c.movie, c.description, c.rating
FROM        Cinema c
WHERE       c.id % 2 != 0
AND         c.description != 'boring'
ORDER BY    c.rating DESC;
```

## 2. [Average Selling Price](https://leetcode.com/problems/average-selling-price/)

```sql
SELECT      p.product_id, coalesce(ROUND(SUM(p.price*u.units)/SUM(u.units)::numeric,2),0) as average_price
FROM        Prices as p
LEFT JOIN   UnitsSold as u
ON          p.product_id = u.product_id 
            AND u.purchase_date >= p.start_date 
            AND u.purchase_date <= p.end_date
GROUP BY    p.product_id;
```

## 3. [Project Employees I](https://leetcode.com/problems/project-employees-i/)

```sql
-- Write your PostgreSQL query statement below

SELECT      p.project_id, ROUND(SUM(e.experience_years)/count(p.project_id)::NUMERIC,2) AS average_years
FROM        Project p
LEFT JOIN   Employee e
ON          e.employee_id = p.employee_id
GROUP BY    p.project_id;
```

## 4. [Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/)

```sql
-- Write your PostgreSQL query statement below

WITH UsersCount AS (
    SELECT COUNT(*) AS count FROM Users
)
SELECT        r.contest_id, ROUND((COUNT(r.contest_id) * 100.0 / uc.count)::NUMERIC, 2) AS percentage
FROM          Register r
LEFT JOIN     Users u
ON            r.user_id = u.user_id, UsersCount uc
GROUP BY      r.contest_id,
              uc.count
ORDER BY      percentage DESC,
              r.contest_id;
```

## 5. [Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/)

```sql
# Write your MySQL query statement below


SELECT      query_name,
            ROUND(AVG(rating / position), 2) AS quality,
            ROUND(100.0 * SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END) / COUNT(*), 2) AS poor_query_percentage
FROM        Queries
WHERE       query_name IS NOT NULL
GROUP BY    query_name;
```

## 6. [Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/)

Postgres
```sql
-- Write your PostgreSQL query statement below

SELECT 
    TO_CHAR(trans_date, 'YYYY-MM') AS month, 
    country, 
    COUNT(*) AS trans_count,
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count, 
    SUM(amount) AS trans_total_amount, 
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM 
    Transactions 
GROUP BY 
    TO_CHAR(trans_date, 'YYYY-MM'), 
    country;
```

MySQL
```sql
# Write your MySQL query statement below

SELECT 
    DATE_FORMAT(trans_date, '%Y-%m') AS month
    , country
    , COUNT(*) AS trans_count
    , SUM(IF(state = 'approved', 1, 0)) AS approved_count
    , SUM(amount) AS trans_total_amount
    , SUM(IF(state = 'approved', amount, 0)) AS approved_total_amount
FROM Transactions
GROUP BY month, country
```

SQLServer
```sql
/* Write your T-SQL query statement below */

SELECT 
    FORMAT(trans_date, 'yyyy-MM') AS month
    , country
    , COUNT(*) AS trans_count
    , SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count
    , SUM(amount) AS trans_total_amount
    , SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM 
    Transactions 
GROUP BY 
    FORMAT(trans_date, 'yyyy-MM'), 
    country;
```

## 7.  [Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/)

```sql
/* Write your T-SQL query statement below */

WITH cte AS (
    SELECT 
        d.customer_id, 
        MIN(d.order_date) AS fir, 
        MIN(d.customer_pref_delivery_date) AS pref
    FROM 
        Delivery AS d
    GROUP BY 
        d.customer_id
)
SELECT 
    ROUND(SUM(CASE WHEN fir = pref THEN 1 ELSE 0 END)*100.0 / COUNT(*), 2) AS immediate_percentage
FROM 
    cte;
```

```sql
-- Write your PostgreSQL query statement below


with cte as (
    SELECT d.customer_id, 
            MIN(d.order_date) AS fir, 
            MIN(d.customer_pref_delivery_date) AS pref
        FROM Delivery AS d
        GROUP BY d.customer_id
        ORDER BY fir ASC
)
select round(sum(case when fir=pref then 1 else 0 end)::numeric/count(*)*100,2)  as immediate_percentage
from cte;
```

## 8. [Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/)

```sql
/* Write your T-SQL query statement below */

WITH FirstLogin AS (
    SELECT player_id, MIN(event_date) AS first_login_date
    FROM Activity
    GROUP BY player_id
),
NextDayLogin AS (
    SELECT f.player_id
    FROM FirstLogin f
    JOIN Activity a
    ON f.player_id = a.player_id
    WHERE a.event_date = DATEADD(day, 1, f.first_login_date)
)
SELECT ROUND(COUNT(DISTINCT n.player_id)*1.0 /COUNT(DISTINCT f.player_id), 2) AS fraction
FROM FirstLogin f
LEFT JOIN NextDayLogin n
ON f.player_id = n.player_id;
```