# [MapReduce](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)
map (k1,v1) &rarr; list(k2,v2)

reduce (k2,list(v2)) &rarr; list(v2)

## Fault Tolerance
### Worker Failure
Any map tasks completed by the worker are reset back to their initial idle state, and therefore become eligible for scheduling on other workers. Similarly, any map task or reduce task in progress on a failed worker is also reset to idle and becomes eligible for rescheduling.

### Master Failure
restart