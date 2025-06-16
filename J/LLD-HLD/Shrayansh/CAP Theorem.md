
>Desirable property of distributed system with replicated data.

#### C - Consistency
#### A - Availability
#### P - Partition Tolerance

We can't achieve CAP 3s at a time
### **CAP Theorem (Brewer's Theorem)**

The **CAP Theorem** is a fundamental concept in distributed systems introduced by Eric Brewer in 2000. It states that a distributed system can provide at most **two out of three** of the following guarantees simultaneously:

1. **Consistency (C)**:  
    All nodes in the system see the same data at the same time. If a client reads from the system, it always gets the most recent write.
    
2. **Availability (A)**:  
    Every request made to the system receives a response, even if some nodes are down. The system remains operational 100% of the time.
    
3. **Partition Tolerance (P)**:  
    The system continues to operate despite network partitions or communication failures between nodes.
    

---

### **Trade-offs in CAP**

In a distributed system, network partitions can occur, where nodes cannot communicate with each other. When such a partition happens, a system must choose between:

1. **Consistency and Partition Tolerance (CP)**:
    
    - The system sacrifices availability during network partitions to ensure data consistency.
    - Example: **HBase, MongoDB (configurable)**
2. **Availability and Partition Tolerance (AP)**:
    
    - The system sacrifices consistency to remain available during network partitions.
    - Example: **DynamoDB, Cassandra**
3. **Consistency and Availability (CA)**:
    
    - This is achievable only in systems without network partitions, which is unrealistic in distributed systems.
    - Example: **Single-node databases** (not distributed systems).

---

### **Key Points to Understand**

- A **Partition** is inevitable in distributed systems due to potential network failures, making it necessary to choose between **C** and **A** during a partition.
- Real-world distributed systems prioritize either **CP** or **AP** based on their use case.

---

### **Examples of CAP Trade-offs**

|System Type|CAP Focus|Examples|
|---|---|---|
|**CP Systems**|Consistency + Partition Tolerance|HBase, Zookeeper, MongoDB (when configured for CP)|
|**AP Systems**|Availability + Partition Tolerance|Cassandra, DynamoDB, Couchbase|
|**CA Systems**|Consistency + Availability (not truly distributed)|Traditional RDBMS like MySQL, PostgreSQL (single-node setup)|

---

### **Real-World Scenarios**

1. **Banking Systems (CP)**:
    - Consistency is critical to avoid inconsistent account balances.
    - Sacrifice availability during network partitions.
2. **Social Media (AP)**:
    - Availability is prioritized over consistency to allow users to post and interact even during network partitions.
    - Eventual consistency is used.

---

### **CAP Theorem Visualized**

- Imagine a triangle with **Consistency (C)**, **Availability (A)**, and **Partition Tolerance (P)** at each vertex.
- During network partitions, the system can only "pick two" from the three guarantees.

Understanding the CAP Theorem helps in designing distributed systems by making informed trade-offs based on the system's requirements.