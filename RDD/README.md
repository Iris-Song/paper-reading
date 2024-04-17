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