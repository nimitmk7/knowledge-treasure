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

| id  | user_id | channel_id | affinity_score | is_muted | notify_always | checkpoint(Last message Id read) |
| --- | ------- | ---------- | -------------- | -------- | ------------- | -------------------------------- |
|     |         |            |                |          |               |                                  |
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

# APIs
1. `send_message(from_user, membership, text)`: REST Call. 
	1. API server finds relevant shard using channel_id and stores message there. 
2. `read_messages(channel_id):` REST call
	1. Send most recent message
	2. Scroll older messages iteratively page by page on the corresponding shard.

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

### Communication with Edge Server
- REST based short polling is a very poor UX for something as realtime as messaging…, so we need a better solution: 
- **Web-sockets!!!!**
- Each user will have 1 Websocket connection open with our backend infra and that will be used for anything and everytime realtime.
	- ![[Pasted image 20250221215245.png]]
- **Edge Servers**: Because Websockets are expensive and browsers have 6 concurrent TCP connection limit per domain, we have to multiplex all reatime communication on ONE websocket connection. 
- Hence, we need a fleet of servers(Edge Servers) to whom our end users connect over Web Sockets. 
- Any service chat, notification, etc. wants to talk to users in realtime, the info will go through these web servers. 
- ![[Pasted image 20250221215543.png]]
### Real-time communication.

- Every edge server knows whichuser is connected to it and how to communicate with it. (Using SocketIO library).
- ![[Pasted image 20250221215819.png]]
- Hence, when the message is sent from A to B the edge server will Find B in the local pool, and send message to B. 
- ![[Pasted image 20250221220346.png | 500]]
- In case of our design, Messaging service talks to Edge to send message to others. 

**What if some messages are not sent in real-time ?**
1. Periodic API call to fetch last 50 messsages and render them in UI
	1. Just in case few are missed. 
	2. Generate pseudo-notification for them
2. Checkpoint(last message ID) not updated, so client will ask for newer messages. 

## Scaling Edge Severs
- Will there be just one Edge server ? 
- How will we horizonatally scale them ?
- **Core Idea:** Connect the servers(using a TCP connection)
- ![[Pasted image 20250221221128.png]]

- Say each server can handle 4 users, what if get the 5th user ?
- The 5th user forms a websocket with another server. 

- How will message from A go to B ?
	- Local Connection Pool
- How will message from A go to E ?
	- Possible only when message reaches Edge Server 2.
- But if we have 100 Edge servers, will every server connect to every another server ?
	- Not a great idea !
	- Can we do better ?

- **Real-time Pub Sub!!!**
- Every Slack Channel has one Pub-Sub channel.
- Each Edge server subscribes to the Pubsub channel corresponding to the slack channels that user connected to it are part of.
- ![[Pasted image 20250221221425.png]]

- Now E joins the system and is part of channel C3. 
- So Edge server 2 will connect to real-time Pub Sub and subscribe to channel C3. 
- ![[Pasted image 20250221221537.png]]
- So users A, C and E are part of channel C3. 
- When A → C3(A sends a message in channel C3):
	- Message is asynchronously persisted in message store.
	- Edge Server 1 sends message to user C connected locally
	- Edge Server 1 pushes the message in PubSub(Redis) on PubSub channel corresponding to C3.
	- Edge server 2 receives the message because it sub(C3).
	- Edge server 2 finds local users Sub(C3) and forwards messages to them.
	- Thus, E receives message sent from A.
# Overall Architecture

![[Pasted image 20250221221839.png]]

- We put a connection balancer, it tells which edge server to connect to, when a user application wants to connect to a Web socket server.
	- All users of the same organization should be highly likely to connect to the same edge server, thus minimizing cross edge-server communication.













