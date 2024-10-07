## Introduction

### What is Borg ?
Google’s Borg system is a cluster manager that runs **hundreds of thousands of jobs**, from **many thousands of different applications**, across a **number of clusters** each with up to **tens of thousands of machines**.

It admits, schedules, starts, restarts and monitors the full range of applications that Google runs. 

## Main features
1. **Abstraction:** Hides the details of ==resource management== and ==failure handling==
2. **High reliability and availability**

> [!faq] What is the difference between Availability and reliability ?
>  Reliability measures the ability of a system to function correctly, including avoiding data corruption, whereas availability measures how often the system is available for use, even though it may not be functioning correctly. 
>  
>  In more detail, 
>  - **Reliability**: The probability that a system will meet its performance standards and produce the correct output at a specific time. Reliability aims to minimize system failures and downtime.
>  - **Availability**: The percentage of time a system is available for users. Availability aims to maximize operational time
3. **Effectively run workloads across huge number of machines**

## User Perspective
1. Users submit their work to Borg in the form of **jobs**.
2. Each **job** consists of one or more **tasks** that all run the same program (binary). 
3. Each job runs in one Borg **cell**, a set of machines that are managed as a unit.

### The workload
There are primarily 2 types of workload that Borg cells are supposed to run: 
1. **Long-running services**
	1. Designed to run continuously without interruption
	2. Handle short-lived, latency-sensitive requests
	3. Examples: Gmail, Google Docs, Search, BigTable
2. **Batch jobs**
	1. These are tasks that take anywhere from a few seconds to several days to complete
	2. They are less sensitive to short-term performance fluctuations

#### Key points about the workload
1. **Heterogeneity**: The workload is diverse, combining both types of tasks.
2. **Variability across cells:** Different Borg cells (which are essentially clusters) run different mixes of applications depending on their main users or tenants.
3. **Temporal variability**: The workload mix changes over time:
    - Batch jobs come and go
    - Many user-facing services show diurnal (daily) usage patterns
4. **Flexibility requirement**: Borg is expected to handle all these diverse scenarios equally well, adapting to the varying needs of different types of workloads, different cells, and changing patterns over time.

### Clusters and Cells

#### Cell
- The machines in a cell belong to a single **cluster**. 
- Our median cell size is about 10k machines(excluding test cells.)
- The machines in a cell are heterogeneous in many dimensions: 
	- **Sizes** (CPU, RAM, disk, network)
	- **Processor type**, 
	- **Performance**
	- **Capabilities** such as an external IP address or flash storage.
#### Cluster
- A cluster is defined by a high-performance, datacenter-scale network fabric connecting the machines.
- A cluster is physically located within a single datacenter building.
- Multiple datacenter buildings make up a site.
#### Borg’s role
Borg manages this heteregenous environment by:
1. Determining where in a cell to run tasks.
2. Allocating resources to tasks
3. Installing programs and dependencies
4. Monitoring health of tasks
5. Restarting tasks if they fail
#### Abstraction
- Borg isolates users from most of the differences in machine specifications and capabilities.
- This abstraction allows users to focus on their applications rather than worrying about the underlying hardware differences.
### Jobs and tasks

#### Job Properties
1. Name: A unique identifier
2. Owner: Entity respo
3. Number of tasks it has. 

#### Job Constraints
Jobs can have constraints to force its tasks to run on machines with particular attributes such as:
1. Processor architecture
2. OS version, or 
3. An external IP address. 

Constraints can be hard or soft.
	Hard constraint is like a requirement.
	Soft constraint is like a preference.

#### Job Start
The start of a job can be deferred until a prior one finishes, due to either:
- **Job dependencies**
- **Queuing or Resource Availability**

#### Cell-level execution of Job
A job runs in just one cell.
#### Tasks
A task is a unit of work within a job.

Each task maps to a set of Linux processes running in a container on a machine. The vast majority of the Borg workload does not run inside virtual machines (VMs).

