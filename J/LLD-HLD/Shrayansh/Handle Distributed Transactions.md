### **Transaction:**  

A **transaction** is a sequence of one or more operations performed as a single, logical unit of work, ensuring data integrity and consistency.  

### **Key Properties (ACID):**  
1. **Atomicity**:  
   - All operations in a transaction are executed completely or not at all.  
   - Example: In a bank transfer, either both debit and credit occur, or neither does.

2. **Consistency**:  
   - Ensures the database remains in a valid state before and after the transaction.  
   - Example: Constraints like foreign keys are maintained.

3. **Isolation**:  
   - Concurrent transactions do not interfere with each other.  
   - Example: One user updating a record doesn't affect another user's read operation.

4. **Durability**:  
   - Once a transaction is committed, the changes are permanent, even in case of a system failure.  
   - Example: Database writes are persisted to disk.

---

### **Types of Transactions:**  
1. **Single-operation Transactions**: Involve only one operation, like inserting a record.  
2. **Multi-operation Transactions**: Involve multiple operations, like updating and deleting records in sequence.

---

### **Example in SQL:**  
```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

### **Benefits:**  
- Maintains data integrity.  
- Prevents partial updates.  
- Facilitates error handling and rollback.


### **Distributed Transaction:**  

A **distributed transaction** is a transaction that spans multiple databases, services, or systems, ensuring ACID properties across all involved resources.

---

### **Key Characteristics:**  
1. **Multi-resource Operations**:  
   - Involves operations on different systems (e.g., multiple databases, message queues).  

2. **Two-Phase Commit (2PC)**:  
   - **Phase 1 (Prepare)**: All participating systems prepare for the transaction and confirm readiness.  
   - **Phase 2 (Commit/Rollback)**: If all systems are ready, the transaction is committed; otherwise, it is rolled back.  

3. **Consistency Across Systems**:  
   - Ensures that all systems reflect the same state after the transaction.  

---

### **Challenges:**  
1. **Network Latency**: Communication between systems may introduce delays.  
2. **Failure Handling**: Ensuring atomicity when one or more systems fail.  
3. **Performance Overhead**: 2PC can be slow and resource-intensive.  

---

### **Alternatives to Distributed Transactions:**  
1. **Eventual Consistency**: Used in distributed systems to relax strict ACID requirements.  
2. **Saga Pattern**: Breaks transactions into a sequence of smaller, local transactions with compensating actions for rollback.

---

### **Example Use Case:**  
- An **e-commerce application** processes a transaction that:  
  - Deducts inventory in a database.  
  - Updates payment status via a payment gateway.  
  - Publishes an order confirmation event to a message queue.  

### **Benefits:**  
- Ensures data consistency across systems.  
- Facilitates complex workflows involving multiple services.




### **Two-Phase Commit (2PC):**  

**2PC** ensures strict consistency for distributed transactions by dividing the process into two phases:

1. **Phase 1: Prepare**  
   - The **Transaction Coordinator** asks all participating systems (databases, services) to prepare for the transaction.  
   - Each system performs preliminary checks and locks resources but doesn’t commit changes yet.  
   - They respond with either:  
     - **"Yes" (ready)**: Indicating the system is ready to commit.  
     - **"No" (failure)**: Indicating the system cannot commit.

2. **Phase 2: Commit or Rollback**  
   - If all participants respond with "Yes," the coordinator sends a **commit** command to finalize the transaction.  
   - If any participant responds with "No," the coordinator sends a **rollback** command, undoing all changes.  

#### **Advantages of 2PC:**  
- Guarantees strict ACID compliance.  
- Ensures atomicity in distributed systems.  

#### **Drawbacks of 2PC:**  
- **Performance Overhead**: High latency due to resource locking during the transaction.  
- **Single Point of Failure**: Transaction Coordinator failure can disrupt progress.  

---

### **Saga Pattern:**  

The **Saga pattern** is an alternative approach for distributed transactions, focusing on **eventual consistency**. It breaks the transaction into smaller, independently managed steps (local transactions).

1. **Choreography-Based Saga**:  
   - Each step publishes events that trigger the next step.  
   - If a failure occurs, compensating transactions are triggered by other services.  
   - No central coordinator is used.  
   - **Example**:  
     - Step 1: Deduct inventory → publish event.  
     - Step 2: Process payment → publish event.  
     - Step 3: Update order status.

2. **Orchestration-Based Saga**:  
   - A **central orchestrator** coordinates all steps of the transaction.  
   - Orchestrator handles failures by invoking compensating actions (e.g., refund payment, restock inventory).  

#### **Advantages of Saga:**  
- **Better Scalability**: Each step is asynchronous and independent.  
- **No Resource Locking**: Improves performance by avoiding locks.  

#### **Drawbacks of Saga:**  
- **Complexity**: Requires careful design of compensating transactions.  
- **Eventual Consistency**: Does not guarantee strict consistency; intermediate states may be visible.

---

### **Comparison of 2PC vs. Saga:**  

| **Aspect**             | **2PC**                          | **Saga**                         |
|-------------------------|-----------------------------------|-----------------------------------|
| **Consistency**         | Strict (ACID)                   | Eventual Consistency             |
| **Performance**         | Slower due to locks and overhead | Faster and scalable              |
| **Complexity**          | Simpler to implement logic       | Complex due to compensating steps|
| **Fault Tolerance**     | Single Point of Failure (Coordinator) | More fault-tolerant (Choreography) |
| **Use Case**            | Financial transactions, critical operations | Microservices, high-performance apps |

### **Example Use Case**  
- **2PC**: Bank transfer between accounts in two different banks.  
- **Saga**: Booking a travel package where flight, hotel, and car reservations can fail independently and require compensation.
