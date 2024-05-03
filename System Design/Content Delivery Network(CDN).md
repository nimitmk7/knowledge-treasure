A content delivery network (CDN) is a **geographically distributed group of servers** that **caches content close to end users**.

A CDN **allows for the quick transfer of assets needed for loading Internet content**, including HTML pages, JavaScript files, stylesheets, images, and videos.

The popularity of CDN services continues to grow, and today the majority of web traffic is served through CDNs, including traffic from major sites like Facebook, Netflix, and Amazon.

## Benefits of using CDN

Although the benefits of using a CDN vary depending on the size and needs of an Internet property, the primary benefits for most users can be broken down into four different components:

1. **Improving website load times** - By distributing content closer to website visitors by using a nearby CDN server (among other optimizations), visitors experience faster page loading times. As visitors are more inclined to click away from a slow-loading site, a CDN can reduce bounce rates and increase the amount of time that people spend on the site. In other words, a faster a website means more visitors will stay and stick around longer.
2. **Reducing bandwidth costs** - Bandwidth consumption costs for website hosting is a primary expense for websites. Through caching and other optimizations, CDNs are able to reduce the amount of data an origin server must provide, thus reducing hosting costs for website owners.
3. **Increasing content availability and redundancy** - Large amounts of traffic or hardware failures can interrupt normal website function. Thanks to their distributed nature, a CDN can handle more traffic and withstand hardware failure better than many origin servers.
4. **Improving website security** - A CDN may improve security by providing [DDoS mitigation](https://www.cloudflare.com/learning/ddos/ddos-mitigation/), improvements to security certificates, and other optimizations.

## How does a CDN work ?

At its core, a CDN is a **network of servers linked together** with the goal of delivering content as quickly, cheaply, reliably, and securely as possible. In order to improve speed and connectivity, a CDN will place servers at the exchange points between different networks.

![[Pasted image 20240503051543.png| 800]]

Beyond placement of servers in IXPs, a CDN makes a number of optimizations on standard client/server data transfers. CDNs place Data Centers at strategic locations across the globe, enhance security, and are designed to survive various types of failures and Internet congestion.

## Latency - How does a CDN improve load times?

When it comes to websites loading content, users drop off quickly as a site slows down. CDN services can help to reduce load times in the following ways:

- The globally distributed nature of a CDN means reduce distance between users and website resources. Instead of having to connect to wherever a website’s [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/) may live, a CDN lets users connect to a geographically closer [data center](https://www.cloudflare.com/learning/cdn/glossary/internet-exchange-point-ixp/). Less travel time means faster service.
- Hardware and software optimizations such as efficient load balancing and solid-state hard drives can help data reach the user faster.
- CDNs can reduce the amount of data that’s transferred by reducing file sizes using tactics such as [minification](https://www.cloudflare.com/learning/performance/why-minify-javascript-code/) and file compression. Smaller file sizes mean quicker load times.
- CDNs can also speed up sites which use [TLS](https://www.cloudflare.com/learning/security/glossary/transport-layer-security-tls/)/[SSL](https://www.cloudflare.com/learning/security/glossary/what-is-ssl/) certificates by optimizing connection reuse and enabling TLS false start.

## Reliability and redundancy - How does a CDN keep a website always online?
Uptime is a critical component for anyone with an Internet property. Hardware failures and spikes in traffic, as a result of either malicious attacks or just a boost in popularity, have the potential to bring down a web server and prevent users from accessing a site or service. A well-rounded CDN has several features that will minimize downtime:

- Load balancing distributes network traffic evenly across several servers, making it easier to scale rapid boosts in traffic.
- Intelligent failover provides uninterrupted service even if one or more of the CDN servers go offline due to hardware malfunction; the failover can redistribute the traffic to the other operational servers.
- In the event that an entire data center is having technical issues, [Anycast](https://www.cloudflare.com/learning/cdn/glossary/anycast-network/) routing transfers the traffic to another available data center, ensuring that no users lose access to the website

## Data security - How does a CDN protect data?

Information security is an integral part of a CDN. a CDN can keep a site secured with fresh [TLS/SSL certificates](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/) which will ensure a high standard of authentication, [encryption](https://www.cloudflare.com/learning/ssl/what-is-encryption/), and integrity. Investigate the security concerns surrounding CDNs, and explore what can be done to securely deliver content. [Learn about CDN SSL/TLS security](https://www.cloudflare.com/learning/cdn/cdn-ssl-tls-security/)

## Bandwidth expense - How does a CDN reduce bandwidth costs?

Every time an origin server responds to a request, bandwidth is consumed. See  A CDN, cuts down on origin requests and reduces bandwidth costs.


## References
1. https://www.cloudflare.com/en-in/learning/cdn/what-is-a-cdn/
2. 