## Stage 1: Single Web Server
- Everything on a single machine: web app, database, cache. etc.
- Overview: ![[Pasted image 20240522153503.png]]
- Request flow and traffic source: ![[Pasted image 20240522153639.png]]
### Steps: 
1. Users access websites through domain names, such as “api.mysite.com”. Usually the DNS is a paid service provided by 3rd parties and not hosted by our servers.
2. IP address is returned to the browser or mobile app.(e.g. 15.125.23.214)
3. Once IP address is obtained, HTTP requests are sent directly to your web server.
4. The web server returns HTML pages or JSON response for rendering.

Next, let us examine the traffic source. The traffic to your web server comes from two sources: web application and mobile application.
- **Web application**: it uses a combination of ==server-side languages== (Java, Python, etc.) to handle business logic, storage, etc., and ==client-side languages== (HTML and JavaScript) for presentation.
- **Mobile application**: HTTP protocol is the communication protocol between the mobile app and the web server. JavaScript Object Notation (JSON) is commonly used API response format to transfer data due to its simplicity.

## Stage 2:  Separate Database server
- We need multiple servers to handle additional load:
1. Web Server: To Handle web/mobile traffic
2. Databaser Server: To Handle database
- **Advantage**: Allows both of them to be scaled independently
- Overview: ![[Pasted image 20240522154516.png]]
- **Limitation**: If many users are connected to web server directly, and the server reaches its limit, users experience slower response or the fail to connect the server. 
## Stage 3: Introduce a [[Load Balancer]]
![[Pasted image 20240522155334.png]]
- Users connect to the IP of the load balancer directly, and can’t access the web servers directly anymore. Load balancer communicates with web servers through private IPs.
- We solve 2 problems with a Load balancer:
	1. **No failover issue**: If one server goes offline, all traffic is rerouted to server 2.
	2. **Improved availability of web tier**: All traffic is divided between 2 servers, and even if that is not enough, we can simply add more we servers to the pool, and load balancer automatically starts sending requests to them.
- **Limitation**: No failover or redundancy support for data tier(data server). 

## Stage 4: Add Master-Slave DB Replication
![[Pasted image 20240522155955.png]]
- The web server reads all data from slave database, whereas routes any data-modifying operations(write/update/delete) to the master database.
- **Potential Improvement**: Improving the load/response time.

## Stage 5: Add a [[Cache]] layer and [[Content Delivery Network(CDN)]]

### Adding a cache

![[Pasted image 20240522160746.png]]
- Use cache for data which is modified infrequently but read frequently. 
- Save important data in persistent data stores, not cache.
- Implement an expiration policy.
- Ensure the data store and cache are in sync → Consistency.
- Have multiple cache servers across different data centres to avoid SPOF.
- Implement an eviction policy.

### Adding a CDN
- As CDN servers are closer to users, it delivers static content faster than web servers.
![[Pasted image 20240522161246.png]]
- CDN workflow: ![[Pasted image 20240522161326.png]]
- Considerations:
	1. **Cost**: CDNs charge for data transfers in and out, so do not keep infrequently used assets in CDN.
	2. Set an appropriate cache expiry, especially for time-sensitive content.
	3. **Fallback**: You should consider how your website/application copes with CDN failure.

![[Pasted image 20240522161635.png]]
**Improvements**: 
1. Static assets(JS, CSS, images, etc.) are no longer served by web servers. They are fetched from CDN for better performance.
2. The database load is lightened by caching data.

## Stage 6: Stateless Web Tier
To scale the web tier horizontally, we need to move state(e.g. user instance data) out of the web tier. We can store stateful data in a persistent data store(SQL/NoSQL), and web servers can access state data from databases. This way we get a stateless web tier.

[[Stateful Architecture]] → [[Stateless Architecture]]

![[Pasted image 20240522212026.png]]

In the stateless architecture, **auto-scaling** of the web tier is easily achieved by adding or removing servers based on traffic load. 

**Potential Improvement**: Improved Availability and a better user experience across wider geographical areas.

## Stage 7: Setup Data Centers
Users are geoDNS-routed, to the closest data center. geoDNS is a DNS service that allows domain names to be resolved to IP addresses based on the location of a user. Here $x$% traffic goes to US-East and $(100-x)$% traffic goes to US-West.

![[Pasted image 20240522212329.png]]In the event of any significant data center outage, we direct all traffic to a healthy data center. In our example, if data center 2 (US-West) is offline, and 100% of the traffic is routed to data center 1 (US-East).

![[Pasted image 20240522212621.png]]

### Technical Challenges:
1. **Traffic Redirection**: Use GeoDNS.
2. **Data Synchronization**: Users from different regions could use different local databases or caches. In failover cases, traffic might be routed to a data center where data is unavailable. A common strategy is to replicate data across multiple data centers
3. **Test and Deployment**: With multi-data center setup, it is important to test your website/application at different locations. Automated deployment tools are vital to keep services consistent through all the data centers.

**Potential Improvement**: Ability to scale different system components independently.

## Stage 8: Add a messaging queue and other tools
==Decoupling== makes a message queue a preferred architecture for building a scalable and reliable application. 

When working with a small website that runs on a few servers, logging, metrics, and automation support are good practices but not a necessity. However, now that your site has grown to serve a large business, investing in those tools is essential.

**Logging**: Monitoring error logs is important because it helps to identify errors and problems in the system. You can monitor error logs at per server level or use tools to aggregate them to a centralized service for easy search and viewing.
**Metrics**: Collecting different types of metrics help us to gain business insights and understand the health status of the system. Some of the following metrics are useful:
- Host level metrics: CPU, Memory, disk I/O, etc.
- Aggregated level metrics: for example, the performance of the entire database tier, cache tier, etc.
- Key business metrics: daily active users, retention, revenue, etc.

**Automation**: When a system gets big and complex, we need to build or leverage automation tools to improve productivity. Continuous integration is a good practice, in which each code check-in is verified through automation, allowing teams to detect problems early. Besides, automating your build, test, deploy process, etc. could improve developer productivity significantly.

![[Pasted image 20240522215829.png]]

**Potential Improvement**: Scaling the data tier

## Stage 9: Database Scaling
We will horizontally scale the database tier, also called as ==sharding==. 

Sharding separates large databases into smaller, more easily managed parts called shards. Each shard shares the same schema, though the actual data on each shard is unique to the shard.

![[Pasted image 20240522220157.png |400]]
![[Pasted image 20240522220251.png | 400]]


The most important factor to consider when implementing a sharding strategy is the ==choice of the sharding key==. Sharding key (known as a partition key) consists of one or more columns that determine how data is distributed.

A sharding key allows you to retrieve and modify data efficiently by routing database queries to the correct database. When choosing a sharding key, one of the most important criteria is to choose a key that can evenly distribute data. 

### Challenges
1. Re-sharding Data
2. Celebrity Problem
3. Join and de-normalization

Updated architecture: ![[Pasted image 20240522220536.png]]


