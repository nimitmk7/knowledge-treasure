## Premise
7 network trips for a small update to a database.

How ? 
TCP Connection: 
- 3-way handshake : Establish Connection
+ Request-Response : Send query and receive acknowledgement
+ 2-way teardown: Terminate Connection

What’s hectic ?
Creating a connection every time.

How to bypass this ?
Instead use pre-established connections. 

## How ?

![[Pasted image 20240527071757.png]]

1. When the API server boots up, it creates a pre-configured number of connections to the database pre-emptively.
2. When a request comes in, API server’s request handler needs a DB connection to perform its operation.
3. It checks the in-memory blocking queue, and picks one connection out of the queue.
4. It uses this connection to send the request to and get the response from the DB.
5. Once the job is done, connection is not terminated, but instead added back to the connection pool. 
6. If the queue is empty(all connections are in use), the request waits until one of the connections is put back in the queue and becomes available to use. 

> [!INFO] Usage of Blocking Queue 
> Core idea behind Blocking queue is that you wait unless there is something to remove. But you can add as many elements as you want. 
>  
> Here, the elements of the blocking queue are TCP connection objects.


## Timeouts and Number of Connections
Every database has a limit to the maximum number of connections. What if my API server is not getting enough queries ? 

1. It is a waste of resources as it just holds up the connections without even using them.
2. Other API servers might be requiring more connections, but they are unable to do so.

So connection pool has a timeout for every connection. If I am putting a connection into the pool, and it is not being used for a certain amount of time, we terminate the connection.

In case, the API server get more requests and the connection pool does not have an active connection and it is being underutilized, it creates a new connection and adds it to the pool. The new request will then use this newly added connection.

So, every connectionPool has a `minConnectionCount` and a `maxConnectionCount` parameter.

> [!faq] FAQs
> **Q**. Suppose I have multiple requests waiting for connections from the pool. But in this waiting list, a request which has come later has a higher importance than a request which has come earlier. How do we ensure that we give the connection to the high priority request in this case ?
>
> **A**. Have 2 connection pools. 1 for high priority requests, and 1 for low priority requests. 













