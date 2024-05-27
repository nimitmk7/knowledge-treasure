## Stage 1: Basic Architecture

- Simply storing whether user is offline/online.
- Access Pattern: Key-Value
- Key: User ID(integer)
- Value: Online/Offline(boolean)
### Storage
![[Pasted image 20240523070511.png]]
### Interfacing API
- To get the data, we need an API server that interacts with the database, gets the values and sends it back to the user.
- We need to expose an endpoint from the API server.
- But in our current architecture, we need to send a single user ID to get its status. If we have a lot of users(e.g. 50) to show status for, it would slow down the system with a lot of API calls. 
- **Magic Bullet**: Batching.
- So we batch the information from UI, and we send a list of users for which we want the online/offline status in a single API call.
- This helps improve latency as: 
	- User ←→ API server latency > API server ←→ DB server latency
	- User connects to API server via Internet, whereas API server and DB server are in the same data center.
	- So less API calls, improved application performance in terms of latency.
![[Pasted image 20240523070704.png]]
### Update the Database
We can have 2 approaches:
1. **Push-based**: Client tells API server I am alive.
2. **Pull-based**: API server asks client whether it is alive.

![[Pasted image 20240523071640.png]]

- API server does not know client’s IP address, connection is unless there is a persistent connection with the client.
- It is always the client that initiates connection with the server, and server can’t initiate the connection with the client(doesn’t know IP address). 
- So we need a **push-based** [[Heartbeats]] system. Every user periodically sends “hearbeat” to the API server.

![[Pasted image 20240523072230.png]]
- **So when is a user offline**? : When we did not receive “heartbeat” for long enough(threshold is a parameter to be decided).
- That means we need to store in database store “==time you last received heartbeat==”.
## Stage 2

### Storage & Update Query
- Instead of a boolean online/offline, we store the time of last heartbeat we received.
![[Pasted image 20240523072731.png]]

### API interface
- If the user has a heartbeat in the last 30 seconds, they are online. Otherwise offline.
![[Pasted image 20240523072903.png]]
### Estimating the Scale
- No of DB entries = No of users. 
- Each entry has 2 columns:

| user_id      | last_hb      |
| ------------ | ------------ |
| int(4 bytes) | int(4 bytes) |
- Size of each entry: 8 bytes
- Total storage for 1 Billion entries $\sim$ 8 GB
- ==Storage is not an issue at all for this system==. 

## Stage 3

### Optimize the Storage
- Can we reduce the number of columns ?
	- **NO**
- Can we reduce the number of rows/entries ?
	- **YES**

- If entry exists, user online. Otherwise user offline.
	- If only 1M users are online out of 1B total users, I will have 1M entries in DB instead of 1B. 
	- Storage requirement: 8 GB → 8 MB.
	- **Total Entries = Active Users**
- Lets expire/delete the entries after 30 seconds, and save a bunch of space by not storing data of inactive users.

### Delete Job
**Approach 1**: Write a CRON Job that deletes expired entries
**Approach 2**: Can we not offload this to our data store ?

We go with approach 2, as we never reinvent the wheel. Less components to build and control, the better. 

 Upon receiving a heartbeat, 
	 ![[Pasted image 20240523075039.png]]
### Choice of Data Store
- **Requirements**: K-V Database + Expiry feature
- Choices: Redis, DynamoDB

#### Redis vs DynamoDB

|                                   | Redis | DynamoDB |
| --------------------------------- | ----- | -------- |
| **Feature**                       |       |          |
| Vendor Agnostic/No Vendor lock-in | True  |          |
| Availability Zones/Scalability    |       | True     |
| Security                          |       | True     |
| In-memory                         | True  |          |
| Lower Cost                        |       | True     |
| Speed/Latency                     | True  |          |
| Extensibility                     | True  |          |
### Performance Evaluation
- Heartbeat sent every 10 seconds
- 1 user in 1 minute → 6 hearbeats
- 1M active users → 6M requests/minute
- Each heartbeat = 1 DB Call → 6M updates/minute
- **Bottleneck!!!**

## Stage 4
- Use Connection Pool


