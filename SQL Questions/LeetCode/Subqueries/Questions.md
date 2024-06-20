
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