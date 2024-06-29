
## 1. [Fix Names in a Table](https://leetcode.com/problems/fix-names-in-a-table/)

```sql
/* Write your T-SQL query statement below */

SELECT 
    user_id,
    UPPER(LEFT(name, 1)) + LOWER(SUBSTRING(name, 2, LEN(name) - 1)) AS name
FROM 
    Users
ORDER BY 
    user_id;
```