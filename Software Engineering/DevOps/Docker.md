A platform for building, running and shipping applications, in a consistent manner.

## Motivation/Problem Statement 

**"It works on my machine issue"**
### Reasons
1. Files Missing
2. Software version mismatch
3. Different configuration settings and environment variables

### Solution
By adopting Docker, the problem of inconsistent development and deployment environments can be effectively resolved. Docker containers ensure consistency and simplify the deployment process.
## Core Idea: Containerization

Applications running in containers can be deployed easily to multiple different operating systems and hardware platforms.

DevOps teams know applications in ==containers will run the same, regardless of where they are deployed.== Containers allow applications to be more rapidly deployed, patched, or scaled.

## Example
- We are building an application, so we will package it along with everything it needs(dependencies) in a container.
- Now we can run this container on any machine with Docker.

![[43c8dc64bbf0d8062b35517a2138b896_MD5.jpeg]]

On new machine, when docker is called upon to bring up the app, Docker will automatically
download and run these dependencies inside an **isolated environment**, called
a container.

![[Pasted image 20240227154449.png]]

When we don't need an app, we can remove the app and its dependencies in one go.

## Virtual Machines vs Containers

**VM** : Abstraction of a machine
**Container**: Isolated environment for running an application

### Problems with VMs
1. **SLOW START**: Each VM needs a full-blown OS, to work.
2. **RESOURCE INTENSIVE**: As each VM takes a slice of the actual physical hardware(CPU, RAM, Disk)
3. **INSTANCE LIMIT**: The limit for the number of VMs to run on a single machine is low.
4. **NO HARDWARE RESOURCE SHARING**

### Container Benefits
1. Allow running multiple apps in isolation.
2. Lightweight
3. Use host OS(**Quick Start**)
4. Need less hardware resource and share the hardware.

## Architecture


