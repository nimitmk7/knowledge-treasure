Load balancing is a key component of highly-available infrastructures commonly used to **improve the performance and reliability** of web sites, applications, databases and other services by **distributing the workload across multiple servers**.

# Infra without load balancer

![Web infrastructure with no load balancing](wo_load_b.png)

In this example, the user connects directly to the web server, at yourdomain.com. If this single web server goes down, the user will no longer be able to access the website - SPOF(Single Point of Failure).

In addition, if many users try to access the server simultaneously and it is unable to handle the load, they may experience slow load times or may be unable to connect at all.

# Infra with Load Balancer

This single point of failure can be mitigated by introducing a load balancer and at least one additional web server on the backend. Typically, all of the backend servers **will supply identical content** so that users receive consistent content regardless of which server responds.

![Web Infra with Single Load Balancer](infra_with_lb.png)

The user accesses the load balancer, which **forwards the user’s request to a backend server**, which then responds directly to the user’s request. 

In this scenario, **the single point of failure is now the load balancer itself**. This can be mitigated by introducing a second load balancer.

# Choosing the backend server
Load balancers choose which server to forward a request to based on a combination of two factors. 

1. **Health Check**: Ensure that any server they can choose is actually responding appropriately to requests.
2. **Algorithm**: Use a pre-configured rule to select from among that healthy pool

## Health Check
Load balancers should only forward traffic to “healthy” backend servers. 

To monitor the health of a backend server, health checks **regularly attempt to connect to backend servers** using the protocol and port defined by the forwarding rules **to ensure that servers are listening.** 

If a server fails a health check, and therefore is unable to serve requests, it is automatically removed from the pool, and traffic will not be forwarded to it until it responds to the health checks again.

## Algorithms
The load balancing algorithm that is used determines which of the healthy servers on the backend will be selected. A few of the commonly used algorithms are:

### Round Robin
The servers will be selected sequentially. The load balancer will select the first server on its list for the first request, then move down the list in order, starting over at the top when it reaches the end.

> **“Distribute the load iteratively”**

Q. **When to use ?** 
A. Uniform infrastructure
B. Response times have a very small variance

![[Pasted image 20250512123706.png | 500 ]]
The numbers represent requests in order of arrival.
### Weighted Round Robin
> **“Distribute the load iteratively but as per weights”**

![[Pasted image 20250512123844.png | 500]]
The numbers represent requests in order of arrival.

Q. **When to use ?** 
A. Uniform infrastructure
B. Different types of workloads

### Least Connections
Least Connections means the load balancer will select the server with the least connections from the load balancer and is recommended when traffic results in longer sessions.

The purpose is to send the request to the server which is least occupied. 

Q. When to use ?
A. When response times has a big variance, e.g. Analytics

![[Pasted image 20250512124601.png | 500]]
### Source
The load balancer will select which server to use based on a hash of the source IP of the request, such as the visitor’s IP address. 

This method ensures that a particular user will consistently connect to the same server.

## How do load balancers handle state?
Some applications require that a user continues to connect to the same backend server. A Source algorithm creates an **affinity based on client IP** information. 

Another way to achieve this at the web application level is through **sticky sessions**, where the load balancer sets a **cookie** and all of the requests from that session are directed to the same physical server.

## Redundant Load Balancers
To **remove the load balancer as a single point of failure**, a second load balancer can be connected to the first to form a cluster, where **each one monitors the others’ health**. Each one is equally capable of failure detection and recovery.

![Redundant Load Balancers](red_lb.png)

In the event the main load balancer fails, DNS must take users to the to the second load balancer. Because DNS changes can take a considerable amount of time to be propagated on the Internet and to make this failover automatic, many administrators will use **systems that allow for flexible IP address remapping**, such as [Reserved IPs](https://www.digitalocean.com/community/tutorials/how-to-use-floating-ips-on-digitalocean). 

On demand IP address remapping eliminates the propagation and caching issues inherent in DNS changes by providing a static IP address that can be easily remapped when needed. The domain name can remain associated with the same IP address, while the IP address itself is moved between servers.

![Highly available Infra using Reserved IPs](ha-diagram-animated.gif)

## References
1. https://www.digitalocean.com/community/tutorials/what-is-load-balancing
2. 