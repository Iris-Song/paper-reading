# RocksDB
RocksDB is an embedded, high-performance, persistent key-value storage engine developed at Facebook.

optimize space efficiency while ensuring read and write latencies meet service-level requirements for the intended workloads. This choice is motivated by the fact that storage space is most often the primary bottleneck when using Flash SSDs under typical production workloads at Facebook.

### Log-Structured Merge-Trees
RocksDB is based on Log-Structured Merge-Trees (LSM-trees). The LSM-tree was originally designed to minimize random writes to storage as it never modifies data in place, but only appends data to files located in stable storage to exploit the high sequential write speeds of hard drives. As technology changed, LSM-trees became attractive because of their low write amplification and low space amplification characteristics.

## Innovation
+ dynamic LSM-tree level size adjustment
+ tiered compression, shared compression dictionary
+ prefix bloom filters
+ different size multipliers at different LSM-tree levels