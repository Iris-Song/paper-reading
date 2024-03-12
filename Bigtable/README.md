# [Bigtable](https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)
#### key: indexing, doing fast search


Bigtable is a **distributed storage system** for managing structured data that is designed to scale to a very large size

Bigtable resembles a database: it shares many implementation strategies with databases. Parallel databases and main-memory databases have achieved scalability and high performance, but Bigtable provides a different interface than such systems. Bigtable does not support a full relational data model; instead, it provides clients with a **simple data model** that supports dynamic control over data layout and format, and allows clients to reason about the locality properties of the data represented in the underlying storage.

Bigtable treats data as uninterpreted strings

Bigtable schema parameters let clients dynamically control whether to serve data out of memory or from disk.

## Data Model
A Bigtable is a sparse, distributed, persistent multi-dimensional sorted map. The map is indexed by a row key, column key, and a timestamp; each value in the map is an uninterpreted array of bytes.
```
(row:string, column:string, time:int64) → string
```
NoSQL

## API
Bigtable supports single-row transactions(Atomic transactions), which can be used to perform atomic read-modify-write sequences on data stored under a single row key.

Bigtable allows cells to be used as integer counters.

Bigtable supports the execution of client-supplied scripts in the address spaces of the servers.

## Build Blocks
Bigtable uses the **distributed Google File System (GFS)** to store log and data files. 

The Google ***SSTable*** file format is used internally to store Bigtable data. An SSTable provides a persistent, ordered immutable map from keys to values, where both keys and values are arbitrary byte strings.
A lookup can be performed with a single disk seek: we first find the appropriate block by performing a binary search in the in-memory index, and then reading the appropriate block from disk. Optionally, an SSTable can be completely mapped into memory, which allows us to perform lookups and scans without touching disk.

Bigtable relies on a highly-available and persistent distributed lock service called **Chubby**. A Chubby service consists of five active replicas, one of which is elected to be the master and actively serve requests. The service is live when a majority of the replicas are running and can communicate with each other. Chubby uses the Paxos algorithm to keep its replicas consistent in the face of failure.

## Implementation
three major components: 
1. a library that is linked into every client
2. one master server
3. many tablet servers. 

![](./Tablet%20location%20hierarchy.png)
![](./Tablet%20Representation.png)

### Why MemTable and STable?
+ Write Performance (MemTable)
+ Reduced Disk 1/O (SSTable)
+ Compression of SSTable into multiple SSTable

### refinement made by Google
1. Compression
(Tablet Representation)
2. Caching for read performance
3. Bloom filters
4. seperate locality group
5. handling commit logs