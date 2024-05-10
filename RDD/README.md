# Resilient Distributed Datasets (RDDs)
a distributed memory abstraction that lets programmers perform in-memory computations on large clusters in a fault-tolerant manner

based on coarse grained transformations rather than fine-grained updates to shared state

## RDD Abstraction
an RDD is a read-only, partitioned collection
of records.
#### transformations
RDDs can only be created through deterministic
operations on either (1) data in stable storage or (2) other RDDs. these operations are called transformations.

e.g. map, filter, and join.

### actions
e.g. count, save, collect

### persist
indicate which RDDs they want to reuse in future operations

Spark keeps persistent RDDs in memory by de
fault, but it can spill them to disk if there is not enough RAM

RDDs do not need to be materialized at all times. Instead, an RDD has enough information about how it was derived from other datasets (its lineage) to compute its partitions from data in stable storage.

users can control two other aspects of RDDs:
persistence and partitioning.

e.g. count,select,save

![](./Transformations%20and%20actions.png)

## Key Features
Fault Tolerance: RDDs ensure fault tolerance via lineage, tracking applied transformations to reconstruct lost partitions by re-executing them.

Immutability: Once formed, their content remains unchanged. Transformations generate new RDDs, upholding data integrity and consistency during computation.

Partitioning: RDD partitioning divides data into smaller distributed chunks across cluster nodes, enabling parallel processing for improved efficiency and scalability.

In-Memory Computation: In-memory computation in RDDs stores data within cluster node memory, facilitating faster access and processing than disk-based methods.

Lazy Evaluation: In RDDs, the transformations are not computed immediately; they are postponed until an action is invoked, optimizing performance by allowing Spark to optimize execution plans and minimize unnecessary computations.

Persistence: It involves caching intermediate results in memory, reducing the need for recomputation and enhancing performance by allowing reuse of computed data across multiple actions.

Coarse-grained operations: It involves applying transformations on entire datasets, resulting in large-scale data processing across distributed nodes, optimizing performance and parallelism in computation.

## COMPARISON WITH DSM
**RDD**

- Only coarse-grained transformations can create or "write" RDDs.
- Trivial consistency due to immutability.
- Fine-grained and low-overhead fault recovery using lineage, avoiding the need for explicit checkpoints and program rollbacks.
- Possible through backup tasks, enabling the system to run redundant copies of slow tasks.
- Work placement is automatic based on data locality, improving performance by scheduling tasks closer to the data.

**DSM (Distributed Shared Memory)**

- Allows fine-grained reads and writes to arbitrary memory locations within a global address space.
- Consistency varies and can be managed by the application or runtime.
- Typically requires checkpoints and potentially program rollback upon failure.
- Straggler mitigation is difficult due to interference between multiple copies of tasks accessing the same memory locations.
- Work placement is often left to the application, although runtimes may aim for transparency.

## COMPARISON WITH HADOOP
1. Performance and Efficiency: Spark's ability to store data in-memory reduces I/O and deserialization costs.
2. Fault Tolerance: Spark quickly recovers lost RDD partitions after a node failure, by reconstructing only the necessary partitions through lineage.
3. Interactive Data Processing: Supports interactive data querying with significantly lower latencies (5-7 seconds) when working with large datasets.
4. Application Suitability: Spark, through its RDDs, is highly effective for a wide range of applications, from traffic modeling and spam classification to large-scale data analytics and machine learning.

## Advantages of RDD
1. **Efficient Fault Recovery:** RDDs utilize lineage for fault tolerance, eliminating the need for costly checkpointing. Lost partitions are recomputed upon failure, minimizing overhead and ensuring system robustness.

2. **Straggler Handling:** Immutable nature of RDDs allows for easy management of slow nodes (stragglers) by running backup copies of tasks, enhancing system reliability and performance.

3. **Data Locality Optimization:** RDDs enable efficient task scheduling based on data locality, reducing data movement across nodes and improving overall performance in distributed computing environments.

4. **Graceful Degradation:** RDDs seamlessly handle memory constraints by storing oversized partitions on disk, ensuring continuous operation and maintaining performance levels even with limited memory resources.

5. **Simplified Programming:** RDDs offer a straightforward programming model with coarse-grained transformations, simplifying application development and deployment on commodity clusters while preserving fault tolerance and performance.

## Limitations of RDD
1. Suited for Bulk Writes Only: Designed primarily for applications that execute bulk operations, not suitable for fine-grained updates.
2. RDDs and Dynamic Update Challenges: Resilient Distributed Datasets, while powerful for batch processing and iterative computations, show limitations in scenarios that demand dynamic, fine-grained updates to shared states. This includes environments where data modifications happen asynchronously and in real-time, such as web storage systems and incremental web crawlers.

## Q
### benefit of lazy evaluation
In RDDs, the transformations are not computed immediately; they are postponed until an action is invoked, optimizing performance by allowing Spark to optimize execution plans and minimize unnecessary computations.

The key difference between RDD (Resilient Distributed Dataset) and DSM (Distributed Shared Memory) lies in their data abstraction and memory management:

### What is the key difference between RDD and DSM? What are the pros/cons of this key difference?
1. **Data Abstraction**: RDD represents data as a collection of immutable distributed elements, allowing parallel processing through transformations and actions. DSM, on the other hand, presents a shared memory abstraction across distributed nodes, enabling processes to directly access and modify shared data.

2. **Memory Management**: RDD manages data persistence through lineage information, enabling fault tolerance and lazy evaluation. DSM maintains a shared memory space across distributed nodes, necessitating coherence protocols to synchronize memory accesses and consistency models to ensure data integrity.

Pros and cons:

- **RDD Pros**:
  - Fault tolerance through lineage information.
  - Lazy evaluation for optimized processing.
  - Suitable for iterative algorithms and batch processing tasks.

- **RDD Cons**:
  - Overhead of lineage information maintenance.
  - Limited to data parallelism, less suitable for shared-memory-like operations.

- **DSM Pros**:
  - Provides a shared-memory abstraction for simplified programming models.
  - Allows direct access and modification of shared data, facilitating fine-grained synchronization.
  - Suitable for tasks requiring shared-state communication and coordination.

- **DSM Cons**:
  - Complexity in managing coherence protocols for consistency.
  - Increased risk of data inconsistency and race conditions.
  - Limited scalability due to contention in shared memory accesses.