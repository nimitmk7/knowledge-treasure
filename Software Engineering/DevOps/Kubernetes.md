It is an open-source container orchestration platform that automates the deployment, scaling and management of containerized applications. 

> [!faq] What is Orchestration ?
>  **Orchestration** is the automated configuration, coordination, deployment, development, and management of computer systems and software.

## Features

1. **Container orchestration**: 
	1. Schedules and manages containers, ensuring they run in a reliable and scalable manner.
2. **Automated Scaling**
	1. It can automatically scale applications up or down based on demand.
3. **Load Balancing**: 
	1. Kubernetes can distribute traffic across multiple containers for high availability.
4. **Self Healing**:
	1. It detects and replaces failed containers to maintain application health.
5. **Rollouts and Rollbacks**
	1. Kubernetes facilitates controlled updates and rollbacks of application versions
6. **Resource Management**
	1. It efficiently allocates computing resources to containers.

## Architecture Overview
![[Pasted image 20241028104621.png]]
### Masters

They act as the primary control plane for Kubernetes. Masters are responsible
at a minimum for running the 
1. **API Server**
2. **Scheduler**, and 
3. **Cluster controller**. 

They commonly also manage 
1. **Storing cluster state**, 
2. **Cloud-provider specific components** 
3. Other cluster essential services.

#### Master Components

![[Pasted image 20241028104714.png | 400]]
##### Kube API Server
Serves as the ==central control point for the entire Kubernetes cluster==.

==Exposes the Kubernetes API==, which allows users, administrators, and other
components to interact with the cluster.

Accepts and processes RESTful API requests, which can include creating,
updating, and deleting resources like Pods, Services, ConfigMaps, and more.

Implements authentication, authorization, and admission control for security.

> [!faq] What is admission control ?
>  Admission control is a validation process that limits access to network resources to ensure that demand doesn't exceed supply.

#### etcd
A distributed and highly-available key-value store that acts as Kubernetes'
primary database.

 It ==stores all configuration data and cluster state information==.

##### Kube Controller Manager

Manages controllers that regulate the state of the system to match the desired
state. 

Includes various controllers like 
1. Replication Controller
2. ReplicaSet, and
3. StatefulSet

Ensures that the correct number of Pod replicas are running and healthy,
helping in fault tolerance.

Reconciles the current state of resources with the desired state specified in
the configuration, making necessary adjustments.

Helps in automatic scaling and self-healing of applications.

###### Replication Controller
A _ReplicationController_ ==ensures that a specified number of pod replicas are running at any one time==. In other words, a ReplicationController makes sure that a pod or a homogeneous set of pods is always up and available.

Too many pods → Terminate extra pods
Too few pods → Start more pods
Pods failed, deleted or terminated → Automatically replace

It is similar to a process supervisor, but instead of supervising individual processes on a single node, the ReplicationController supervises multiple pods across multiple nodes.

This example ReplicationController config runs three copies of the nginx web server.

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

ReplicaSet is the Next Generation ReplicationController. Supports set-based selectors.

###### StatefulSet
A StatefulSet runs a group of Pods, and maintains a sticky identity for each of those Pods. This is useful for managing applications that need persistent storage or a stable, unique network identity.

StatefulSet is the workload API object used to manage stateful applications.

StatefulSets are valuable for applications that require one or more of the following.

- Stable, unique network identifiers.
- Stable, persistent storage.
- Ordered, graceful deployment and scaling.
- Ordered, automated rolling updates.
##### Cloud controller Manager
The Cloud Controller Manager (CCM) is a component in a Kubernetes cluster that offloads cloud-specific functionality from the core Kubernetes control plane. 

It's primarily used in managed Kubernetes services provided by cloud providers (such as AWS EKS, GKE, or Azure AKS) and is designed to improve the separation of concerns between Kubernetes and cloud provider-specific operations.

##### Kube Scheduler
Monitors the API Server for new Pods without assigned nodes.

Selects an appropriate node for each Pod based on resource requirements, constraints, and policies.

Takes into account factors like 
1. CPU and memory availability
2. Anti-affinity rules
3. Node Capacity.

