### **Difference Between Function and Stored Procedure**

| **Feature**          | **Function**                                             | **Stored Procedure**                                                                      |
| -------------------- | -------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Purpose**          | Used to perform computations and return a value.         | Used to execute a series of statements or perform a specific task.                        |
| **Return Type**      | Must return a value (scalar, table, or a result set).    | May return nothing, a single value, or multiple result sets.                              |
| **Execution**        | Can be called within a query (e.g., `SELECT`).           | Invoked using the `EXEC` or `CALL` statement.                                             |
| **Input Parameters** | Only input parameters allowed.                           | Can have input, output, or both types of parameters.                                      |
| **Use in Queries**   | Can be used in `SELECT`, `WHERE`, or `JOIN`.             | Cannot be directly used in `SELECT` or `JOIN`.                                            |
| **Transactions**     | Cannot manage transactions (e.g., `COMMIT`, `ROLLBACK`). | Can manage transactions, including `BEGIN TRANSACTION`, `COMMIT`, and `ROLLBACK`.         |
| **Side Effects**     | Cannot modify database state (read-only).                | Can modify database state (e.g., `INSERT`, `UPDATE`, `DELETE`).                           |
| **Error Handling**   | Limited error handling using `TRY...CATCH`.              | Supports robust error handling mechanisms.                                                |
| **Performance**      | Generally faster for lightweight calculations.           | More suited for complex operations and bulk processing.                                   |
| **Compilation**      | Compiled every time it is invoked.                       | Precompiled and stored in the database, leading to faster execution for repetitive tasks. |
| **Calling Location** | Can be called in SQL queries, triggers, and procedures.  | Cannot be used directly in queries but can call functions.                                |

---

### **Example: Function**

```sql
CREATE FUNCTION GetTotalPrice(@Quantity INT, @Price DECIMAL(10, 2))
RETURNS DECIMAL(10, 2)
AS
BEGIN
    RETURN @Quantity * @Price;
END;
```

**Usage:**

```sql
SELECT dbo.GetTotalPrice(5, 100.00) AS TotalPrice;
```

---

### **Example: Stored Procedure**

```sql
CREATE PROCEDURE InsertOrder
    @OrderID INT,
    @CustomerName NVARCHAR(100),
    @OrderDate DATE
AS
BEGIN
    INSERT INTO Orders (OrderID, CustomerName, OrderDate)
    VALUES (@OrderID, @CustomerName, @OrderDate);
END;
```

**Usage:**

```sql
EXEC InsertOrder @OrderID = 1, @CustomerName = 'John Doe', @OrderDate = '2025-06-30';
```

---

### **When to Use Which?**

* Use **functions** when you need to perform reusable computations and return results without modifying the database state.
* Use **stored procedures** for complex business logic, managing transactions, or performing database modifications.
