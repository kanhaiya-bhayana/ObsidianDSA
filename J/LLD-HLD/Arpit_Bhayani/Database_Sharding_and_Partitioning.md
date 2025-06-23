# 📦 Database Scaling: Sharding & Partitioning

---
## 🧩 Sharding vs Partitioning
### Sharding

> **Sharding** is a method of distributing data across multiple **independent database servers** (called _shards_), where each shard holds a portion of the dataset.
### Partitioning

> **Partitioning** is the process of dividing a dataset into smaller **logical segments** (partitions), which may be stored on the same or different servers.

> 🔹 **Sharding = Horizontal scaling** using partitioning  
> 🔹 **Partitioning** = Logical data split, can exist without sharding

---
## 🧱 How is a Database Scaled?

A database server is just a process (e.g., `mysqld`, `mongod`) running on a machine (e.g., EC2 instance).
You start with a single database instance:

```
O ---> API Server ---> [ Database ] ---> 100 WPS
```

- **O**: End-user sending a request
- **API Server**: Application layer
- **Database**: Central DB handling all reads/writes
- **100 WPS**: Database can handle 100 Writes Per Second

---
### 🔼 Vertical Scaling (Scale-Up)

As demand increases, you scale **vertically** by giving the DB more resources: CPU, RAM, disk.

```
O ---> API Server ---> [ Write DB ] ---> 200 WPS
                 \
                  ---> [ Read DB ]
```

- **Write DB**: Handles updates/inserts/deletes
- **Read DB**: Read replicas to serve read-heavy traffic
- **200 WPS**: Improved write throughput after scaling

> ✅ Easier to implement  
> ❌ Has hardware and cost limitations

---
## 🚨 When Vertical Scaling Hits Its Limit

Eventually, vertical scaling is not enough.

**Scenario**:  
Your product goes viral.  
Database can handle max 1000 WPS, but traffic hits 1500 WPS.

You now scale **horizontally**.

---
### ⚖️ Horizontal Scaling with Sharding

You split data across multiple DB instances (shards):

```
O ---> API Server ---> [ DB Shard A ] ---> 50% data | 750 WPS
                 \
                  ---> [ DB Shard B ] ---> 50% data | 750 WPS
```

- Traffic and data split across shards
- Application or routing layer decides which shard to talk to

> ✅ Linearly increases throughput  
> ✅ Enables massive scaling  
> ❌ Introduces operational complexity

---
## 🧮 Example: Partitioning a 100 GB Dataset

Let's split a 100 GB DB into 5 partitions:

```
100 GB Total Data
       |
   ┌───┴───┬─────┬─────┬─────┐
   │       │     │     │     │
  30GB    10GB  30GB  20GB  10GB
   A       B     C     D     E
   │       │     │     │     │
   └───┬───┘     │     └──┬──┘
       │         │        │
   Shard 2     Shard 1   Shard 2
   (40GB)      (60GB)
```

- **Partitions**: A–E (logical data splits)
- **Shards**: DB servers hosting one or more partitions
- **Load Distribution**: Partitions grouped onto shards for balance

---
## 📂 Types of Partitioning
### 1. Horizontal Partitioning

> Split by **rows** — each partition has the same schema, different data subsets  
> _e.g.,_ Users 1–1000 in Partition A, 1001–2000 in Partition B
### 2. Vertical Partitioning

> Split by **columns** — each partition has a subset of the schema  
> _e.g.,_ User profile data in one table, settings in another

> 🧠 Choose based on access patterns, query types, and data distribution

---
## ✅ Advantages of Sharding

- 🚀 **Scalability**: Supports higher throughput and storage needs
- ⚡ **Performance**: Reduced latency and load per DB
- 💾 **Storage**: Easily scale out by adding shards
- ⚙️ **Parallelism**: Concurrent operations on multiple DBs
- 🛡️ **Isolation**: Failures in one shard won’t affect others

---
## ⚠️ Disadvantages of Sharding

- 🔧 **Operational Overhead**: More infrastructure to manage
- 🔁 **Rebalancing**: Adding/removing shards requires redistributing data
- 🔍 **Cross-Shard Queries**: Hard to join data across shards efficiently
- 🧩 **Complexity**: App logic or middleware must handle routing logic

---
## 📝 Summary

|Concept|Description|
|---|---|
|**Sharding**|Distributing data across DB servers (horizontal scaling)|
|**Partitioning**|Dividing data into logical segments (used in sharding or standalone)|
|**Vertical Scaling**|Adding more power to one server (limited by hardware)|
|**Horizontal Scaling**|Adding more servers and distributing data across them|
