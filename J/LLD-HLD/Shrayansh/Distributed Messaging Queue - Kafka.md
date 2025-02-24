
![[Kafka architecture.png]]

## 1. What is Messaging Queue? Why is it needed and its advantages?

### Messaging Queue:

A **messaging queue** is a software component that enables asynchronous communication between different components of a system by sending and receiving messages.

### Why it’s needed:

- Decouples system components for scalability and flexibility.
- Handles message buffering to ensure reliability in communication.
- Allows different services to process tasks at their own pace.
## 2. What is Point 2 Point & Pub|Sub Messaging types?

### 1. **Point-to-Point (P2P):**

- **Definition:** A messaging model where messages are sent to a specific queue and consumed by a single consumer.
- **Use Case:** Task queues (e.g., processing orders).
- **Example:** A message in the queue is consumed by one worker only.

### 2. **Publish-Subscribe (Pub/Sub):**

- **Definition:** A messaging model where messages are published to a topic and delivered to all subscribed consumers.
- **Use Case:** Event broadcasting (e.g., notifications, logs).
- **Example:** A news topic sends updates to all subscribers simultaneously.
## 3. How Messaging Queue works?
### How Messaging Queue Works:

1. **Producer**: Sends messages to the queue.
2. **Queue**: Acts as a buffer that holds messages until they are processed.
3. **Broker**: Manages the queue and ensures message delivery (e.g., RabbitMQ, Kafka).
4. **Consumer**: Retrieves and processes messages from the queue.
5. **Acknowledgement**: Consumer sends an acknowledgment back to the queue after processing, ensuring reliable delivery.

This flow ensures asynchronous, reliable, and decoupled communication between components.

### Kafka Components:

1. **Producer**: Sends messages to Kafka topics.
2. **Topic**: A category where messages are stored and organized.
3. **Partition**: Topics are divided into partitions for scalability and parallel processing.
4. **Broker**: A Kafka server that stores messages and serves requests.
5. **Consumer**: Reads messages from topics and processes them.
6. **Consumer Group**: A set of consumers that collaboratively consume messages from a topic.
7. **Zookeeper** (or **Kafka Controller**): Manages metadata, leader elections, and cluster state (being replaced by KRaft).

### Offset
An **offset** is a unique identifier assigned to each message within a Kafka partition. It represents the sequential position of the message.

### Key Points:

- **Purpose**: Tracks the consumer's read position in a partition.
- **Managed By**: Consumers store offsets to resume processing in case of failure.
- **Use Case**: Ensures messages are processed exactly-once or at-least-once, depending on the configuration.

Example: If a consumer's offset is 5, it has processed messages 0–4 and will read the next message (offset 5).

### What is Leader and follower?
### **Leader and Follower in Kafka:**

1. **Leader**:
    
    - Handles all read and write requests for a specific partition.
    - Ensures high throughput by being the primary point of interaction.
2. **Follower**:
    
    - Replicates the leader's data for fault tolerance.
    - Takes over as the leader if the current leader fails (via election).

### Key Points:

- **Replication**: Ensures data durability and availability.
- **Failover**: Follower becomes the new leader during leader failure.
- **Example**: A topic with 3 partitions can have 1 leader per partition and 2 followers (replication factor = 3).

#### What does this mean? 
##### Example: A topic with 3 partitions can have 1 leader per partition and 2 followers (replication factor = 3).

In Kafka, **replication factor** defines how many copies of a partition are maintained across brokers for fault tolerance.

### Explanation of the Example:

- **3 Partitions**: The topic is divided into 3 partitions (P1, P2, P3).
- **Replication Factor = 3**: Each partition has 3 replicas:
    - **1 Leader**: Actively handles reads/writes.
    - **2 Followers**: Passive replicas that sync data from the leader.

### Scenario:

- Partition **P1**: Broker A is the leader, and Brokers B & C are followers.
- Partition **P2**: Broker B is the leader, and Brokers A & C are followers.
- Partition **P3**: Broker C is the leader, and Brokers A & B are followers.

This ensures high availability because if the leader fails, one of the followers can take over.
## 4. What happen when Queue size limit is reached?

We can add different brokers

When the **queue size limit** is reached:

1. **New Messages Rejected**: Producers are blocked or receive an error while sending new messages.
2. **Messages Dropped**: Some brokers discard the oldest or lowest-priority messages based on configuration.
3. **Performance Degradation**: Queue operations may slow down, impacting the system.
4. **Backpressure**: Producers may need to handle the situation by reducing message rates or buffering messages themselves.

### Mitigation Strategies:

- Increase queue size.
- Implement backpressure handling in producers.
- Use message retention policies (e.g., TTL).
- Scale out consumers to process faster.
### **Scale Out and Scale In**:

1. **Scale Out**:
    
    - Adding more instances or resources to a system to handle increased load.
    - Example: Adding more Kafka brokers, consumers, or partitions.
    - Use Case: Improving throughput or accommodating a growing number of users.
