
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



## 6. [Investments in 2016](https://leetcode.com/problems/investments-in-2016/)

```sql
/* Write your T-SQL query statement below */
SELECT      
    ROUND(SUM(tiv_2016),2) as tiv_2016
FROM
    Insurance AS i1
WHERE       
    tiv_2015 IN (
        SELECT 
            tiv_2015
        FROM
            Insurance
        GROUP BY
            tiv_2015
        HAVING 
            COUNT(tiv_2015) > 1
    )
AND
    EXISTS (
        SELECT 1
        FROM Insurance AS i2
        WHERE i1.lat = i2.lat AND i1.lon = i2.lon
        GROUP BY lat, lon
        HAVING COUNT(*) = 1
    );
```

## 7. [Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/)

```sql
/* Write your T-SQL query statement below */

SELECT 
    d.name AS Department, 
    e.name AS Employee, 
    e.salary AS Salary
FROM 
    Employee e
JOIN 
    Department d ON e.departmentId = d.id
WHERE 
    e.salary IN (
        SELECT TOP 3 sub.salary
        FROM 
            Employee sub
        WHERE 
            sub.departmentId = e.departmentId
        GROUP BY 
            sub.salary
        ORDER BY 
            sub.salary DESC
    )
ORDER BY 
    Department, Salary DESC;
```