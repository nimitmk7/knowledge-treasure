# Requirements
1. Balance the load
2. Tunable Algorithm
3. Scaling beyond one machine

The most important requirement for Load balancer is very high availability. 

Need to brainstorm on: 
1. Load Balancer configurations
2. Config synchronization
3. Admin Console
4. Monitoring
5. Scaling
6. Availability

# Brainstorm

## Storing the configuration: What, How, Where ?

1. List of all available backend servers
2. Statistics for load balancing algorithms
	1. Availability of backend servers
	2. No of connections to them
	3. Health status of backend servers

### DB requirements
Storage: Persistence(Need in case of system crash or reboot)
Reads >> Writes (Data can be in-memory)

It is just retrieving a configuration(100 API servers: IP Addresses and ports and some metadata), data is in KB. Can be easily stored in memory. Can use a KV Store. 

## Demo Flow

Pre-load the config from DB to in-memory at the time of LB server bootup, as the data is small and in-memory lookup is extremely fast.

![[Pasted image 20250522095736.png]]

Based on the algorithm, it will forward the request to backend server. 

LB server initiates a TCP connection to the backend server, and passes the HTTP request received to the backend server. In this case, LB server acts a client.

## How to change the configuration

We need to have a console(LB admin console), it will have APIs that will change the config stored in the database. 

Now we want the LB servers to update their config(stored in memory), so we use a messaging bus where we push the updates to the LB server. 

Why push and not pull ? Because the number of updates are very low in frequency. 

![[Pasted image 20250522200804.png]]
## High Availability

**Q. What if the messaging bus goes down ?**
**A.** Have a pull mechanism as a fallback, read the config DB once a minute for every load balancer server, and monitor for changes. 

A better version is: 
Given that connection between LB servers and Messaging bus is persistent, once we receive an error, we can get the fallback pull mechanism started, instead of it being ON all the time. 

When the connection comes back up, the fallback mechanism is closed. 

What if the connection from LB Admin console to the Messaging bus breaks down ? The config DB will be updated but the event is not sent to the messaging bus. 

We need to use CDC for that.

**What about the high availability of LB servers ?**

 We need a LB orchestrator which will periodically send heartbeats to the LB servers, and add/remove LB servers based on need. It can also manage the backend servers in a similar fashion. 

![[Pasted image 20250522203125.png]]
 
**What about the availability of LB orchestrator ?**
We’ll add multiple orchestrators. But who’ll manage them ? What if one of them goes down. This is where we use leader election.
## Who monitors the orchestrator
We have a master orchestrator and worker orchestrators. 
![[Pasted image 20250531114528.png | 400]]
Worker orchestrators will do the grunt work, like checking heartbeats and monitoring the LB and backend servers, and other responsibilities.

Master orchestrator’s responsibility is to:
1. Monitor workers
2. Assign mutually exclusive work to workers. 

Like a manager of an engineering team. 

### What if the master orchestrator goes down ?
One of the worker orchestrators takes up the responsibility of being the master orchestrator by running ==leader election==. It is conducted when the workers discover that the master is down, this system allows your system to self recover from the failure. 

## How will orchestrator decide how many LB servers we need ?
It will be decided by the machine metrics like CPU usage, network card usage, RAM usage, etc. 

We’ll configure a time series database like Prometheus, there’ll be agents running on all the nodes(Backend, LB, Orchestrator), who will push the machine vitals to the time series DB. 

The orchestrator will periodically query prometheus to check the metrics and use them to make its decisions(adding/removing LB servers).

![[Pasted image 20250531120847.png]]

## How will client connect  to this system ?
For a client to connect to a load balancer, it needs the IP address of the load balancer, but if you have multiple LB servers, which IP address will you connect to ? 

We configure a private DNS server. Its will resolve IP address for the domain name(of the LB). When the request is made to the system, it first goes to a DNS server, where it has entries of the LB servers. 

DNS server does round robin on the the IP addresses and resolves the domain name to one of the LB server IP address using the round robin or weighted round robin algorithm. 

![[Pasted image 20250531121835.png]]
Thus our updated design looks like: 

![[Pasted image 20250531121907.png]]
Also, you don’t need to call DNS server for every request, the client will cache the resolved IP address on its side. The DNS server is very lightweight, as its only job is to return one of the IP addresses for the domain name.

One 4 GB machine can handle 32K requests per sec. 
## What about high availability of DNS server ?

We typically have 2 core DNS server, sharing the same virtual IP address: Primary and Secondary core DNS servers. 

All the requests go to the primary DNS server, secondary remains inactive. 

When secondary detects that primary is down, it “takes over”. The machines do not need to change config because virtual IP are same for both. 

![[Pasted image 20250531125947.png]]

Whenever a new LB server is added/removed, Orchestrator informs the Core DNS server to add/remove its IP address. 
# Further Reads
1. [Maglev Load Balancer](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/44824.pdf)



 