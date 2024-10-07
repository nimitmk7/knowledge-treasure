## Definition
It is the single point of entry to the clients of an application. It sits between the clients and collection of backend services for the application. 

![[Pasted image 20241005105458.png | 500]]

## Typical Functions
1. Authentication and security policy enforcements
2. Load balancing and circuit breaking
3. Protocol translation and service discovery
4. Monitoring, Logging, analytics and billing
5. Caching

## Flow

**Step 1:** Client sends a request to the API gateway.

The request is typically HTTP-based, it could be REST, GraphQL or some other higher-level abstractions.
![[Pasted image 20241005105739.png]]

**Step 2:** The API gateway validates the HTTP request
![[Pasted image 20241005105953.png | 500]]

**Step 3:** The API gateway check the caller’s IP address and other HTTP address against its allow-list and deny-list.

![[Pasted image 20241005110104.png]]

**Step 3A:** It may also perform basic rate limit checks against attributes such as IP address and HTTP headers. It can reject requests from an IP address exceeding a certain rate.

![[Pasted image 20241005110316.png]]

**Step 4:** the API gateway passes the request to an identity provider for authentication and authorization.

The API gateway receives an authenticated session token from the provider with the scope of what the request is allowed to do. 

![[Pasted image 20241005110635.png]]

> [!faq] What is the difference between authentication and authorization ?
> **Authentication verifies a user's identity**, confirming that they are who they claim to be, while **authorization determines what level of access a user has** to specific resources once their identity is verified.

> [!faq] What is an identity provider ?
>  An identity provider (IdP), or IdP administrator, is **a system that creates, stores, and manages digital identities**. The IdP can either directly authenticate the user or can provide authentication services to third-party service providers (apps, websites, or other digital services).
> 
> Some examples include: Google, Facebook, Okta, Azure Active Directory. Often used for single sign-on (SSO), they allow users to log in once and   access multiple services without repeated logins. 
>  
>  In contrast, a service provider is a website or application that offers services or resources to users. It relies on the IdP to confirm a user's Identity before granting access. The SP trusts the IdP to authenticate users correctly, and based on this trust, allows users access to its services. This includes online platforms like cloud-based applications, shopping sites, or corporate intranet portals. The relationship between IdPs and SPs provides user convenience and security.

**Step 5:** A higher level rate-limit check is applied against the authenticated session. If it is over the limit, the request is rejected at this point. 

![[Pasted image 20241005111319.png]]

**Step 6 and 7**: With the help of a service discovery component, The API gateway locates the appropriate backend service to handle the request by path matching.
![[Pasted image 20241005112044.png]]

> [!INFO] Analogy between gate and gateway - My own understanding
> The essential feature of a gate of a house/building is essentially an entry point the place. But along with that, it also acts as a security checkpoint for entry, like having a door password or tall gate for protection, or a security guard. 
> 
> API gateway can be thought of as the same, its main feature is to route user to the required service,or provide access to the resources. But along with that, it also checks the incoming requests for security(rate-limiting, auth, validation, etc.)

**Step 8:** The API gateway transforms the request into the appropriate protocol and sends the transformed request to the backend service. 
**Example:** gRPC

![[Pasted image 20241005112221.png | 500]]

When the response comes back from the backend service, the API gateway transforms the response back to the public-facing protocol and returns the response to the client. 

![[Pasted image 20241005112422.png | 500]]

## Other services
1. It should track errors and provide circuit-breaking functionality to protect the services from overloading.
2. It should also provide logging, monitoring and analytics services for operational observability.
![[Pasted image 20241005113908.png | 500]]

> [!faq] What is circuit-breaking ?
> How Circuit Breaking Works: 
> 1. The API Gateway monitors the health and response times of downstream services 
> 2. If a service starts failing or responding slowly, the circuit breaker "trips" and stops sending requests to that service. 
> 3. For a set period, all requests to the problematic service are either rejected immediately or redirected to a fallback mechanism.
> 4. After this timeout, the circuit breaker allows a limited number of test requests through.
> 5. If these requests succeed, the circuit is closed and normal operation resumes. If they fail, the open circuit is maintained.

## Conclusion
1. It is a very critical piece of the infrastructure.
2. It should be deployed to multiple regions to improve availability.
3. For many cloud provider offerings, the API gateway is deployed across the world close to the clients.

## AWS Tutorial
<iframe width="560" height="315" src="https://www.youtube.com/embed/qnVfWG8N7Fw?si=714WXbp8zrak7a_d" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


## References

1. 
<iframe width="560" height="315" src="https://www.youtube.com/embed/6ULyxuHKxg8?si=JVKeIFXJPWMZK0tX" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

2. https://www.okta.com/identity-101/why-your-company-needs-an-identity-provider/












Examples: Amazon API gateway, Azure API management, Google API gateway, Cloudflare

