# Drizzle
Fast and Adaptable Stream Processing at Scale

## Overview of Drizzle
Fast Data Processing: Drizzle is designed to handle data very quickly. It makes sure that information moves through the system fast and without delays.

Keeps Data Processing Separate: Drizzle separates the main data processing tasks from other system management tasks. This helps it process data faster because it doesn't have to stop for frequent system checks.

Stays Reliable During Changes: Even when things change, like increased data or system problems, Drizzle keeps working well without slowing down

**WHAT ARE STREAMING PROCESSING SYSTEMS?**

Stream processing systems are engineered to continuously ingest and process data streams item by item or in small batches, facilitating immediate analysis and rapid response to emerging data.

**IMPORTANCE OF STREAMING PROCESSING SYSTEMS**

Stream processing systems are vital for modern real-time applications, handling high volumes of data with sub-second latencies to support decision-making processes that are critical for operations such as real-time object recognition and internet quality experience prediction.

## previous system problem
previous systems(Apache Flink,Naiad, FlumeJava, Spark Streaming) typically impose a barrier across all nodes after every batch, resulting in high latency

**CHALLENGES IN EXISTING SYSTEMS**

The challenges related to existing systems in stream processing mainly focus on continuous operator and micro-batch systems:

**CONTINUOUS OPERATOR SYSTEM:**
+ Achieves low operational latency during normal conditions but incurs high adaptation costs and slow recovery times due to reliance on stateful checkpoints during failures.
+ It includes systems like Naiad and Apache Flink.

**MICRO-BACTH SYSTEM:**
+ Utilizes a Bulk-Synchronous Processing (BSP) model to enable efficient fault recovery and adaptability, but suffers from higher latency due to synchronization barriers required across all nodes after each batch.
+ It includes systems like Spark Streaming and FlumeJava.

**DESIRED PROPERTIES OF STREAM PROCESSING SYSTEMS**
+ High Throughput: Essential to handle massive data volumes as reported by major platforms like LinkedIn and Twitter; Drizzle targets exceeding these high ingest rates through efficient distributed processing.
+ Low Latency: Minimal delay between data reception and output production, critical for the responsiveness required in real-time applications such as anomaly detection.
+ Adaptability: Capability to manage long-lived jobs and adapt to frequent changes in hardware, workload, and operational variables without affecting system performance or accuracy.
+ Consistency: Maintaining reliable and accurate outputs across distributed systems, ensuring coherent transactional states or outputs to preserve data integrity.


## Architecture
Extends BSP model: By building on the well-established BSP model used in data processing, Drizzle introduces optimizations that make it more suitable for the high-speed requirements of streaming data.

Group scheduling for reduced task scheduling overhead: Instead of scheduling each task one by one, Drizzle schedules a batch of tasks together, reducing the time and computational resources needed for scheduling.

Pre-scheduling shuffles to eliminate waiting: Data shuffling, which is redistributing data across different parts of the system, is prepared in advance so that tasks can be executed as soon as the data is ready, without waiting for a signal.

## Computation Models for Streaming
### BSP
The bulk-synchronous parallel (BSP) model

In this model, the computation consists of a phase whereby all parallel nodes in the system perform some local computation, followed by a blocking barrier that enables all nodes to communicate with each other, after which the process repeats itself.

e.g. MapReduce
Spark(DAG)

Drizzle builds on existing BSP-based streaming systems,

### dataflow
Continuous Operator Streaming

Unlike BSP-based systems, which require a barrier at the end of a micro-batch, continuous operator systems do not impose any such barriers.

handle machine failures, dataflow systems typically use distributed checkpointing algorithms to create consistent snapshots periodically

## Top 3 contribution
1. Scalability: Drizzle focused on improving scalability by redesigning many of MySQL's internals. It aimed to make the system more efficient and scalable, particularly for modern web applications dealing with large volumes of data and high concurrency.
2. Simplicity and Modularity: Drizzle emphasized simplicity and modularity in its architecture. It removed many features and modules that were deemed unnecessary for typical web applications, resulting in a leaner and more manageable codebase. This approach made it easier to maintain and extend Drizzle for specific use cases.
3. Performance Optimization: Drizzle put a strong emphasis on performance optimization. It introduced various improvements, such as better caching mechanisms, optimized storage engines, and enhanced query execution strategies, to deliver faster and more efficient database operations. These optimizations were crucial for meeting the demands of high-traffic web applications.

prof
1. Low lantency