#### Task properties
- **Resource Requirements**:
    - Each task specifies the exact amount of resources it needs. These resources include:
        - **CPU cores**
        - **RAM**
        - **Disk space**
        - **Disk access rate**
        - **TCP ports** (network requirements)
    - The resource requirements are specified with fine granularity, meaning that tasks are not forced into predefined buckets or slots. This gives greater flexibility in assigning resources.
- **Task Index**:
    - Each task has an **index** that represents its position or identifier within the job. This index is helpful for managing and distinguishing between tasks.
- **Overridable Properties**:
    - While many properties are shared across all tasks in a job, some can be **overridden** for individual tasks. For example, you might override a task's properties to provide **task-specific command-line flags**.

####  Borg programs
1. Borg programs are **statically linked**, meaning that the necessary libraries and dependencies are compiled directly into the program. 
	- This reduces reliance on the runtime environment, improving portability and consistency across different machines.
2. **Packages of Binaries and Data**:
    - Borg packages programs as **binaries and data files**, which are installed and orchestrated by the system. This packaging ensures that all necessary components are deployed together.

#### Job Operations via RPC
Users interact with jobs by issuing **Remote Procedure Calls (RPCs)** to Borg. These interactions are commonly done through:

- A **command-line tool**
- **Other Borg jobs** (jobs that manage or influence other jobs)
- **Monitoring systems**

Job descriptions are primarily written in **BCL** (Borg Configuration Language), which is a variant of **GCL** (Google Configuration Language).
#### Job States

![[Pasted image 20241001225035.png]]

#### Updating task properties.

A user can update properties of some or all of the tasks in a running job by pushing a new job configuration to Borg, and then instructing Borg to update the tasks to the new specification. 

##### Job Configuration Update

- A l**ight-weight non-atomic transaction** that can be easily undone until it is committed.
- Updates are generally done in a rolling fashion
- A limit can be imposed on the number of task disruptions an update causes(Any changes that would cause more disruptions are skipped.)

##### Impact of task updates
- **Task Restarts**: Some updates will **always require a task restart**, such as:
    - Pushing a **new binary** (executable code).
- **Task Rescheduling**: Some updates may cause the task to be **stopped and rescheduled** if the task can no longer run on its current machine. For example:
    - **Increasing resource requirements** (e.g., more CPU or memory).
    - **Changing constraints** (e.g., requiring the task to run on a machine with specific attributes like an IP address or OS version).
- **No Restart Needed**: Other updates can be done **without restarting or moving the task**, such as:
    - **Changing the priority** of the task.


##### Preemption and Notification Mechanism
- **Preemption**: Borg may preempt a task (stop it to give resources to a higher-priority task), which usually happens through a **SIGKILL** signal (a forceful termination).
- **SIGTERM Notification**: Before a task is preempted, it can ask to be notified via a **SIGTERM signal**. This gives the task time to:
    - **Clean up resources**.
    - **Save state** (so it can resume smoothly when restarted).
    - **Finish ongoing requests**.
    - **Decline new requests**.
- **Notice Timing**: Although the task may ask for a notification, the **actual time given** for cleanup depends on the **preemptor’s delay bound** (how much time the system allows before forcefully stopping the task).
    - In practice, **80% of the time**, tasks receive the notification and are able to perform the cleanup before being preempted.

### Allocs

#### Definition

- An **alloc** is a reserved set of resources on a machine (such as CPU, memory, disk space, etc.) where one or more tasks can run. The key aspect is that the resources are **reserved** for the alloc, meaning they stay assigned to the alloc whether they are actively being used or not.

#### **Uses of Alloc**

1. **Future Task Reservation**: Allocations can be used to set aside resources for **future tasks**, ensuring that when tasks are ready to be scheduled, the necessary resources are already reserved.
2. **Resource Retention**: If a task is stopped temporarily (e.g., for maintenance or updates), the resources within the alloc can be retained so that when the task starts again, it can use the same resources without needing to be rescheduled.
3. **Gathering Tasks on the Same Machine**: Allocations allow tasks from different jobs to share resources on the same machine. For example:
    - A **web server task** could run alongside a **logsaver task** on the same machine. The logsaver would collect logs from the web server’s local disk and copy them to a distributed file system.

