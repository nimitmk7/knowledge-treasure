A stateful server remembers client data(state) from one request to next. 

An example: 
![[Pasted image 20240522211523.png]]

So, to authenticate User A, HTTP requests must be routed to Server 1. If a request is sent to other servers like Server 2, authentication would fail as it does not contain session data for User A. Similar situation prevails for User B and User C. 

The issue is that every request from the same client must be routed to the same server. This can be done with sticky sessions in most load balancers; however, this adds the overhead. ==Adding or removing servers is much more difficult with this approach. It is also challenging to handle server failures==.


