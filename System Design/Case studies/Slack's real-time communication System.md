# Requirements
1. Multiple Users, Multiple Channels
2. Users DM on message in channel
3. Real-time chat
4. Historical messages can be scrolled through

## Similar Systems
1. Multi-player Games
2. Real-time chat
3. Interactions
4. Real-time polls
5. Creator Polls

We need to brainstorm on: Channels, CheckPoints, Realtime Communication

# Database Schema

> [!WARN]
> The schemas can be in SQL or NoSQL, they are just representative of attributes of object.
## Users

| id  | name | handle |     |
| --- | ---- | ------ | --- |
|     |      |        |     |
## Channels

| id  | name | handle | type(DM/Channel) |
| --- | ---- | ------ | ---------------- |
|     |      |        |                  |
- 1:1 Communication(DM) is also a `channel` with 2 people.
- **How to render on UI , channels and DMs separately ?**
	- We use the “type” attribute to determine that.
## Membership

| id  | user_id | channel_id | affinity_score | is_muted | notify_always |
| --- | ------- | ---------- | -------------- | -------- | ------------- |
|     |         |            |                |          |               |
## Messages

| id  | user_ch_id | msg | created |
| --- | ---------- | --- | ------- |
|     |            |     |         |
# Storage
Q. **What could go wrong with MySQL/RDBMS for storing messages?**
A. Messages table wouldn’t scale very well. You need Sharding for this scale. And RDBMS doesn’t support sharding out of the box(but is supported using additional tools, and it is more painful to manage.). Sharding also needed as it is an enterprise application and you want **tenant isolation**(Amazon wouldn’t want to share its data storage with Netflix).

It will also cause problems with high throughput.

1. Huge Data
2. Sharding out of the box
3. Tenant Isolation
4. Load balancing
	1. Need to move data across nodes seamlessly
5. Support for high throughput without a lot of additional overhead

That is why we should use a Document style database(Cassandra/MongoDB). Also they store data in JSON format, so you can add new fields to the data without any major migrations.

Q. What is Sharding key ?
A. Membership ID or Organization ID

We can still store User, Channel, Membership on another database, preferably RDBMS(as the data is not huge, and is rendered mostly one-time on app startup).
# High level Architecture

## Sending messages

### Approach 1
- User sends a request to an API server in the Messaging service.
	POST: send_message(user_id, membership, msg)
- But millions of people are sending messages together, the API service would go down on the load. 
### Approach 2
- Use Websockets instead of Rest API, to avoid communication overhead and keep the communication real-time.
- Use a message queue to send data from WebSocket server to the database.
- What is the most important guarantee in Slack ?
	- Its not real-time communication, so we might not need Websocket.
	- We can’t lose the data, it is an enterprise application.(Message sent = Message saved)
	- It is our respo
### Approach 3
- Use Rest API
- Use different DB clusters, each having a lot of shards to handle tenant isolation and "stickiness"( ==the concept where a user's requests are consistently routed to the same server within a load balancer during their session==)
- Have Database routing in the API server/Proxy server to route to the correct DB cluster and shard.

Refer to [[Message Communication]].

### Approach 4
- Use REST API for storing messages
- Use Web-socket for real-time reads, use Edge servers for that.
- So the flow looks like: 
	1. Sync API call to Rest API server(Messaging service).
	2. Sync write to the DB, and receive ACK that the write is persisted
	3. Send notification to Edge server.
	4. Edge server notifies receiver that message is received.

![[Pasted image 20250221151123.png]]