#### Alloc as a Resource Container
- The resources within an alloc are treated similarly to the resources of a machine. If multiple tasks are running inside an alloc, they **share the alloc's resources**. This means the alloc acts like a container that encapsulates the resources needed by its tasks.

#### Rescheduling and Relocation
- If an alloc needs to be **relocated to another machine** (e.g., due to maintenance or a machine failure), all the tasks running inside that alloc will be **rescheduled with it**. This ensures continuity for the tasks and their resource reservations.

#### Alloc Set
- An **alloc set** is like a **job** but for allocations. It groups multiple allocs across different machines. The alloc set reserves resources on multiple machines, and once it’s created, one or more jobs can be submitted to run within it.
    - An alloc set gives users the ability to reserve resources across a cluster of machines and schedule jobs as needed within those reservations.

### Priority, quota and admission control
**Q. What happens when more work shows up than can be accommodated ?**
**Solution**: Priority, Quota

#### Priority
Every job has a *priority*, a small positive integer. Priority expresses relative importance for jobs that are running or waiting to run in a cell. 

A high-priority task can obtain resources at the expense of a lower-priority one, even if that involves preempting (killing) the lower priority task.

Borg defines non-overlapping priority bands for different uses, including (in decreasing-priority order): 

**Monitoring** > **Production** > **Batch** > **Best effort** (also known as testing or free). 

Jobs in the monitoring and production bands are also known as prod jobs.

##### Preemption Cascade
**Problem:** Although a preempted task will often be rescheduled elsewhere in the cell, preemption cascades could occur if a high-priority task bumped out a slightly lower-priority one, which bumped out another slightly-lower priority task, and so on.

**Solution**: Tasks in “production” priority band are not allowed to pre-empt one another.

#### Quota
It is used to decide which jobs to **admit** for scheduling. It's a mechanism to control and allocate resources among users.

##### Definition
- It's expressed as a vector of resource quantities: <CPU, RAM, Disk, etc.> at a given priority, for a period of time(typically months)

##### Specification
- Quotas define the ==maximum amount of resources that a user's jobs can request at any given time==.
- Example : "20TiB of RAM at prod priority from now until the end of July in cell xx"
	- This means the user can submit jobs requesting up to 20 TiB of RAM in total, at production priority, in the specified cell, until the end of July.

##### Admission Control:

Quota checking is part of the admission control process, not the scheduling process. When a job is submitted, it's immediately checked against the user's available quota.

If a job's resource requests exceed the user's available quota, it is immediately rejected upon submission, and doesn’t go to the scheduling queue.
##### Separation from Scheduling

The quota system operates independently from the actual scheduling of jobs.
It's a preliminary check to ensure users don't overwhelm the system with resource requests beyond their allocation.
##### Pricing
Higher priority quota costs more than quota at lower-priority. This pricing structure encourages efficient use of resources and helps manage demand.

##### Production-priority quota
- It's limited to the actual resources available in the cell.
- This ensures that if a user submits a production-priority job within their quota, it should be able to run.
- Exceptions may occur due to resource fragmentation or constraints.

##### User Behavior and Overbuy
- Users are encouraged to purchase only the quota they need.
- However, many users tend to overbuy quota as a buffer against future growth.
- This behavior is a form of insurance against potential resource shortages as their application's user base grows.

##### Overselling at Lower Priorities:
- To address overbuy behavior, Google oversells quota at lower priority levels.
- Every user has infinite quota at priority zero (the lowest priority).
- However, using this infinite low-priority quota can be challenging due to resource oversubscription.

##### Quota Allocation Process:
Quota allocation is handled outside of Borg and it's closely tied to physical capacity planning.
##### Capacity Planning Impact:
The results of capacity planning are reflected in:
a. The price of quota in different data-centers.
b. The availability of quota in different data-centers.

### Naming and Monitoring
