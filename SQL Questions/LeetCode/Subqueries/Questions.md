
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


