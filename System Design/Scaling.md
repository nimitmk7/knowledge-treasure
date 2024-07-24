It is the ability to handle large number of concurrent requests.
## [[Vertical and Horizontal Scaling]]

> [!info] Whenever you scale, always do it bottom up!
>  ![[Pasted image 20240717215645.png]] 
>  Your stateful components like DB and Cache should be able to handle the large number of concurrent requests before you increase your API servers.

## Scaling Database

![[Pasted image 20240717215852.png]]

### Read Replicas
Non-critical read requests go to read replicas. 
Critical reads and writes go to master.


### Sharding
Split your data into mutually exclusive subsets and have multiple different servers handle different subsets. Each of those servers can have their own read replicas if needed. 

API server has connections all the DB servers and their replicas. 

### Proxy SQL
Add a proxy server ==that is aware of Topology and takes care of routing==.

![[Pasted image 20240718125039.png]]

**Con**: Extra network hop to Proxy server.