2. **Scale In**:
    
    - Reducing the number of instances or resources when the load decreases.
    - Example: Shutting down excess consumer instances during low traffic.
    - Use Case: Optimizing resource utilization and reducing costs.

Both approaches are part of horizontal scaling.
## 5. What happens to messages, when Queue goes down?

Follower can take over...,

Yes, if the **queue system** supports replication and has a **leader-follower architecture** (e.g., Kafka), a **follower can take over** in case of failure.

### How It Works:

1. **Replication**: Messages are replicated from the leader to followers.
2. **Failover**:
    - When the leader (queue) goes down, a follower is elected as the new leader.
    - The new leader starts handling read/write operations.

### Example in Kafka:

- If the leader for a partition fails, one of its followers takes over as the leader, ensuring no data loss or service disruption (provided all replicas are in-sync).

### Key Requirement:

Replication must be configured, and followers should be in-sync with the leader.
## 6. What happens when Consumer goes down?
When a **consumer goes down**:

1. **Unprocessed Messages**: Messages remain in the topic/partition.
2. **Rebalancing**:
    - If the consumer is part of a **consumer group**, Kafka triggers a **rebalance**.
    - Remaining consumers in the group take over the partitions previously assigned to the failed consumer.
3. **Offset Retention**:
    - The consumer's last committed offset is saved.
    - When it restarts, it resumes reading from the last committed offset.
4. **Impact**: Temporary delay in message processing until rebalance is complete or the consumer recovers.

## 7. What happen when Consumer not able to process messages?
When a **consumer is unable to process messages**, the following happens:

### 1. **Message Retention in the Queue**:

- Messages remain in the queue until processed or their time-to-live (TTL) expires (if configured).

### 2. **Retry Mechanism**:

- **Automatic Retries**: Many systems (e.g., Kafka, RabbitMQ) retry delivery if the consumer fails to acknowledge the message.
- **Dead Letter Queue (DLQ)**: Failed messages can be sent to a DLQ for later inspection or reprocessing.

### 3. **Backpressure**:

- The queue may fill up if the consumer cannot process messages quickly enough, potentially causing system bottlenecks.

### 4. **Timeouts and Errors**:

- If the consumer crashes or exceeds processing time, the queue may reassign the message to another consumer.

### Mitigation Strategies:

- Implement **retry logic** or configure **DLQs**.
- Optimize consumer performance or scale out consumers.
- Monitor and handle **exception cases** effectively.
## 8. How Retry works? Different ways to retry?

Go to 7
## 9. How Distributed Messaging Queue Works?

A **distributed messaging queue** operates across multiple nodes or servers to ensure scalability, fault tolerance, and high availability.

### Key Components and Workflow:

1. **Producers**:
    
    - Send messages to the messaging queue (cluster).
    - Messages are distributed across nodes (brokers) based on partitions, topics, or load-balancing strategies.
2. **Partitioning**:
    
    - Messages are divided into **partitions** for parallel processing.
    - Each partition is assigned a **leader** (active node) and **followers** (replica nodes).
3. **Replication**:
    
    - Messages are replicated across multiple nodes to ensure fault tolerance.
    - If a node fails, replicas on other nodes ensure continuity.
4. **Consumers**:
    
    - Consumers pull messages from the partitions assigned to them.
    - In **consumer groups**, each partition is processed by one consumer within the group.
5. **Message Offset**:
    
    - Offsets track the processing status of each message.
    - Consumers resume processing from the last committed offset after a failure.
6. **Failover and Leader Election**:
    
    - If a leader node fails, a follower becomes the new leader through an election process.

### Advantages:

- **Scalability**: Handles high throughput by distributing load.
- **Fault Tolerance**: Ensures reliability with replication and failover mechanisms.
- **Decoupling**: Enables independent scaling of producers and consumers.

### Example: Kafka

- Kafka distributes topics into partitions across brokers.
- Each broker handles a subset of partitions as leaders, while replicas are maintained on other brokers.
- Consumers in a group process messages from assigned partitions.
## 10. What is Dead letter Queue?

### **Dead Letter Queue (DLQ):**

A **Dead Letter Queue** is a special queue used to store messages that cannot be processed successfully by the consumer or message broker.

### Key Features:

1. **Purpose**:
    
    - To capture and isolate problematic or unprocessable messages for further analysis or reprocessing.
2. **Triggered When**:
    
    - Message delivery exceeds retry limits.
    - Message format or content is invalid.
    - Consumer logic fails repeatedly.
    - TTL (Time-to-Live) for a message expires.
3. **Separate Queue**:
    
    - DLQ is independent of the main queue to prevent blocking or overflowing.

### Benefits:

- Prevents message loss.
- Facilitates debugging and error analysis.
- Helps maintain system reliability and throughput.

### Example:

In Kafka, a DLQ can be implemented by producing failed messages to a specific topic (e.g., "dead-letter-topic") for later inspection.