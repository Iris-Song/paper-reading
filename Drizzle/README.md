# Drizzle
Fast and Adaptable Stream Processing at Scale

## previous system problem
previous systems(Apache Flink,Naiad, FlumeJava,Spark Streaming) typically impose a barrier across all nodes after every batch, resulting in high latency

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