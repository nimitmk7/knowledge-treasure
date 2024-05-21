Infrastructure, when setup right, can help automate processes, reducing the need for specialized knowledge and engineering time. This in turn, can speed up the development and delivery of ML applications, reduce the surface area for bugs, and enable new use cases. 

When setup wrong, however, infrastructure is painful to use and expensive to replace. 

Every company’s infrastructure needs are different. The infrastructure required for you depends on the number of applications you develop and how specialized the applications are.

![[Pasted image 20240518140519.png]]

**What is ML infra?**
The set of fundamental facilities that support the development and maintenance of ML systems. 

## 4 Layers of ML infra

1. Storage and Compute
2. Resource Management
3. ML Platform
4. Development Environment
![[Pasted image 20240518140735.png]]

## Storage and Compute

#### Storage Layer
ML systems work with a lot of data, and this data needs to be stored somewhere. The *storage layer* is where data is collected and stored. 

**Simplest Form**: HDD/SDD, or
**One location in Cloud**: Amazon S3/Snowflake, or
**Data spread over multiple locations**

#### Compute Layer
The *compute layer* refers to all the compute resources a company has access to and the mechanism to determine how these resources can be used. The amount of compute resources available **determines the scalability of your workloads**. You can think of the *compute layer* as the engine to execute your jobs.

**Simplest Form**: single CPU core or a GPU core
**Most Common Form**: Managed Cloud compute AWS EC2 or GCP

The compute layer can usually be sliced into smaller compute units to be used concurrently. 

##### Compute Metrics
1. Memory Size (in GB)
2. Execution Speed (in FLOPS)

### Public Cloud vs Private Data Centers

Instead of setting up their own data centers for storage and compute, companies can pay cloud providers like AWS, GCP and Azure for the exact amount of compute they use. 

Cloud compute makes it extremely easy for companies to start building without having to worry about the compute layer. ==It’s especially appealing to companies that have variable-sized workloads==. 

It’s convenient to be able to just add more compute or shut down instances as needed—most cloud providers even do that automatically for you—reducing engineering operational overhead. This is especially useful in ML as ==data science workloads are bursty==. 

Data scientists tend to run experiments a lot for a few weeks during development, which requires a surge of compute power. Later on, during production, the workload is more consistent.

> Cloud compute is elastic but not magical. It doesn’t actually have infinite compute. Most cloud providers offer limits on the compute resources you can use at a time.

While getting started with the cloud is easy, moving away from the cloud is hard. Cloud repatriation requires nontrivial up-front investment in both commodities and engineering effort. 
## Development Environment 
The dev environment is where ML engineers write code, run experiments, and interact with the production environment where champion models are deployed and challenger models evaluated. 

The dev environment consists of the following components: 
1. IDE (integrated development environment)
2. Versioning
3. CI/CD.
### Dev Env Setup

#### IDE
The IDE is the editor where you write your code. IDEs tend to support multiple programming languages. IDEs can be native apps like VS Code or Vim. IDEs can be browser-based, which means they run in browsers, such as AWS Cloud9.

Many data scientists write code not just in IDEs but also in notebooks like Jupyter Notebooks and Google Colab. Notebooks are more than just places to write code. You can include arbitrary artifacts such as images, plots, data in nice tabular formats, etc., which makes notebooks ==very useful for exploratory data analysis and analyzing model training results==.

Notebooks have a nice property: they are stateful—they can retain states after runs. If your program fails halfway through, you can rerun from the failed step instead of having to run the program from the beginning. This is especially helpful when you have to deal with large datasets that might take a long time to load. With notebooks, you only need to load your data once—notebooks can retain this data in memory— instead of having to load it each time you want to run your code.

![[Pasted image 20240518210840.png]]
Note that this statefulness can be a double-edged sword, as it allows you to execute your cells out of order. For example, in a normal script, cell 4 must run after cell 3 and cell 3 must run after cell 2. However, in notebooks, you can run cell 2, 3, then 4 or cell 4, 3, then 2. This makes notebook reproducibility harder unless your notebook comes with an instruction on the order in which to run your cells.

![[Pasted image 20240518210938.png]]

