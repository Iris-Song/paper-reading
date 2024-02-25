# Kafka
a Distributed Messaging System for Log Processing

### log data
1. user activity events corresponding to logins, pageviews, clicks, “likes”, sharing, comments, and search queries
2. operational metrics such as service call stack, call latency, errors, and system metrics such as CPU, memory, network, or disk utilization on each machine. 

### Kafka vs other log product 
Facebook’s Scribe, Yahoo’s Data Highway, and Cloudera’s Flume. Those systems are primarily designed for collecting and loading the log data into a data warehouse or Hadoop for offline consumption.

Kafka (LinkedIn) that combines the benefits of traditional log aggregators and messaging systems.
1. Kafka is distributed and scalable, and offers high throughput.
2. Kafka provides an API similar to a **messaging** system and allows applications to consume log events in real time.

"pull" model

## Architecture and Design Principles
![](./Kafka%20Architecture.png)

![](./Kafka%20log.png)
### Efficiency on a Single Partition
Simple storage

Efficient transfer

Stateless broker

### Distributed Coordination
`consumer groups`: consists of one or more consumers that jointly consume a set of subscribed topics

goal is to divide the messages stored in the brokers evenly among the consumers, without introducing too much coordination overhead.

1. make a partition within a topic the smallest unit of parallelism.
2. not have a central “master” node, but instead let consumers coordinate among themselves in a decentralized fashion.

To facilitate the coordination, we employ a highly available consensus service Zookeeper. Zookeeper has a very simple, file system like API. 

### Delivery Guarantees
Kafka only guarantees at-least-once delivery.