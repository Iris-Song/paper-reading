# BigQuery
Google BigQuery, a fully-managed and cloud-based interactive query service for massive datasets. 

BigQuery is the external implementation of one of the company’s core technologies whose code name is Dremel. BigQuery provides the core set of features available in Dremel to third party developers. It does so via a REST API, a command line interface, a Web UI, access control and more,

Dremel is a query service that allows you to run SQL-like queries against very, very large data sets and get accurate results in mere seconds

## Dremel
### Columnar Storage
![](./Columnar%20storage%20of%20Dremel.png)
pros:
+ Traffic minimization
+ Higher compression ratio

cons:
+ not working efficiently when updating existing records. Dremel, it simply doesn’t support any update operations. 

### Tree Architecture
![](./Tree%20architecture%20of%20Dremel.png)

## BigQuery versus MapReduce
+ Dremel is designed as an **interactive** data analysis tool for large datasets
+ MapReduce is designed as a programming framework to **batch process** large datasets
+ MapReduce and Dremel are both massively parallel computing infrastructures, but Dremel is specifically designed to run queries on Big Data in as little as a few seconds.

![](./MapReduce%20and%20BigQuery%20Comparison1.png)
![](./MapReduce%20and%20BigQuery%20Comparison2.png)