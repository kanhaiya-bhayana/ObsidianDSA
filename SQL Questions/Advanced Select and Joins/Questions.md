
## 1. [The Number of Employees Which Report to Each Employee](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/)

```sql
/* Write your T-SQL query statement below */

SELECT 
    e.employee_id,
    e.name,
    COUNT(r.employee_id) AS reports_count,
    ROUND(AVG(r.age * 1.0), 0) AS average_age
FROM 
    Employees e
JOIN 
    Employees r ON e.employee_id = r.reports_to
GROUP BY 
    e.employee_id, e.name
ORDER BY 
    e.employee_id;
```


## 2. [Primary Department for Each Employee](https://leetcode.com/problems/primary-department-for-each-employee/)

```sql
/* Write your T-SQL query statement below */

SELECT employee_id, department_id
FROM Employee
WHERE primary_flag = 'Y'
UNION
SELECT employee_id, department_id
FROM Employee
WHERE employee_id IN (
    SELECT employee_id
    FROM Employee
    GROUP BY employee_id
    HAVING COUNT(*) = 1
);
```

