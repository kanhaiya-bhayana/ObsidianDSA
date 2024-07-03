
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

## 2. [Patients With a Condition](https://leetcode.com/problems/patients-with-a-condition/)

```sql
/* Write your T-SQL query statement below */

SELECT 
    patient_id, patient_name, conditions
FROM
    Patients
WHERE 
    conditions LIKE 'DIAB1%' OR conditions LIKE '% DIAB1%' ;
```

## 3. [Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/)

```sql
/* Write your T-SQL query statement below */

DELETE
    p1
FROM
    Person p1, Person p2
WHERE
    p1.email = p2.email AND p1.id > p2.id;
```

## 4. [Second Highest Salary](https://leetcode.com/problems/second-highest-salary/)

```sql
/* Write your T-SQL query statement below */

SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary <> (SELECT MAX(salary) FROM Employee);
```

## 5. [Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date/)

```sql
/* Write your T-SQL query statement below */

WITH t AS (
    SELECT DISTINCT *
    FROM Activities
)

SELECT 
    t.sell_date,
    COUNT(t.product) AS num_sold,
    STRING_AGG(t.product, ',') WITHIN GROUP (ORDER BY t.product) AS products
FROM t
GROUP BY t.sell_date
ORDER BY t.sell_date;
```

