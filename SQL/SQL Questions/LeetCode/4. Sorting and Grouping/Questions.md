
## 1. [Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/)

```sql
/* Write your T-SQL query statement below */

SELECT teacher_id, COUNT(DISTINCT subject_id) AS cnt
FROM Teacher
GROUP BY teacher_id;
```

## 2. [User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/)

```sql
/* Write your T-SQL query statement below */

SELECT activity_date AS day, 
       COUNT(DISTINCT user_id) AS active_users
FROM Activity 
WHERE activity_date > '2019-06-27' 
  AND activity_date <= '2019-07-27'
GROUP BY activity_date;
```

## 3. [Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/)

```sql
/* Write your T-SQL query statement below */

SELECT 
    s.product_id,
    s.year AS first_year,
    s.quantity,
    s.price
FROM 
    Sales s
JOIN 
    (SELECT product_id, MIN(year) AS first_year
     FROM Sales
     GROUP BY product_id) sub
ON 
    s.product_id = sub.product_id AND s.year = sub.first_year;
```

## 4. [Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students/)

```sql
/* Write your T-SQL query statement below */

SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;
```

## 5. [Find Followers Count](https://leetcode.com/problems/find-followers-count/)

```sql
/* Write your T-SQL query statement below */


SELECT      user_id, COUNT(follower_id) as followers_count
FROM        Followers
GROUP BY    user_id
ORDER BY    user_id;
```



## 6. [Biggest Single Number](https://leetcode.com/problems/biggest-single-number/)

```sql
/* Write your T-SQL query statement below */

WITH SingleNumberCTE AS(
    SELECT      num
    FROM        MyNumbers
    GROUP BY    num
    HAVING      COUNT(*) = 1
)
SELECT  MAX(num) AS num
FROM    SingleNumberCTE;
```

## 7. [Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/)

```sql
/* Write your T-SQL query statement below */

SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(*) FROM Product);
```