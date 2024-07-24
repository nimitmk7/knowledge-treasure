## Vertical Scaling
It is the process of adding more power(CPU, RAM, etc.) to your servers. Also referred to as “Scale up”. 

- **Pro:** Easy to manage
- **Con**: Risk of downtime higher

## Horizontal Scaling
It is the process of adding more servers to the resource pool when the load is higher. Also referred to as “Scale out”.  The extra servers are removed when the load is decreased. Therefore the servers are stateless.

- **Pro**: Linear amplification
	- **Tech Economics** : We can exactly tell how many servers are required for how much load. ($x$ load → $y$ servers)
		- Use load testing to figure out tech economics.
	- Can help **warm up** the infra: Pre-emptively ensure enough infra for increased load. 
		- Auto-scaling takes time, can’t rely on it for sudden surges in traffic. 
- **Pro**: Fault tolerance

## Vertical vs Horizontal

When traffic is low, vertical scaling is a great option, and the simplicity of vertical scaling is its main advantage. Unfortunately, it comes with serious limitations.
- Vertical scaling has a hard limit. It is impossible to add unlimited CPU and memory to a single server.
- Vertical scaling does not have failover and redundancy. If one server goes down, the website/app goes down with it completely.

Horizontal scaling is more desirable for large scale applications due to the limitations of vertical scaling.

