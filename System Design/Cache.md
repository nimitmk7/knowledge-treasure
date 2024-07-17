A cache is a temporary storage area that ==stores the result of expensive responses or frequently accessed data in memory== so that subsequent requests are served more quickly.

In a nutshell, it is reducing response times by saving results of any heavy computation. We can save Disk I/O, Network I/O, Compute, etc. using Cache.

### Cache tier
The cache tier is a temporary data store layer, much faster than the database.
### Benefits
1. Better system performance
2. Reduced database workload
3. Ability to scale cache tier independently.

![[Pasted image 20240522160718.png]]

## Caching at different levels-Examples
1. Caching IP addresses in-memory at Proxy (Helps in stickiness)
2. Caching static assets on CDN.
3. Caching static assets at browser.(Saves network calls to CDN)
4. Caching “top-n” on huge tables(Saves CPU/DB computation)
5. In main memory of API server
6. Disk of API server

