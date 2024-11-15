## Introduction
Spanner is a scalable, multi-version, globally distributed and synchronously replicated database, built and deployed at Google. 

> “First system to distribute data at global scale and support externally consistent distributed transactions.”

It is a database that shards data across many sets of Paxos state machines in ==data-centers spread all over the world==.

This Replication is used for 
1. Global availability
2. Geographic locality
### Automatic handling

1. Clients automatically failover between replicas.
2. Spanner automatically reshards data.
	1. When amount of data changes
	2. When number of servers changes
3. Spanner Automatically migrates data across machines
	1. When machines fail
	2. When load has to be balanced

### Requirement and Application

**Millions of Machines**,
across **Hundreds of Data Centers**
and **Trillions of Database rows**.

> *“Most applications will choose ==lower latency over higher availability==, as long as they can survive 1 or 2 datacenter failures.”*

**Main Focus:** Managing cross-datacenter replicated data. 

### Features
1. Dynamically controllable replication configurations at a fine grain level.
	1. Which datacenters contain which data, how far data is from users(to control Read latency)
	2. How far replicas are from each other(to control Write latency)
	3. How many replicas are maintained(Control durability, availability and read performance)
2. Externally consistent reads and writes
3. Globally consistent reads across the database at a timestamp

#### Enabled features
1. Consistent backups
2. Consistent MapReduce executions
3. Atomic Schema updates

All at global scale, and even in the presence of global transactions. 
## Implementation

![[Pasted image 20241110142504.png]]

1. **Universe**:
    - A universe is the highest-level deployment unit in Spanner
    - Each organization typically runs very few universes
    - The three common types are:
        - Test/playground universe (for testing and experimentation)
        - Development/production universe (mixed environment)
        - Production-only universe (strictly for production workloads)

2. Zones:
	- Zones are the fundamental administrative deployment units within a universe
	- They are analogous to a Bigtable server deployment
	- Each zone typically represents:
	    - A distinct physical location or datacenter
	    - A collection of Spanner servers
		    - Zonemaster
		    - Span Servers
	    - A unit of fault isolation