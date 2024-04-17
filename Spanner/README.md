# Spanner
Spanner is Google’s scalable, multi-version, globally distributed, and synchronously-replicated database

### problem of big data
Bigtable can be difficult to use for some kinds of applications:

those that have complex, evolving schemas, or those that want strong consistency in the presence of wide-area replication.

## interesting features
First, the replication configurations for data can be dynamically controlled at a fine grain by applications.

provides externally consistent reads and writes, and globally-consistent reads across the database at a timestamp

## Organization
![](./Spanner%20server%20organization.png)

A zone has one zonemaster and between one hundred
and several thousand spanservers.
The former assigns data to spanservers; the latter serve data to clients.

The universe master and the placement driver are currently singletons.
The universe master is primarily a console that
displays status information about all the zones for interactive debugging. The placement driver handles automated movement of data across zones on the timescale of minutes.

## Data Model
a data model based on schematized
semi-relational tables, a query language, and general purpose transactions.