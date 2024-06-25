###### Retrieve only the non matching rows from the left table
###### Retrieve only the non matching rows from the right table
###### Retrieve only the non matching rows from both the left and right table


## Retrieve only the non matching rows from the left table

```sql
SELECT    t1.Name, t1.Gender, t1.Salary, t2.DepartmentName 
FROM      tblEmployee1 AS t1 
LEFT JOIN tblDepartment AS t2 
ON        t1.DepartmentId = t2.Id 
WHERE     t1.DepartmentId IS NULL;
```

## Retrieve only the non matching rows from the right table

```sql
SELECT     t1.Name, t1.Gender, t1.Salary, t2.DepartmentName 
FROM       tblEmployee1 AS t1 
RIGHT JOIN tblDepartment AS t2 
ON         t1.DepartmentId = t2.Id 
WHERE      t1.DepartmentId IS NULL;
```

## Retrieve only the non matching rows from both the left and right table

```sql
SELECT      t1.Name, t1.Gender, t1.Salary, t2.DepartmentName 
FROM        tblEmployee1 AS t1 
FULL JOIN   tblDepartment AS t2 
ON          t1.DepartmentId = t2.Id 
WHERE       t1.DepartmentId IS NULL 
			OR 
			t2.Id IS NULL;
```

## Self Join

- A self join is a type of join in SQL where a table is joined with itself. 
- This is particularly useful when you need to compare rows within the same table or retrieve hierarchical data.
- A self join can be used to find relationships among records in the same table.

### Key Points of a Self Join:

1. **Table Aliases**: To distinguish the table from itself, table aliases are used. Aliases give the same table different names within the query.
2. **Comparison Within the Same Table**: A self join is useful for comparing rows within the same table. For example, finding employees and their managers within an employee table.
3. **Hierarchical Data**: Self joins are often used to manage hierarchical data structures like organizational charts or family trees.

### Example Scenario:

Suppose you have an `employees` table with the following structure:

| employee_id | name       | manager_id |
|-------------|------------|------------|
| 1           | Alice      | NULL       |
| 2           | Bob        | 1          |
| 3           | Charlie    | 2          |
| 4           | David      | 2          |

Here, `manager_id` is a foreign key that refers to `employee_id` in the same table. To find out each employee and their corresponding manager, you would perform a self join.

### SQL Query Example:

```sql
SELECT 
    e1.employee_id AS EmployeeID, 
    e1.name AS EmployeeName, 
    e2.employee_id AS ManagerID, 
    e2.name AS ManagerName
FROM 
    employees e1
LEFT JOIN 
    employees e2
ON 
    e1.manager_id = e2.employee_id;
```

### Explanation:

- `employees e1` and `employees e2` are aliases for the same table.
- The `LEFT JOIN` is performed on `e1.manager_id = e2.employee_id`, linking each employee to their manager.
- The result will show each employee alongside their manager, even if some employees do not have a manager (`NULL`).

### Result:

| EmployeeID | EmployeeName | ManagerID | ManagerName |
|------------|--------------|-----------|-------------|
| 1          | Alice        | NULL      | NULL        |
| 2          | Bob          | 1         | Alice       |
| 3          | Charlie      | 2         | Bob         |
| 4          | David        | 2         | Bob         |

This is a simple example of how a self join can be used to relate rows within the same table, illustrating relationships such as employee-manager hierarchies.


#### Self join can be classified as:

- Inner Self Join
- Outer Self Join (Left, Right and Full)
- Cross Self Join


#### Left Self Join

```sql
SELECT     E.Name AS Employeename, M.Name AS ManagerName 
FROM       tblEmployee2 E 
Left JOIN  tblEmployee2 M 
ON         E.ManagerID = M.EmployeeID
```
#### Inner Self Join

```sql
SELECT     E.Name AS Employeename, M.Name AS ManagerName 
FROM       tblEmployee2 E 
INNER JOIN tblEmployee2 M 
ON         E.ManagerID = M.EmployeeID
```
#### Cross Self Join
```sql
SELECT       E.Name AS Employeename, M.Name AS ManagerName 
FROM         tblEmployee2 E 
CROSS JOIN   tblEmployee2 M
```

