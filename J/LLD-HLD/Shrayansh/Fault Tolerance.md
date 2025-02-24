
### **Fault Tolerance:**

**Fault tolerance** is the ability of a system to continue functioning correctly, even when some of its components fail. It ensures reliability and high availability.

### Key Concepts:

1. **Redundancy**:
    
    - Duplicate components (e.g., replicas, backup servers) to take over during failures.
2. **Failover**:
    
    - Automatically switching to a backup component when a failure occurs.
3. **Replication**:
    
    - Data is replicated across nodes to prevent data loss.
4. **Monitoring and Recovery**:
    
    - Detecting failures and restarting or replacing failed components.

### Examples:

1. **Kafka**: Replicated partitions ensure messages are available even if a broker fails.
2. **Cloud Systems**: Load balancers redirect traffic to healthy servers.

### Benefits:

- Increases system reliability and availability.
- Minimizes downtime and impact on users.