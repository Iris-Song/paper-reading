# Kafka
a Distributed Messaging System for Log Processing

It is a distributed, scalable system which offers high throughput and has a API similar to a messaging system and allows applications to consume log events in real time.
- incorporates efficient and scalable design choices, making it suitable for both **offline** and online message consumption.
- supports publishing, subscribing, storing, and processing record **streams in real time**
## The Dawn of Data
Traditional:
- Info stored in Databases
- Databases store 'things', with no state.
- Cumbersome to store events in DB.

Log data: The digital footprint.
- User activity: Share, Comments, Searches
- Operational metrics: Latency, Errors, Network Metrics

The Challenges with Log Data:
- Volume: More than traditional data
- Real-time processing: Almost Impossible due to volume
- Previous systems relied on offline analysis of log data.
### log data
1. user activity events corresponding to logins, pageviews, clicks, “likes”, sharing, comments, and search queries
2. operational metrics such as service call stack, call latency, errors, and system metrics such as CPU, memory, network, or disk utilization on each machine. 

## Kafka vs other log product 
Facebook’s Scribe, Yahoo’s Data Highway, and Cloudera’s Flume. Those systems are primarily designed for collecting and loading the log data into a data warehouse or Hadoop for offline consumption.

Kafka (LinkedIn) that combines the benefits of traditional log aggregators and messaging systems.
1. Kafka is distributed and scalable, and offers high throughput.
2. Kafka provides an API similar to a **messaging** system and allows applications to consume log events in real time.

"pull" model

## Architecture and Design Principles
![](./Kafka%20Architecture.png)

![](./Kafka%20log.png)
1. Topic: Stream of Messages of a particular type.
2. Producer: publishes messages to a topic.
3. Brokers: Servers where published messages are stored.
4. Consumer: subscribe to one or more topics from the brokers.

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
1. At-least-once delivery
2. Customized duplicate logic instead of two phase commits
3. Order from one partition is guaranteed, but not from different partitions
4. Cyclic Redundancy Check (CRC) is deployed to check log corruption
5. 1/0 error: recovery process
6. Permanently damaged broker: unconsumed messages will be lost

### Deployment in Linkedin
1. Live datacenter and offline analysis datacenter
2. Auditing system to verify no data lost
3. MapReduce to store successful data in HDFS
4. Avro serialization protocol
![](./Deployment%20in%20Linkedin.png)