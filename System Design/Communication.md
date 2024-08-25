It is all about how a client communicates with a server. 

## Request-Response Paradigm
It is the simplest and most used communication paradigm between a client and server.
![[Drawing 2024-08-22 10.18.38.excalidraw.png]]
It is the RPC(Remote Procedure Call) paradigm, where you invoke method calls on another machine. 

HTTP APIs is one way to do RPC, which is the most commonly used method of implementing RPCs.

## Short Polling
- **Short Polling** involves the client sending HTTP requests at regular intervals to check for updates.
- The server processes each request and returns a response immediately, regardless of whether new data is available.

![[Pasted image 20240822104702.png | 400]]

Examples: Continuously refreshing cricket score, checking if server is ready.

### Disadvantages
1. HTTP overhead
2. Requests and Responses


## Long Polling
- **Long Polling** is a variation of the traditional polling technique where the server holds a client request open until new data is available.
- Upon receiving new data, the server responds and closes the connection. The client then immediately sends another request, thus creating a near-continuous connection.

![[Pasted image 20240822105206.png | 400]]
- Long polling reduces the number of request calls between client and server.
- If the server takes too long, connection times out and it has to be re-established.

E.g. Response only when new ball is bowled, in cricket score application.

## Short Polling vs Long Polling
- Short polling sends response right away
- Long polling sends response only when done.
	- Connection kept open for the entire duration

- E.g. EC2 provisioning
	- Short Polling → Gets status every few seconds
	- Long Polling → Gets response when server is ready

## Websockets

### Premise
In a normal RPC connection, you do a 3-way handshake to establish connection, then 2 network calls for request and response, and 2 network calls for terminating the connection. 

So, you are making 5 network calls to create a connection, which is a huge overhead. Websockets solves this issue.
### Detail
![[Pasted image 20240822144842.png | 400]]
It establishes a persistent(so bidirectional) connection between client and server.

Server can proactively send data to client without client even asking for it.

### Advantages
1. Real-time data transfer
2. Low communication overhead

### Applications
1. Real-time communication
2. Stock Market ticker
3. Live experiences
4. Multi-player games

## Server-sent events

![[Pasted image 20240822150008.png | 500]]
Client establishes connection with the server, and server keeps sending data to client without client asking for it. 

### Applications:
1. Stock market ticker
2. Deployment logs streaming










