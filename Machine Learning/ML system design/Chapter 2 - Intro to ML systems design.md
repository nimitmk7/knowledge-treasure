If an ML system is built for a business, it must be driven by business objectives, which will need to be translated into ML objectives to guide the development of ML models.

We’ll consider the four requirements: reliability, scalability, maintainability, and adaptability. We will then introduce the iterative process for designing systems to meet those requirements.

Before using ML algorithms to solve your problem, you first need to frame your problem into a task that ML can solve.

## Business and ML Objectives

Most companies don’t care about the fancy ML metrics. They don’t care about increasing a model’s accuracy from 94% to 94.2% unless it moves some business metrics. 

A pattern seen in many short-lived ML projects is that the data scientists become too focused on hacking ML metrics without paying attention to business metrics.

The ultimate goal of any project within a business is, therefore, to increase profits, either directly or indirectly. For an ML project to succeed within a business organization, it’s crucial to tie the performance of an ML system to the overall business performance.

The effect of an ML project on business objectives can be hard to reason about.
For example, an ML model that gives customers more personalized solutions can make them happier, which makes them spend more money on your services. The same ML model can also solve their problems faster, which makes them spend less money on your services.

To gain a definite answer on the question of how ML metrics influence business metrics, experiments are often needed. Many companies do that with experiments like A/B testing and choose the model that leads to better business metrics, regardless of whether this model has better ML metrics.

When evaluating ML solutions through the business lens, it’s important to be realistic about the expected returns. Due to all the hype surrounding ML, generated both by the media and by practitioners with a vested interest in ML adoption, some companies might have the notion that ML can magically transform their businesses overnight.
Magically: possible. Overnight: no.

Returns on investment in ML depend a lot on the maturity stage of adoption. The longer you’ve adopted ML, the more efficient your pipeline will run, the faster your development cycle will be, the less engineering time you’ll need, and the lower your cloud bills will be, which all lead to higher returns

## Requirements for ML Systems

### Reliability
The system should ==continue to perform the correct function at the desired level of performance even in the face of adversity== (hardware or software faults, and even human error).

With traditional software systems, you often get a warning, such as a system crash or runtime error or 404. However, ==ML systems can fail silently==. End users don’t even know that the system has failed and might have kept on using it as if it were working.

### Scalability
The ML system should be able to handle growth in complexity, traffic volume or ML model count.

### Maintainability
It’s important to structure your workloads and set up your infrastructure in such a way that different contributors(DevOps, ML Engineers, Data Scientists, Subject Experts) can work using tools that they are comfortable with, instead of one group of contributors forcing their tools onto other groups.

1. Code should be documented.
2. Code, data and artifacts should be versioned.
3. Models should be sufficiently reproducible
4. Different contributors working together on a problem without finger-pointing

### Adaptability
To adapt to shifting data distributions and business requirements, the system should have some capacity for both discovering aspects for performance improvement and allowing updates without service interruption.

## Iterative Process
Developing an ML system is an iterative and, in most cases, never-ending process. Once a system is put into production, it’ll need to be continually monitored and updated.

![[Pasted image 20240524205713.png]]
## Framing ML Problems

![[Pasted image 20240524210238.png | 500]]

### Types of ML tasks
![[Pasted image 20240524210349.png]]
### Objective Functions
To learn, an ML model needs an objective function to ==guide the learning process==. An objective function is also called a loss function, because the objective of the learn‐ ing process is usually to minimize (or optimize) the loss caused by wrong predictions.

#### Decoupling Objectives
Framing ML problems can be tricky when you want to minimize multiple objective functions. What if the 2 objectives are in conflict with one another ?

##### Approach 1: Combine the loss functions

$$loss = \alpha \space loss_1 \space + \beta \space loss_{2}$$

You can randomly test out different values of α and β to find the values that work best. 

**Problem**: Each time you tune $\alpha$ and $\beta$ — for example, if the quality of your users’ newsfeeds goes up but users’ engagement goes down, you might want to decrease $\alpha$ and increase $\beta$ —you’ll have to retrain your model.

##### Approach 2: Train separate models
Train two different models, each optimizing one loss.

You can combine the models’ outputs and rank posts by their combined scores: 

$$score = \alpha \space score_1 \space + \beta \space score_{2}$$

Now you can tweak  $\alpha$ and $\beta$ without retraining your models!

> In general, when there are multiple objectives, it’s a good idea to decouple them first because it makes model development and maintenance easier.