> [!faq] What are affinity rules ?
> 
> Affinity or Anti-affinity rules in Kubernetes are ==used to prevent pods from being scheduled on the same node or on different nodes==. This is useful for separating pods from each other, which can help with load balancing, system resilience, and high availability. 
> 
> In essence, they provide the ability to control pod placements relative to other pods.

### Nodes
They are the ‘workers’ of a Kubernetes cluster. They run a minimal agent that
manages the node itself, and are tasked with executing workloads as designated by the master.

####  Node and Pod 
 Pod is a Kubernetes abstraction that represents a group of one or more application containers (such as Docker), and some shared resources for those containers. Those resources include:

- Shared storage, as Volumes
- Networking, as a unique cluster IP address
- Information about how to run each container, such as the container image version or specific ports to use

A Pod models an application-specific "logical host" and can contain different application containers which are relatively tightly coupled. 

For example, a Pod might include both the container with your Node.js app as well as a different container that feeds the data to be published by the Node.js webserver. 

The containers in a Pod share an IP Address and port space, are always co-located and co-scheduled, and run in a shared context on the same Node.
  ![[Pasted image 20241028110837.png]]
 A Pod always runs on a **Node**. 
 
 A Node is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster. 
 
 Each Node is managed by the control plane. A Node can have multiple pods, and the Kubernetes control plane automatically handles scheduling the pods across the Nodes in the cluster. 
 
 The control plane's automatic scheduling takes into account the available resources on each Node.
#### Node components


![[Pasted image 20241028110028.png | 400]]
##### Kubelet

- **Pod Supervisor**: 
	- It acts as a Pod supervisor, ensuring that the containers defined within Pods are in the desired state.
- **Pod Lifecycle Management**: 
	- Kubelet starts, stops, and monitors containers within Pods. 
	- It takes instructions from the control plane to maintain the desired number of Pod replicas.
- **Health Checks**: 
	- Kubelet performs health checks on containers and can restart them if they fail.
- **Resource Management**: 
	- It monitors resource usage and reports this information to the control plane, which helps in making scheduling decisions.

##### Kube Proxy

- **Network Proxy**: 
	- Kube Proxy is responsible for network routing within the cluster.
- **Service Load Balancing**: 
	- It enables load balancing to ensure that service requests are distributed among available Pods.
- **Packet Forwarding**: 
	- Kube Proxy forwards network packets to the appropriate destination Pod based on service selectors.

##### Container Runtime

With respect to Kubernetes, A container runtime is a CRI (Container Runtime
Interface) compatible application that executes and manages containers. Examples are:
- Containerd (docker)
- Cri-o
- Rkt
- Kata (formerly clear and hyper)
- Virtlet (VM CRI compatible runtime)

A container runtime is ==a software component that runs containers on a host operating system==. It's also known as a container engine. Container runtimes are responsible for: 
1. loading container images from a repository
2. Monitoring local system resources
3. Isolating system resources for use of a container, and managing container lifecycle

Example: 
![[Pasted image 20241028111546.png]]

![[Pasted image 20241028111620.png]]
## Objects in Kubernetes

### Labels
Labels are key/value pairs that are attached to objects such as Pods. Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users.

Labels can be used to organize and to select subsets of objects. Labels can be attached to objects at creation time and subsequently added and modified at any time. 

Each object can have a set of key/value labels defined. Each Key must be unique for a given object.

```json
"metadata": {
  "labels": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
```

#### Example Labels
- `"release" : "stable"`, `"release" : "canary"`
- `"environment" : "dev"`, `"environment" : "qa"`, `"environment" : "production"`
- `"tier" : "frontend"`, `"tier" : "backend"`, `"tier" : "cache"`
- `"partition" : "customerA"`, `"partition" : "customerB"`
- `"track" : "daily"`, `"track" : "weekly"`

### Label Selectors

Unlike names and UIDs, labels do not provide uniqueness. In general, we expect many objects to carry the same label(s).

Via a label selector, the client/user can identify a set of objects. The label selector is the core grouping primitive in Kubernetes.

The API currently supports two types of selectors: 
1. equality-based 
2. set-based.

A label selector can be made of multiple _requirements_ which are comma-separated. In the case of multiple requirements, all must be satisfied so the comma separator acts as a logical _AND_ (`&&`) operator.