### Standardizing Dev Environments
The first thing about the dev environment is that it should be standardized, if not company-wide, then at least team-wide. 

One of the best ways to do so is move to a cloud-based dev environment. Moving from local dev environments to cloud dev environments has many other benefits. 

1. It makes IT support so much easier—imagine having to support 1,000 different local machines instead of having to support only one type of cloud instance. 
2. It’s convenient for remote work—you can just SSH into your dev environment wherever you go from any computer. 
3. Cloud dev environments can help with security. For example, if an employee’s laptop is stolen, you can just revoke access to cloud instances from that laptop to prevent the thief from accessing your codebase and proprietary information. 
4. Having your dev environment on the cloud reduces the gap between the dev environment and the production environment. 

> [!WARNING] 
> Cloud dev environments might not work for every company due to cost, security, or other concerns. Setting up cloud dev environments also requires some initial investments, and you might need to educate your data scientists on cloud hygiene, including establishing secure connections to the cloud, security compliance, or avoiding wasteful cloud usage.

### From Dev to Prod: Containers

During development, you might usually work with a fixed number of machines or instances (usually one) because your workloads don’t fluctuate a lot. A production service, on the other hand, might be spread out on multiple instances. The number of instances changes from time to time depending on the incoming workloads, which can be unpredictable at times.

You will have to turn on new instances as needed, and these instances will need to be set up with required tools and packages to execute your workloads. When a new instance is allocated for your workload, you’ll need to install dependencies using a list of predefined instructions.

> **How do you re-create an environment on any new instance?**
> Container technology

[[Docker]]

If your application does anything interesting, you will probably need more than one container, like different services having different execution times and memory requirements. 

Different containers might also be necessary when different steps in your pipeline have conflicting dependencies.  Manually building, running, allocating resources for, and stopping lots of containers might be a painful chore. A tool to help you manage multiple containers is called **container orchestration**. Docker Compose is a lightweight container orchestrator that can manage containers on a single host.

However, each of your containers might run on its own host, and this is where Docker Compose is at its limits. [[Kubernetes]] is a tool for exactly that. K8s creates a network for containers to communicate and share resources. It can help you spin up containers on more instances when you need more compute/memory as well as shutting down containers when you no longer need them.
## Resource Management

- Pre-cloud world: Finite compute and storage
	- Focus: “Make the most out of limited resources”
- Cloud world: Elastic compute and storage
	- Focus: “How to use resources cost-effectively”
	- Companies are OK with adding more resources to an application as long as the added cost is justified by the return(extra revenue/saved engineering time)
### Cron, Schedulers and Orchestrators

2 key characteristics of ML workflows that influence their resource management
1. Repetitiveness
	- E.g. Training a model every week, Generating a batch of predictions every few hours
2. Dependencies
	- E.g. Training a new model after pulling the latest data from warehouse.

Scheduling repetitive jobs to run at fixed times is exactly what cron does. This is also all that **cron** does: run a script at a predetermined time and tell you whether the job succeeds or fails.

**Problem**: It doesn’t care about the dependencies between the jobs it runs, so can’t run anything complex. 

Steps in an ML workflow might have complex dependency relationships with each other. 
Example:
![[Pasted image 20240519100520.png]]
This is where schedulers come into the picture. 

#### Schedulers
**Schedulers** are cron programs that can handle dependencies. It takes in the DAG of a workflow and schedules each step accordingly. You can even schedule to start a job based on an event-based trigger, e.g., start a job whenever an event X happens. 

Schedulers also allow you to specify what to do if a job fails or succeeds, e.g., if it fails, how many times to retry before giving up.

Schedulers tend to ==leverage queues to keep track of jobs==. Jobs can be queued, prioritized, and allocated resources needed to execute. This means that schedulers need to be aware of the resources available and the resources needed to run each job—the resources needed are either specified as options when you schedule a job or estimated by the scheduler. 

Schedulers should also optimize for resource utilization since they have information on resources available, jobs to run, and resources needed for each job to run. 

#### Orchestrators
If schedulers are concerned with when to run jobs and what resources are needed to run those jobs, **orchestrators** are concerned with where to get those resources.

Schedulers deal with job-type abstractions such as DAGs, priority queues, user-level quotas(i.e. the maximum number of instances a user can use at a given time), etc. 

