# Dynamo
1. cut off strong consistency. Reduce consistency checking
2. not all database operations need the same level guarantees

## To Solve
Cloud Data Services are traditionally oriented around Relational Database systems.

#### Features
+ Clustered 
    - ability to replicate data over multiple servers 
    - providing reliability
+ Highly Available 
    - synchronization & load-balancing features to provide nearly 100% Service Uptime
+ Structured Querying 
    - Allow for complex data models and structured querying

However,

+ The service guarantees of such databases put high overheads on the cloud
+ Complex data models make the cloud more expensive to maintain, update and keep synchronized Load distribution often requires expensive networking equipment
+ To maintain the "elasticity" of the cloud, it requires expensive upgrades to the network

Hence, this could become EXPENSIVE!

To maintain, license and store large amounts of data

## What
meet reliability and scaling needs

Data is partitioned and replicated using consistent hashing 

**consistency** is facilitated by object versioning

Dynamo is a completely decentralized system with minimal need for manual administration.

![](./Summary%20of%20techniques%20used%20in%20Dynamo%20and%20their%20advantages.png)

## System Design & Implementation
### Query Model 
simple key/value interface for storing binary objects identified by unique keys

### Customizable Parameters 
enables service owners to customize storage systems based on performance, durability, and consistency requirements through parameters like N, R, and W

### System Interface
#### get
`get(key)` operation locates the object replicas associated with the key in the storage system and returns a single object or a list of objects with conflicting versions along with a context. 

#### put
`put(key, context, object)` operation determines where the replicas of the object should be placed based on the associated key, and writes the replicas to disk. 

### Data Versioning
manages multiple versions of objects, aiding conflict resolution and ensuring data consistency during failures

### Handling Failures: Hint Handoff
stands for zero-hop Distributed Hash Table (DHT), & ensures fast response times by allowing each node to maintain enough routing information locally to route requests directly

### Handling permanent failures: Replica synchronization
Merkle tree for anti-entropy

## Core Techniques
1. Partitioning: divides data for workload distribution across servers, enhancing storage efficiency and retrieval speed

2. Replication: creates data copies on multiple servers for redundancy, ensuring continuous availability and improved read performance

3. Versioning: supports multiple object versions for conflict resolution and data consistency maintenance

4. Membership and Failure Handling: manages active nodes, handles join/leave events, and responds to failures to sustain system operability

5. Scaling: enables efficient handling of increasing workloads through horizontal (adding servers) and vertical (upgrading servers) scaling techniques, ensuring high availability