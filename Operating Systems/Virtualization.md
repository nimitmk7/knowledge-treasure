Virtualization is a computing technology that ==creates virtual versions of physical hardware and devices==, such as servers, storage, and networks.

![[Pasted image 20241028205348.png]]

## Advantages
1. **Partitioning:** Run multiple OS on a single physical system and share the underlying hardware resources.
2. A virtual infrastructure gives administrators the advantage of **managing pooled resources** across the enterprise.
3. **Enablement of Server consolidation and Containment:** Eliminating ‘server sprawl’ via deployment of systems as virtual Machines(VMs) that can run safely and move transparently across shared hardware, and increase server utilization from 5-15% to 60-80%. Can also help save power.
4. **Test and Development Optimization:** Rapidly provisioning test and development servers by reusing pre-configured systems, enhancing developer collaboration and standardizing development environments.
5. **Improved Business Continuity:** Reducing the cost and complexity of business continuity(high availability and disaster recovery solutions) by encapsulating entire systems into single files that can be replicated and restored on any target server, thus minimizing downtime. 

> [!INFO] Server Sprawl 
>  Server sprawl is ==when an organization's IT infrastructure grows faster than the ability to manage it, resulting in underutilized servers that consume more resources than needed==. This can lead to poor productivity and increased costs

## Types

### Hardware Emulation
- **Most complex**: a hardware VM is created for each instance

### Full Virtualization
- Uses hypervisor to share underlying hardware across guest VMs
- Mediates between guest OS and underlying h/w

### Para-virtualization
- Differs from full virtualization in that integrates virtualization handling code into OS – thus the guest OS code is modified
### Operating system level virtualization
- Virtualizes server on top of operating systems
- Single OS that isolates the servers
## Hypervisor
A **hypervisor**, also known as a virtual machine monitor or VMM, is software that creates and runs virtual machines (VMs). A hypervisor allows one host computer to support multiple guest VMs by virtually sharing its resources, such as memory and processing.

### Why use a hypervisor?
Hypervisors make it possible to use more of a system’s available resources and provide greater IT mobility since the ==guest VMs are independent of the host hardware==. This means ==VMs can be easily moved between different servers==. Because multiple virtual machines can run off of one physical server with a hypervisor, a hypervisor reduces:

- Space
- Energy
- Maintenance requirements

## Memory Virtualization



## References
1. https://www.vmware.com/topics/hypervisor
2. 