Whereas, Orchestrators deal with lower-level abstractions like **machines**, **instances**, **clusters**, **service-level grouping**, **replication**, etc. If the orchestrator notices that there are more jobs than the pool of available instances, it can increase the number of instances in the available instance pool. 

> Schedulers are often used for periodical jobs, whereas orchestrators are often used for services where you have a long-running server that responds to requests.

### Data Science Workflow Management

In its simplest form, workflow management tools manage workflows. They generally allow you to specify your workflows as DAGs. 
A workflow might consist of:
1. Featurizing step
2. Model training step,
3. Evaluation step. 

Workflows can be defined using either code (Python) or configuration files (YAML). Each step in a workflow is called a task.

Almost all workflow management tools come with some schedulers, and therefore, you can think of them as schedulers that, instead of focusing on individual jobs, focus on the workflow as a whole. Once a workflow is defined, the underlying scheduler usually works with an orchestrator to allocate resources to run the workflow.

![[Pasted image 20240519102303.png]]
## ML platform

Components: 
1. Model deployment
2. Model Store
3. Feature Store

### Tool Evaluation
General Aspects:
1. Whether the tool works with your cloud provider or allows you to use it on your own data center
2. Whether it’s open source or a managed service
### Model Deployment
Once a model is trained, you want to make its predictive capability accessible to users. The simplest way to deploy a model is push it with its dependencies to a location accessible in production and then expose the model as an endpoint to your users. 

**Online Prediction**: Provoke the model to generate a prediction
**Batch Prediction:** Fetch a pre-computed prediction

A deployment service can help with both ==pushing your models and their dependencies to production== and ==exposing your models as endpoints==.

> [!WARNING] Ease of use of tools for online vs batch prediction
> It is usually straightforward to do online prediction at a smaller scale with most deployment services, doing batch prediction is usually trickier. Batch predictions requires setting up batch jobs and storing predictions.
> 
> Some tools allow you to batch requests together for online prediction, which is different from batch prediction. Many companies have separate deployment pipelines for online prediction and batch prediction. 
### Model Store
Model store suggests that it stores models—you can do so by uploading your models to storage like S3. However, it’s not quite that simple. 

Many companies have realized that storing the model alone in blob storage isn’t enough. To help with debugging and maintenance, it’s important to track as much information associated with a model as possible.

Here are the 8 types of artifacts you might want to store: 
- **Model Definition**
	- Information needed to create the shape of the model(loss function, no. of hidden layers, no of parameters in each hidden layer)
- **Model parameters**
	- Actual values of the parameters of your model. These values are then combined with the model’s shape to re-create a model that can be used to make predictions. 
- **Featurize and predict functions**
	- How do you extract features and input these features into the model to get back a prediction? 
- **Dependencies**
- **Data**
	- Pointers to the location where the data is stored or the name/version of your data.
- **Model generation code**
	- This is the code that specifies how your model was created, such as:
		- What frameworks it used
		- How it was trained
		- The details on how the train/valid/test splits were created
		- The number of experiments run
		- The range of hyperparameters considered
		- The actual set of hyperparameters that final model used
- **Experiment Artifacts**
- **Tags
	- To help with model discovery and filtering, such as owner (the person or the team who is the owner of this model) or task (the business problem this model solves, like fraud detection).

### Feature Store

3 problems addressed by Feature Store: 
1. Feature Management
2. Feature Transformation
3. Feature Consistency

#### Feature Management
A company might have multiple ML models, each model using a lot of features. A feature store can help teams share and discover features, as well as manage roles and sharing settings for each feature.

#### Feature Computation
Feature engineering logic, after being defined, needs to be computed. 

If the computation of this feature isn’t too expensive, it might be acceptable computing this feature each time it is required by a model. However, if the computation is expensive, you might want to execute it only once the first time it is required, then store it for feature uses.

A feature store can help with both performing feature computation and storing the results of this computation. In this capacity, a feature store acts like a data warehouse.

#### Feature Consistency
A key selling point of modern feature stores is that they unify the logic for both batch features(used for training) and streaming features(used for test), ensuring the consistency between features during training and features during inference.




















