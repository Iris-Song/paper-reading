# Dynamo
meet reliability and scaling needs

Data is partitioned and replicated using consistent hashing 

consistency is facilitated by object versioning

Dynamo is a completely decentralized system with minimal need for manual administration.

![](./Summary%20of%20techniques%20used%20in%20Dynamo%20and%20their%20advantages.png)

### System Interface
#### get
`get(key)` operation locates the object replicas associated with the key in the storage system and returns a single object or a list of objects with conflicting versions along with a context. 

#### put
`put(key, context, object)` operation determines where the replicas of the object should be placed based on the associated key, and writes the replicas to disk. 

### Partitioning Algorithm
virtual nodes

### Replication

### Data Versioning

### Handling Failures: Hint Handoff

### Handling permanent failures: Replica synchronization
Merkle tree for anti-entropy