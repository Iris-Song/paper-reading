# Borg
Large-scale cluster management at Google.

It achieves high utilization by combining admission control, efficient task-packing, over-commitment, and machine sharing with process-level performance isolation. It supports high-availability applications with runtime features that min- imize fault-recovery time, and scheduling policies that reduce the probability of correlated failures. Borg simplifies life for its users by offering a declarative job specification language, name service integration, real-time job monitoring, and tools to analyze and simulate system behavior.

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

## Job Lifecycle in Borg
Job Properties: name, owner, number of tasks it has

Task Properties: resource requirements, task’s index within the job

Job States: Pending, Running, Evicted, Dead

How User can Interact with jobs: Submit, Kill, Update
![](./The%20state%20diagram%20for%20both%20jobs%20and%20tasks.png)

### Clusters and Cells
The machines in a cell belong to a single cluster

A cluster usually hosts one large cell and may have a few smaller-scale test or special-purpose cells.

### Allocs
A Borg ***alloc*** is a reserved set of resources on a machine in which one or more tasks can be run; 
resources remain assigned whether or not they are used.

Task → an alloc

Job → alloc set (a group of allocs)


## Architecture
A Borg cell consists of a set of machines, a logically centralized controller called the **Borgmaster**, and an agent process called the **Borglet** that runs on each machine in a cell. All components of Borg are written in C++.

Borgmaster consists of two processes: the main Borgmaster process and a separate scheduler. Borgmaster process handles client RPCs.It also manages state machines for all of the objects in the system (machines, tasks, allocs, etc.), communicates with the Borglets, and offers a web UI as a backup to Sigma.

scheduler, which assigns tasks to machines if there are sufficient available resources that meet the job’s constraints

The Borglet is a local Borg agent that is present on every machine in a cell. It starts and stops tasks; restarts them if they fail; manages local resources by manipulating OS kernel settings; rolls over debug logs; and reports the state of the machine to the Borgmaster and other monitoring systems.

For performance scalability, each Borgmaster replica runs a stateless link shard to handle the communication with some of the Borglets

## The Bad & Good
### The Bad
Restrictive Job Grouping: Borg's mechanism for grouping tasks under jobs was found to be overly restrictive, lacking a first-class way to manage multi-job services as a single entity or to refer to related instances of a service. Kubernetes: using label - arbitrary key-value pair.

Single IP Address Per Machine: Borg assigned a single IP address per machine, leading to shared port space among tasks and resulting complications. Kubernetes: unique IP address per pod and service.

Optimizing for Power Users: Borg's API, catering to power users, offered extensive customization at the cost of complexity for casual users. Kubernetes: streamlining common paths and providing accessible interface.

### The Good
Utility of Alloc: Borg’s alloc abstraction is proved powerful. Kubernetes adopts a similar concept with pods.

Cluster Management: Borg’s cluster management extends beyond just managing tasks and machines. Kubernetes incorporates these lessons, providing abstractions like services for naming and load balancing.

Introspection and Debugging: Borg emphasized the importance of surfacing debugging information to users. Kubernetes continues this approach, including tools for resource monitoring and log aggregation to support introspection.

Ecosystem Around the Master: The Borgmaster evolved into a kernel of a broader ecosystem. Kubernetes designs its architecture with this in mind.


## Difference btw k8s

1. **Origins and Ecosystem**:
   - **Borg**: Borg was developed internally by Google to manage their vast array of containerized applications. It was not initially intended for public use and was tightly integrated into Google's infrastructure.
   - **Kubernetes (K8s)**: Kubernetes, on the other hand, was developed based on the lessons learned from Borg but with a focus on open-source, community-driven development. It was designed to be platform-agnostic and can run on various cloud providers as well as on-premises infrastructure.

2. **Openness and Community**: 
   - **Borg**: Borg was not open-sourced, and its development was solely within Google.
   - **Kubernetes (K8s)**: Kubernetes is open-source and has a vibrant community contributing to its development. This open nature has led to widespread adoption and support across various platforms and industries.

3. **Features and Flexibility**:
   - **Borg**: Borg is tailored specifically to Google's infrastructure and needs, offering deep integration with Google's internal systems and tools.
   - **Kubernetes (K8s)**: Kubernetes is designed to be more flexible and adaptable to different environments. It offers a wide range of features for container orchestration, scaling, deployment, and management, making it suitable for diverse use cases beyond Google-scale operations.

4. **Scope**:
   - **Borg**: Borg is primarily focused on Google's internal use cases and is tightly integrated with their infrastructure.
   - **Kubernetes (K8s)**: Kubernetes is designed to be a general-purpose container orchestration platform, serving a broader audience beyond a single organization. It can be used by enterprises, startups, and individual developers alike.

In summary, while both Borg and Kubernetes share similarities in their origins and purpose (container orchestration), Kubernetes distinguishes itself with its open-source nature, broader community support, and platform-agnostic design, making it more accessible and flexible for a wider range of users and environments.