### Equality-based requirement
_Equality-_ or _inequality-based_ requirements allow filtering by label keys and values. Matching objects must satisfy all of the specified label constraints, though they may have additional labels as well.

Three kinds of operators are admitted $=$, $==$ , $!=$. 

The first two represent equality (and are synonyms), while the latter represents inequality. For example:

```
environment = production
tier != frontend
```

One usage scenario for equality-based label requirement is for Pods to specify node selection criteria. For example, the sample Pod below selects nodes where the `accelerator` label exists and is set to `nvidia-tesla-p100`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cuda-test
spec:
  containers:
    - name: cuda-test
      image: "registry.k8s.io/cuda-vector-add:v0.1"
      resources:
        limits:
          nvidia.com/gpu: 1
  nodeSelector:
    accelerator: nvidia-tesla-p100
```

### Set-based requirement
_Set-based_ label requirements allow filtering keys according to a set of values. Three kinds of operators are supported: `in`,`notin` and `exists` (only the key identifier). For example:

```
environment in (production, qa)
tier notin (frontend, backend)
partition
!partition
```

- The first example selects all resources with key equal to `environment` and value equal to `production` or `qa`.
- The second example selects all resources with key equal to `tier` and values other than `frontend` and `backend`, and all resources with no labels with the `tier` key.
- The third example selects all resources including a label with key `partition`; no values are checked.
- The fourth example selects all resources without a label with key `partition`; no values are checked.

Similarly the comma separator acts as an _AND_ operator. 

So filtering resources with a `partition` key (no matter the value) and with `environment` different than `qa` can be achieved using `partition,environment notin (qa)`. 

The _set-based_ label selector is a general form of equality since `environment=production` is equivalent to `environment in (production)`; similarly for `!=` and `notin`.

_Set-based_ requirements can be mixed with _equality-based_ requirements. For example: `partition in (customerA, customerB),environment!=qa`.
### Annotations
Key-value pairs that contain non-identifying information or
metadata. Annotations do not have the the syntax limitations as labels and can
contain structured or unstructured data.

Annotations, like labels, are key/value maps:

```json
"metadata": {
  "annotations": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
```

Here are some examples of information that could be recorded in annotations:

- Fields managed by a declarative configuration layer. Attaching these fields as annotations distinguishes them from default values set by clients or servers, and from auto-generated fields and fields set by auto-sizing or auto-scaling systems.
    
- Build, release, or image information like timestamps, release IDs, git branch, PR numbers, image hashes, and registry address.
    
- Pointers to logging, monitoring, analytics, or audit repositories.
    
- Client library or tool information that can be used for debugging purposes: for example, name, version, and build information.
    
- User or tool/system provenance information, such as URLs of related objects from other ecosystem components.
    
- Lightweight rollout tool metadata: for example, config or checkpoints.
    
- Phone or pager numbers of persons responsible, or directory entries that specify where that information can be found, such as a team web site.
    
- Directives from the end-user to the implementations to modify behavior or engage non-standard features.

## Deployments
A Deployment manages a set of Pods to run an application workload, usually one that doesn't maintain state.

You ==describe a desired state in a Deployment==, and ==the Deployment Controller changes the actual state to the desired state at a controlled rate==. 

You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.

The following is an example of a Deployment. It creates a ReplicaSet to bring up three `nginx` Pods:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

In this example:

- A Deployment named `nginx-deployment` is created, indicated by the `.metadata.name` field. This name will become the basis for the ReplicaSets and Pods which are created later. See [Writing a Deployment Spec](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#writing-a-deployment-spec) for more details.
    
- The Deployment creates a ReplicaSet that creates three replicated Pods, indicated by the `.spec.replicas` field.
    
- The `.spec.selector` field defines how the created ReplicaSet finds which Pods to manage. In this case, you select a label that is defined in the Pod template (`app: nginx`). However, more sophisticated selection rules are possible, as long as the Pod template itself satisfies the rule.
- 
> [!note]
> The `.spec.selector.matchLabels` field is a map of {key,value} pairs. A single {key,value} in the `matchLabels` map is equivalent to an element of `matchExpressions`, whose `key` field is "key", the `operator` is "In", and the `values` array contains only "value". All of the requirements, from both `matchLabels` and `matchExpressions`, must be satisfied in order to match.
    
- The `template` field contains the following sub-fields:
    
    - The Pods are labeled `app: nginx`using the `.metadata.labels` field.
    - The Pod template's specification, or `.template.spec` field, indicates that the Pods run one container, `nginx`, which runs the `nginx` [Docker Hub](https://hub.docker.com/) image at version 1.14.2.
    - Create one container and name it `nginx` using the `.spec.template.spec.containers[0].name` field.

Ref: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
## Service
In Kubernetes, a Service is a method for exposing a network application that is running as one or more Pods in your cluster.

A key aim of Services in Kubernetes is that you don't need to modify your existing application to use an unfamiliar service discovery mechanism. You can run code in Pods, whether this is a code designed for a cloud-native world, or an older app you've containerized. You use a Service to make that set of Pods available on the network so that clients can interact with it.

The Service API, part of Kubernetes, is an abstraction to help you expose groups of Pods over a network. Each Service object defines a logical set of endpoints (usually these endpoints are Pods) along with a policy about how to make those pods accessible.

For example, consider a stateless image-processing backend which is running with 3 replicas. Those replicas are fungible—frontends do not care which backend they use. While the actual Pods that compose the backend set may change, the frontend clients should not need to be aware of that, nor should they need to keep track of the set of backends themselves.

The Service abstraction enables this decoupling.

The set of Pods targeted by a Service is usually determined by a [selector](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) that you define. 

### Defining a Service

For example, suppose you have a set of Pods that each listen on TCP port 9376 and are labelled as `app.kubernetes.io/name=MyApp`. You can define a Service to publish that TCP listener

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

### Service Types

For some parts of your application (for example, frontends) you may want to expose a Service onto an external IP address, one that's accessible from outside of your cluster.

Kubernetes Service types allow you to specify what kind of Service you want.

The available `type` values and their behaviors are:

#### ClusterIP

Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default that is used if you don't explicitly specify a `type` for a Service

#### NodePort

Exposes the Service on each Node's IP at a static port (the `NodePort`). To make the node port available, Kubernetes sets up a cluster IP address.

#### Load Balancer

Exposes the Service externally using an external load balancer. Kubernetes does not directly offer a load balancing component; you must provide one, or you can integrate your Kubernetes cluster with a cloud provider.

#### ExternalName

Maps the Service to the contents of the `externalName` field (for example, to the hostname `api.foo.bar.example`). The mapping configures your cluster's DNS server to return a `CNAME` record with that external hostname value. No proxying of any kind is set up.


## Ingress

Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

![[Pasted image 20241028163724.png]]

## Storage

### Background
Kubernetes supports many types of volumes. A [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) can use any number of volume types simultaneously.

[Ephemeral volume](https://kubernetes.io/docs/concepts/storage/ephemeral-volumes/) types have a lifetime of a pod, but [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) exist beyond the lifetime of a pod. 

When a pod ceases to exist, Kubernetes destroys ephemeral volumes; however, Kubernetes does not destroy persistent volumes. ==For any kind of volume in a given pod, data is preserved across container restarts.==

At its core, a volume is a directory, possibly with some data in it, which is accessible to the containers in a pod. How that directory comes to be, the medium that backs it, and the contents of it are determined by the particular volume type used.

### Volume
A Storage that is tied to the Pod Lifecycle, consumable by one or more containers within the pod.

### PersistentVolume
A _PersistentVolume_ (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using [Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/). It is a resource in the cluster just like a node is a cluster resource

PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV. This API object captures the details of the implementation of the storage, be that NFS, iSCSI, or a cloud-provider-specific storage system.

#### PersistentVolumeClaim 

A PersistentVolumeClaim (PVC) is a request for storage by a user, similar to a pod. Pods consume node resources, while PVCs consume PV resources.

 Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., they can be mounted ReadWriteOnce, ReadOnlyMany, ReadWriteMany, or ReadWriteOncePod, see [AccessModes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes)).

While PersistentVolumeClaims allow a user to consume abstract storage resources, it is common that users need PersistentVolumes with varying properties, such as performance, for different problems. Cluster administrators need to be able to offer a variety of PersistentVolumes that differ in more ways than size and access modes, without exposing users to the details of how those volumes are implemented.

## Configuration
### General Configuration Tips

- When defining configurations, specify the latest stable API version.
    
- Configuration files should be stored in version control before being pushed to the cluster. This allows you to quickly roll back a configuration change if necessary. It also aids cluster re-creation and restoration.
    
- Write your configuration files using YAML rather than JSON. Though these formats can be used interchangeably in almost all scenarios, YAML tends to be more user-friendly.
- Group related objects into a single file whenever it makes sense. One file is often easier to manage than several. See the [guestbook-all-in-one.yaml](https://github.com/kubernetes/examples/tree/master/guestbook/all-in-one/guestbook-all-in-one.yaml) file as an example of this syntax.
- Note also that many `kubectl` commands can be called on a directory. For example, you can call `kubectl apply` on a directory of config files.
- Don't specify default values unnecessarily: simple, minimal configuration will make errors less likely.
- Put object descriptions in annotations, to allow better introspection.

### ConfigMaps

A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

A ConfigMap allows you to decouple environment-specific configuration from your container images, so that your applications are easily portable.

Use a ConfigMap for setting configuration data separately from application code.

For example, imagine that you are developing an application that you can run on your own computer (for development) and in the cloud (to handle real traffic). You write the code to look in an environment variable named `DATABASE_HOST`. Locally, you set that variable to `localhost`. In the cloud, you set it to refer to a Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/) that exposes the database component to your cluster. This lets you fetch a container image running in the cloud and debug the exact same code locally if needed.

### Secrets

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Using a Secret means that you don't need to include confidential data in your application code.

Because Secrets can be created independently of the Pods that use them, there is less risk of the Secret (and its data) being exposed during the workflow of creating, viewing, and editing Pods. Kubernetes, and applications that run in your cluster, can also take additional precautions with Secrets, such as avoiding writing sensitive data to nonvolatile storage.

Secrets are similar to [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/) but are specifically intended to hold confidential data. They are stored encoded as base64, and encrypted at rest (if configured).

### Liveness, Readiness, and Startup Probes

> [!faq] What is a probe ?
In Kubernetes, a "probe" is ==a mechanism used to periodically check the health and readiness of a container or application running within a pod==, essentially acting as a health check to determine if the application is functioning properly and ready to receive traffic; it's like "probing" the application to see if it's alive and responsive.
#### Liveness Probe
- Determines if a container is operating correctly.
- If the probe fails repeatedly, the kubelet automatically restarts the container
- Helps catch issues like deadlocks where an application is running but unable to make progress.

#### Readiness Probe
- Determine when a container is ready to start accepting traffic.
- If the readiness probe returns a failed state, Kubernetes removes the pod from all matching service endpoints.
- Useful when waiting for an application to perform time-consuming initial tasks, such as establishing network connections, loading files, and warming caches.

#### Startup Probe
- Verifies whether the application within a container is started. 
- This can be used to adopt liveness checks on slow starting containers, avoiding them getting killed by the kubelet before they are up and running.
- If such a probe is configured, it ==disables liveness and readiness checks until it succeeds==.
- ==Only executed at startup==, unlike liveness and readiness probes, which are run periodically.

> [!faq] Who runs these probes ?
> ==The kubelet==, the primary node agent that runs on each node in a Kubernetes cluster, manages the probes.

**Example:**
```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-container
        image: your-flask-image:latest
        ports:
        - containerPort: 5000
        livenessProbe:
          httpGet:
            path: /healthz
            port: 5000
          initialDelaySeconds: 5
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 5000
          initialDelaySeconds: 5
          periodSeconds: 10
```

#### Customizable probe parameters
- **`initialDelaySeconds`**: The initial wait time before the first probe is initiated.
- **`periodSeconds`**: How often the probe is executed.
- **`timeoutSeconds`**: Timeout for the probe to respond.
- **`failureThreshold`**: Number of consecutive failures for the probe to be considered failed.
- **`successThreshold`**: Number of consecutive successes required before the probe is considered successful.(default Value = 1)

## References
1. https://kubernetes.io/docs
2. 

