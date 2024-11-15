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

![[Docker_example.png]]

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

## Docker Engine

Docker Engine is the core technology that runs and manages containers. It's a client-server application consisting of:
1. A server (daemon process)
2. REST API for interacting with the daemon
3. Command-line interface (CLI) client

The Docker Engine creates and manages Docker objects such as images, containers, networks, and volumes.

It runs as a background service on the host operating system, handling container lifecycle operations.

## Dockerfile

A Dockerfile is a text document containing a ==series of instructions for building a Docker image==. 

It specifies:
1. The base image to use
2. Commands to update the base OS and install additional software
3. Source code to add to the image
4. The command to run when launching containers from the image
5. Dockerfiles automate the image creation process, ensuring consistency and reproducibility

Example of a dockerfile of a Python Flask App:
```
FROM python:3.8-slim-buster
WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0"]
```
## Docker images

Docker images are read-only templates used to create containers. 

They are:

1. Composed of layered filesystems
2. Built using instructions from a Dockerfile
3. Immutable once created
4. Shareable across different environments

Each image consists of a series of layers representing filesystem changes. 

Images leverage a Union File System to combine these layers into a single coherent filesystem.

## Docker Containers

Containers are runnable instances of Docker images. 

They are:
1. Isolated environments with their own processes, networks, and mounts
2. Created from images and can be started, stopped, moved, and deleted
3. Defined by the image used to create them and any configuration options provided
4. Containers share the host system's OS kernel but are isolated from each other and the host using namespaces and cgroups.
### Lifecycle

![[Pasted image 20241028200935.png]]

### Docker Hub
Docker Hub is a cloud-based registry service for:

1. Storing and distributing Docker images
2. Integrating with CI/CD pipeline
3. Automating builds from GitHub and Bitbucket

It offers both public repositories for open-source projects and private repositories for proprietary code. Docker Hub serves as the default public registry for Docker, though private registries can also be used.

## Basic Commands

- Pull an image: docker pull hello-world
- List images: docker images
- Run a container: docker run hello-world
- List running containers: docker ps
- List all containers (including stopped ones): docker ps -a
- Stop a container: docker stop <container_id>
- Start a stopped container: docker start <container_id>
- Remove a container: docker rm <container_id>
- View container logs: docker logs <container_id>
- Inspect a container: docker inspect container_id
- Execute a command in a running container: docker exec -it <container_id> /bin/bash

## Docker Volumes

Docker volumes are a mechanism for persisting data generated and used by Docker containers.
- Directories managed by Docker
- Stored on the host filesystem, outside the container's writable layer
- Can be shared and reused among multiple containers
- Persist data independently of the container's lifecycle

## Docker Compose
- Tool for defining and running multi-container Docker applications
- Uses YAML files to configure application services
- Simplifies the process of managing multiple containers as a single application

### docker-compose commands
- docker-compose up: Start services
- docker-compose down: Stop and remove containers
- docker-compose ps: List running services
- docker-compose logs: View output from containers