
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

## 3. [Triangle Judgement](https://leetcode.com/problems/triangle-judgement/)

```sql
/* Write your T-SQL query statement below */

SELECT      X,Y,Z,
CASE        WHEN (X + Y > Z) 
            AND  (X + Z > Y)
            AND  (Y + Z > X)
            THEN 'Yes'
            ELSE 'No'
END
            AS triangle
FROM        Triangle;
```

## 4. [Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/)

```sql
/* Write your T-SQL query statement below */

SELECT      DISTINCT l1.num AS ConsecutiveNums
FROM        Logs l1
JOIN        Logs l2
ON          l1.id = l2.id - 1
JOIN        Logs l3
ON          l1.id = l3.id - 2
WHERE       l1.num = l2.num
            AND 
            l2.num = l3.num;
```

## 5. [Product Price at a Given Date](https://leetcode.com/problems/product-price-at-a-given-date/)

```sql
/* Write your T-SQL query statement below */


SELECT DISTINCT 
    product_id,
    COALESCE(
        (SELECT TOP 1 new_price 
         FROM products b 
         WHERE b.product_id = a.product_id 
           AND CAST(change_date AS date) <= CAST('20190816' AS date) 
         ORDER BY change_date DESC), 
        10
    ) AS price
FROM 
    products a
```

## 6. [Last Person to Fit in the Bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus/)

```sql
/* Write your T-SQL query statement below */

WITH CumulativeWeight AS (
    SELECT 
        person_name,
        weight,
        SUM(weight) OVER (ORDER BY turn) AS cumulative_weight
    FROM 
        Queue
)
SELECT 
    person_name
FROM 
    CumulativeWeight
WHERE 
    cumulative_weight <= 1000
ORDER BY 
    cumulative_weight DESC
OFFSET 0 ROWS FETCH NEXT 1 ROW ONLY;
```

The clause `OFFSET 0 ROWS FETCH NEXT 1 ROW ONLY;` in SQL Server is used to limit the number of rows returned by a query. Specifically:

- `OFFSET 0 ROWS`: Skips the first 0 rows (effectively starting from the first row).
- `FETCH NEXT 1 ROW ONLY`: Fetches the next 1 row from the result set.

In the context of the given query, it serves the purpose of retrieving only the top row from the ordered result set. Here's why it's significant in this query:

### Purpose in the Query

1. **Ordering by Cumulative Weight**: 
   - The query orders the rows by `cumulative_weight` in descending order. This ensures that the person with the highest cumulative weight that does not exceed 1000 kg comes first.

2. **Selecting the Last Person Who Can Board**:
   - After ordering the rows, `OFFSET 0 ROWS FETCH NEXT 1 ROW ONLY` limits the result to the single row at the top of this ordered list, which corresponds to the last person who can board the bus without exceeding the weight limit.

### Complete Query Breakdown
Here's the complete query with the significance of each part:

```sql
WITH CumulativeWeight AS (
    SELECT 
        person_name,
        weight,
        SUM(weight) OVER (ORDER BY turn) AS cumulative_weight
    FROM 
        Queue
)
SELECT 
    person_name
FROM 
    CumulativeWeight
WHERE 
    cumulative_weight <= 1000
ORDER BY 
    cumulative_weight DESC
OFFSET 0 ROWS FETCH NEXT 1 ROW ONLY;
```

1. **CTE (Common Table Expression)**:
   - `CumulativeWeight`: Calculates the cumulative weight for each person based on the order they board the bus (`turn`).

2. **Filtering**:
   - `WHERE cumulative_weight <= 1000`: Filters out any rows where the cumulative weight exceeds 1000 kg.

3. **Ordering**:
   - `ORDER BY cumulative_weight DESC`: Orders the remaining rows by cumulative weight in descending order so that the person with the highest cumulative weight that is still within the limit comes first.

4. **Limiting the Result**:
   - `OFFSET 0 ROWS FETCH NEXT 1 ROW ONLY`: Ensures that only the top row (the last person who can board without exceeding the weight limit) is returned.

### Example

Given the example data:

| person_id | person_name | weight | turn |
|-----------|-------------|--------|------|
| 5         | Alice       | 250    | 1    |
| 3         | Alex        | 350    | 2    |
| 6         | John Cena   | 400    | 3    |
| 2         | Marie       | 200    | 4    |
| 4         | Bob         | 175    | 5    |
| 1         | Winston     | 500    | 6    |

After calculating cumulative weights and filtering those within the 1000 kg limit, the intermediate result might look like this:

| person_name | weight | cumulative_weight |
|-------------|--------|-------------------|
| Alice       | 250    | 250               |
| Alex        | 350    | 600               |
| John Cena   | 400    | 1000              |

Ordering by `cumulative_weight` in descending order gives:

| person_name | weight | cumulative_weight |
|-------------|--------|-------------------|
| John Cena   | 400    | 1000              |
| Alex        | 350    | 600               |
| Alice       | 250    | 250               |

Finally, `OFFSET 0 ROWS FETCH NEXT 1 ROW ONLY` selects only the first row, which is `John Cena`.

This ensures the query returns the name of the last person who can board the bus without the total weight exceeding the limit.




## 7. [Count Salary Categories](https://leetcode.com/problems/count-salary-categories/)

```sql
/* Write your T-SQL query statement below */

SELECT 'Low Salary' AS category, 
       COUNT(CASE WHEN income < 20000 THEN 1 END) AS accounts_count
FROM Accounts

UNION ALL

SELECT 'Average Salary' AS category, 
       COUNT(CASE WHEN income BETWEEN 20000 AND 50000 THEN 1 END) AS accounts_count
FROM Accounts

UNION ALL

SELECT 'High Salary' AS category, 
       COUNT(CASE WHEN income > 50000 THEN 1 END) AS accounts_count
FROM Accounts;
```