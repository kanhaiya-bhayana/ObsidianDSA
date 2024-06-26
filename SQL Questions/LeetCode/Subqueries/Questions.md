
## 1. [Employees Whose Manager Left the Company](https://leetcode.com/problems/employees-whose-manager-left-the-company/)

```sql
/* Write your T-SQL query statement below */

SELECT      employee_id
FROM        Employees e
WHERE       salary  < 30000
            AND
            manager_id NOT IN
            (SELECT employee_id from Employees)
ORDER BY    employee_id;
```

## 2. [Exchange Seats](https://leetcode.com/problems/exchange-seats/)

```sql
/* Write your T-SQL query statement below */

WITH CTE AS (
    SELECT
        id,
        student,
        ROW_NUMBER() OVER (ORDER BY id) AS rn
    FROM
        Seat
)
SELECT
    CASE
        WHEN rn % 2 = 1 AND rn + 1 <= (SELECT COUNT(*) FROM Seat) THEN rn + 1
        WHEN rn % 2 = 0 THEN rn - 1
        ELSE rn
    END AS id,
    student
FROM
    CTE
ORDER BY
    id ASC;
```


## 3. [Movie Rating](https://leetcode.com/problems/movie-rating/)

```sql
/* Write your T-SQL query statement below */


WITH TopUser AS (
    SELECT TOP 1 
        u.name AS name1, 
        COUNT(*) AS rating_count
    FROM 
        MovieRating mr
    INNER JOIN 
        Users u ON u.user_id = mr.user_id
    GROUP BY 
        u.name
    ORDER BY 
        rating_count DESC, 
        name1 ASC
),
MovieRatings AS (
    SELECT TOP 1
        m.title, 
        AVG(CAST(mr.rating AS DECIMAL(10, 2))) AS average_rating
    FROM 
        MovieRating mr 
    INNER JOIN 
        Movies m ON mr.movie_id = m.movie_id
    WHERE 
        mr.created_at >= '2020-02-01' AND mr.created_at <= '2020-02-29'
    GROUP BY 
        m.title
    ORDER BY 
        average_rating DESC, 
        m.title ASC
)

-- Combine the results from TopUser and MovieRatings
SELECT name1 AS results
FROM TopUser

UNION ALL

SELECT title AS results
FROM MovieRatings;
```


## 4. [Restaurant Growth](https://leetcode.com/problems/restaurant-growth/)

```sql
WITH temp AS (
    SELECT
        visited_on,
        SUM(amount) AS amount
    FROM
        Customer
    GROUP BY
        visited_on
)
SELECT
    visited_on,
    SUM(amount) OVER (ORDER BY visited_on ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS amount,
    ROUND(AVG(amount * 1.00) OVER (ORDER BY visited_on ROWS BETWEEN 6 PRECEDING AND CURRENT ROW), 2) AS average_amount
FROM
    temp
ORDER BY
    visited_on
OFFSET 6 ROWS;

```

## 5. [Friend Requests II: Who Has the Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/)

```sql
/* Write your T-SQL query statement below */

SELECT TOP 1 
    id, 
    COUNT(*) as num
FROM    
    (
        SELECT requester_id as id FROM RequestAccepted 
        UNION ALL
        SELECT accepter_id FROM RequestAccepted 
    ) friend_request
GROUP BY    
    id
ORDER BY 
    num DESC;
```

