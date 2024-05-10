# Spanner
Spanner is Google’s scalable, multi-version, globally distributed, and synchronously-replicated database

### problem of big data
Bigtable can be difficult to use for some kinds of applications:

those that have complex, evolving schemas, or those that want strong consistency in the presence of wide-area replication.

### Why Spanner?
+ Limitations of Existing Solutions
    + Previous database systems like Bigtable had challenges with complex schemas
+ Requirements for a New Database System
+ Strong consistency across global data distribution.
+ Features and Benefits of Spanner
+ Supports both transactions and SQL queries, integrating traditional database advantages with NoSQL scalability.

## key features
+ Global Distribution
+ Strong Consistency
+ Complex Schemas
+ SQL Queries
+ Fine-grained Control
+ Consistency and Timestamps

## Organization
![](./Spanner%20server%20organization.png)

A zone has one zonemaster and between one hundred
and several thousand spanservers.
The former assigns data to spanservers; the latter serve data to clients.

The universe master and the placement driver are currently singletons.
The universe master is primarily a console that
displays status information about all the zones for interactive debugging. The placement driver handles automated movement of data across zones on the timescale of minutes.

1. Universe
- Top-level container for all deployments.
2. Zones
- Correspond to data centers or machine clusters.
- Functions: Administrative deployment, physical isolation, and data replication.
3. Zonemaster
- Assigns data to spanservers.
4. Spanservers
- Serve data to clients; manage tablet (tundamental data units).
5. Location Proxies
- Aid clients in locating spanservers.
6. Additional Elements
- Universe Master: Displays system status.
- Placement Driver: Manages data movement for load balancing
## Data Model
a data model based on schematized
semi-relational tables, a query language, and general purpose transactions.

## Q
###  Diff btw GFS and Spanner
GFS is a file system, it has not consistency. Spanner is a driver. It has consistency control

### What is true time in Spanner and how is it used to provide consistency? Why is it required?

TrueTime in Google Spanner is a distributed system clock that provides a globally consistent view of time across data centers. It combines GPS and atomic clocks to generate timestamps with known error bounds. TrueTime is crucial for Spanner's external consistency model, ensuring that transactions are globally ordered based on real time. It's required to maintain strong consistency guarantees, essential for distributed databases like Spanner where data integrity and accuracy are paramount.