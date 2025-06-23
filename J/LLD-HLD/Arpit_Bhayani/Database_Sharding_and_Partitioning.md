## Sharding
> Method of distributing data across multiple machines.

## Partitioning
> Splitting a subset of data within the same instance.


## How a database is scaled?
> A db server is just a db process (mysql, mongod) running on an EC2 machine.

```
You put your db in production, serving real traffic
```

### API Server Architecture Diagram

```
O ---> API Server ---> [ Database ] ---> 100 WPS
```

* **O**: Represents the user.
* **API Server**: The main server handling the requests.
* **Database**: The database where data is stored.
* **100 WPS**: Indicates a capacity of 100 writes per second (WPS).

> You are getting more users, that your DB is unable to manage.

> You scale up your DB... give it more CPU, RAM, and Disk.

### API Server Architecture Diagram

```
O ---> API Server ---> [ Write Database ] ---> 200 WPS
                 \
                  ---> [ Read Database ]
```

* **O**: Represents the user.
* **API Server**: The main server handling the requests.
* **Write Database**: Handles write operations, optimized for updates and inserts.
* **200 WPS**: Indicates a capacity of 200 writes per second (WPS).
* **Read Database**: Handles read operations, optimized for queries and data retrieval.

> This is vertical scaling.

## Consider a scenario, your product went viral and your bulky database is unable to handl ethe laod, so you scale up again.

### API Server Architecture Diagram

```
O ---> API Server ---> [ Database ] ---> 1000 WPS
```

But, after a certain stage you know you would no tbe able to scale up your db because Vertical scaling has limit. So you will have to resort to Horzontal scaling.

Say, one DB Server was handling 1000 WPS and we cannot scale up beyond that but we are getting 1500 WPS, we scale Horizontally and split the data.


```
O ---> API Server ---> [ Database ] ---> 50% data | 750 WPS
                 \
                  ---> [ Database ] ---> 50% data | 750 WPS
```

By adding one more database server, we reduced the load to 750 WPS on each node and thus handled higher throughput.

Each database server is thus a shard and we say that the data is partitioned.
Overall, a databse is sharded while that data is partitioned.


Over simplification, most people use the terms interchangably.


Suppose you partitioned the 100GB of data into 5 mutually exlusive partitions. Each of thse partitions can either live or one database server or a couple of them can share one server. And this depends on the #shards you have.

```
100 GB Database
       |
   ┌───┴───┬───┬───┬───┐
   │       │   │   │   │
  30GB    10GB 30GB 20GB 10GB
   A       B    C    D    E
   │       │    │    │    │
   └───┬───┘    │    └─┬──┘
       │        │      │
   Shard 2   Shard 1  Shard 2
   (40GB)    (60GB)
```

5 Partitions of our 100GB dataset is distributed across.

## How to partition the data?
There are two categories of partitioning
1. Horizontal Partitioning
2. Vertical Partitioning

When we split the 100 GB data, we could have use either of the ways but deciding which one to pick depends on Load, Usecase, and access pattern.


## Advantages of sharding
- Handle large Reads and Writes.
- Increase overall storage capacity.
- Higher availability


## Disadvantages of Sharding
- Operationally complex
- Cross-shard queries expensive.