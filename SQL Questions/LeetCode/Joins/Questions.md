
## 1. [Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)


```sql
-- Write your PostgreSQL query statement below


SELECT      u.unique_id, e.name
FROM        Employees e
LEFT JOIN  EmployeeUNI u
ON          u.id = e.id;
```


## 2. [Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/)

```sql
-- Write your PostgreSQL query statement below

SELECT      p.product_name, s.year, s.price
FROM        Sales s
INNER JOIN  Product p
ON          p.product_id = s.product_id;
```

## 3. [Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/)

```sql
-- Write your PostgreSQL query statement below

SELECT      v.customer_id, COUNT(v.visit_id) AS count_no_trans
FROM        Visits v
LEFT JOIN   Transactions t
ON          v.visit_id = t.visit_id
WHERE       t.transaction_id IS NULL
GROUP BY    v.customer_id;
```


## 4. [Rising Temperature](https://leetcode.com/problems/rising-temperature/)

Postgres
```sql
-- Write your PostgreSQL query statement below

SELECT      w.id
FROM        Weather w
CROSS JOIN  Weather ww
WHERE       w.recordDate = ww.recordDate+1 AND w.temperature > ww.temperature;
```

SQL Server
```sql
/* Write your T-SQL query statement below */

SELECT      w1.id
FROM        Weather w1
cross JOIN  Weather w2
WHERE       w1.recordDate = DATEADD(day,1,w2.recordDate) 
            AND 
            w1.temperature > w2.temperature;
```
## 5. [Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/)

```sql
-- Write your PostgreSQL query statement below

SELECT      s.machine_id, ROUND(AVG(e.timestamp-s.timestamp)::numeric,3) as processing_time
FROM        Activity s
JOIN        Activity e
ON          s.machine_id = e.machine_id
AND         s.process_id = e.process_id
AND         s.activity_type='start'
AND         e.activity_type='end'

GROUP BY    s.machine_id
ORDER BY    s.machine_id;
```

## 6. [Employee Bonus](https://leetcode.com/problems/employee-bonus/)

```sql
-- Write your PostgreSQL query statement below

SELECT      e.name, b.bonus
FROM        Employee e
LEFT JOIN   Bonus b
ON          e.empId = b.empId
WHERE       b.bonus < 1000 
OR          b.bonus IS NULL;
```

## 7. [Students and Examinations](https://leetcode.com/problems/students-and-examinations/)

```sql
# Write your MySQL query statement below

SELECT      s.student_id, s.student_name, 
            sub.subject_name, 
            COALESCE(COUNT(e.student_id),0) AS attended_exams 
FROM        Students s
CROSS JOIN  Subjects sub
LEFT JOIN   Examinations e
ON          s.student_id = e.student_id 
AND         sub.subject_name = e.subject_name
GROUP BY    s.student_id, s.student_name, sub.subject_name
ORDER BY    s.student_id, sub.subject_name;
```

## 8. [Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/)

```sql
# Write your MySQL query statement below

SELECT      b.name
FROM        Employee a
JOIN        Employee b
ON          a.managerId = b.id
GROUP BY    a.managerId
HAVING      COUNT(*) >=5;
```

## 9. [Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)

```sql
# Write your MySQL query statement below

WITH req AS (
    SELECT user_id, COUNT(*) as requested
    FROM Confirmations
    GROUP BY user_id
)
SELECT      s.user_id, 
            ROUND(COUNT(c.user_id) / COALESCE(r.requested, 1),2) as confirmation_rate 
FROM        Signups s
LEFT JOIN   Confirmations c 
ON          s.user_id = c.user_id 
AND         c.action = 'confirmed'
LEFT JOIN   req r ON s.user_id = r.user_id
GROUP BY    s.user_id, r.requested;
```