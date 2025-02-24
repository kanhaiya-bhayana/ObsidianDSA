## What is Caching?

### **Caching:**

**Caching** is a mechanism to store frequently accessed data in a temporary storage layer (cache) for faster retrieval.

### Key Points:

1. **Purpose**:
    
    - Improves application performance by reducing data access time.
    - Reduces the load on databases or backend systems.
2. **Types of Caches**:
    
    - **In-memory Cache**: Stores data in memory (e.g., Redis, Memcached).
    - **Disk Cache**: Stores data on disk for persistence.
    - **Browser Cache**: Stores web resources (e.g., images, CSS) locally on a user’s device.
3. **Cache Strategies**:
    
    - **Write-through**: Updates cache and database simultaneously.
    - **Write-back**: Updates the cache first, and the database later.
    - **Read-through**: Cache fetches data from the database if not present.
4. **Eviction Policies**:
    
    - Determines when to remove old data (e.g., LRU - Least Recently Used).

### Example:

- Storing frequently queried data (e.g., user profiles) in Redis to reduce database access latency.

### Benefits:

- Faster response times.
- Reduces load on underlying systems.
- Enhances scalability.
## What is Distributed Caching and how its done?

### **Distributed Caching:**

**Distributed caching** is a caching mechanism where the cache is spread across multiple nodes in a distributed system, ensuring scalability, fault tolerance, and high availability.

### **How Distributed Caching Works:**

1. **Data Partitioning**:
    
    - The cache data is divided and stored across multiple nodes using partitioning strategies like consistent hashing.
2. **Replication**:
    
    - Cached data is replicated across nodes for fault tolerance and high availability.
3. **Cache Coordination**:
    
    - A central coordinator (or peer-to-peer communication) ensures consistency and manages cache operations across nodes.
4. **Distributed Access**:
    
    - Applications access the cache via a client library or API, which routes requests to the appropriate node.

### **Key Components:**

1. **Nodes**: Individual servers in the cache cluster.
2. **Coordinator**: Optional central component for managing nodes (e.g., in Redis Cluster).
3. **Client**: Application interface for interacting with the cache.

### **Technologies for Distributed Caching:**

- **Redis Cluster**: In-memory distributed cache with partitioning and replication.
- **Memcached**: Simple, lightweight distributed caching.
- **Hazelcast/Apache Ignite**: Distributed data grids with caching capabilities.

### **Benefits:**

- **Scalability**: Easily add nodes to handle more data or traffic.
- **Fault Tolerance**: Replication ensures data availability during node failures.
- **High Performance**: Distributed caching reduces latency and load on primary data sources.

### **Example Use Case:**

- A web application uses Redis Cluster to cache session data across nodes. If one node goes down, the replicated data on another node ensures uninterrupted service.
## Caching Strategy?
	 Cache aside - normally we handle the cache using cache hit and cache miss.
	 Read-Through - Db structure and cache structure must be same, rest is same as `cache aside`
	Write Around - whenwever there will be a write/update, it will mark the cahce as flag dirty, i,e., the has been updated, get from the DB
	Write Through - First writes data into the Cache, then in synchronous writes data into the db - 2PhaseCommit
	Write Back - First writes data into the Cache, then in asynchronous writes data into the queue, then from queue to DB (ASYNCHRONOUSLY)

### **Caching Strategies:**

Caching strategies define how data is cached, retrieved, and invalidated to ensure optimal performance and consistency.

---

### **1. Cache Population Strategies:**

- **Lazy Loading (Read-Through)**:
    
    - Data is loaded into the cache only when requested and not found in the cache.
    - **Pros**: Saves memory by caching only frequently accessed data.
    - **Cons**: Higher latency for the first request (cache miss).
    - **Example**: User profile loaded into cache on first request.
- **Write-Through**:
    
    - Data is written to the cache and database simultaneously.
    - **Pros**: Ensures cache consistency with the database.
    - **Cons**: Slower write operations due to dual writes.
    - **Example**: User settings updated in cache and DB.
- **Write-Around**:
    
    - Data is written directly to the database but not to the cache.
    - **Pros**: Avoids caching rarely accessed data.
    - **Cons**: Cache may have stale data until explicitly invalidated.
- **Write-Behind (Write-Back)**:
    
    - Data is written to the cache first and asynchronously updated in the database.
    - **Pros**: Fast write operations.
    - **Cons**: Risk of data loss if the cache fails.

---

### **2. Cache Invalidation Strategies:**

- **Time-to-Live (TTL):**
    
    - Cached data expires after a specified time.
    - **Example**: Product prices cached for 1 hour.
- **Manual Invalidation:**
    
    - Data is explicitly removed or updated in the cache.
    - **Example**: Clearing session data on logout.
- **Write-triggered Invalidation:**
    
    - Cache entries are updated or removed whenever the database is updated.

---

### **3. Cache Placement Strategies:**

- **Global Cache**: Shared by all application instances (e.g., distributed caching).
- **Local Cache**: Specific to an application instance, faster but less consistent.

---

### **Best Practices:**

- Choose eviction policies like **LRU (Least Recently Used)** or **LFU (Least Frequently Used)**.
- Monitor cache hit/miss rates to fine-tune strategies.
- Use distributed caching for scalability (e.g., Redis, Memcached).

### **Example Use Case:**

- E-commerce: Cache product details using **lazy loading** with a **TTL** to refresh prices periodically.
## Cache Eviction Policies?
	LRU
	FIFO
	LFU etc.,