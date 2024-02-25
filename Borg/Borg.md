# Borg
Large-scale cluster management at Google.

It achieves high utilization by combining admission con- trol, efficient task-packing, over-commitment, and machine sharing with process-level performance isolation. It supports high-availability applications with runtime features that min- imize fault-recovery time, and scheduling policies that reduce the probability of correlated failures. Borg simplifies life for its users by offering a declarative job specification language, name service integration, real-time job monitoring, and tools to analyze and simulate system behavior.

### three main benefits:
1. hides the details of resource management and failure handling so its users can focus on application development instead; 
2. operates with very high reliability and availability, and supports applica- tions that do the same; 
3. lets us run workloads across tens of thousands of machines effectively. 

![](./The%20high-level%20architecture%20of%20Borg.png)

## User Perspective
Google developers and system administrators(site reliability engineers or SREs)

### workload
Borg cells run a heterogenous workload with two main parts.
1. long-running services. should “never” go down, and handle short-lived latency-sensitive requests
2. batch job. take from a few seconds to a few days to complete; these are much less sensitive to short-term performance fluctuations. 

### App run on Borg
+ MapReduce
+ GFS
+ Bigtable ..

### Clusters and Cells
The machines in a cell belong to a single cluster

A cluster usually hosts one large cell and may have a few smaller-scale test or special-purpose cells.
![](./The%20state%20diagram%20for%20both%20jobs%20and%20tasks.png)

### Allocs
A Borg ***alloc*** is a reserved set of re- sources on a machine in which one or more tasks can be run; 
resources remain assigned whether or not they are used. 

## Architecture
A Borg cell consists of a set of machines, a logically centralized controller called the **Borgmaster**, and an agent process called the **Borglet** that runs on each machine in a cell. All components of Borg are written in C++.