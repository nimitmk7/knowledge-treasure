The algorithm is only a small part of an ML system in production. 

The system also includes the business requirements that gave birth to the ML project in the first place, the interface where users and developers interact with your system, the data stack, and the logic for developing, monitoring, and updating your models, as well as the infrastructure that enables the delivery of that logic. 

![[Pasted image 20240129082622.png]]

## When to Use Machine Learning 
Before starting an ML project, you might want to ask whether ML is necessary or cost-effective.

To understand what ML can do, let’s examine what ML solutions generally do: 

Machine learning is an approach to learn complex patterns from existing data 
and use these patterns to make predictions on unseen data.

1. **Learn**: System has the capacity to learn(In most cases, from data)
2. **Complex Patterns**: There are patterns to learn, and they are complex
	1. Whether a pattern exists might not be obvious, or if patterns exist, your dataset or ML algorithms might not be sufficient to capture them
	2. Also, what is complex to machines is different from what is complex to humans.
	3. Patterns are different from distributions. We know the distribution of the outcomes of a fair die, but there are no patterns in the way the outcomes are generated.
	![[Pasted image 20240129084014.png]]
3. **Existing Data**: Data is available, or its possible to collect data
	1. It’s also possible to launch an ML system without data. For example, in the context of continual learning, ML models can be deployed without having been trained on any data, but they will learn from incoming data in production. However, serving insufficiently trained models to users comes with certain risks, such as poor customer experience.
	2. Without data and without continual learning, many companies follow a “fake-it- til-you make it” approach: launching a product that serves predictions made by humans, instead of ML models, with the hope of using the generated data to train ML models later.
4. **Predictions**: Its a predictive problem
	1. ML models make predictions, so they can only solve problems that require predictive answers. ML can be especially appealing when you can benefit from a large quantity of cheap but approximate predictions
	2. As predictive machines (e.g., ML models) are becoming more effective, more and more problems are being reframed as predictive problems.
	3. Compute-intensive problems are one class of problems that have been very successfully reframed as predictive. Instead of computing the exact outcome of a process, which might be even more computationally costly and time-consuming than ML, you can frame the problem as: *“What would the outcome of this process look like?”* and approximate it using an ML model. The output will be an approximation of the exact output, but often, it’s good enough.
5. **Unseen data**: Unseen data shares patterns with the training data
	1. It means your unseen data and training data should come from similar distributions.
	2. "*But if the data is unseen, how do we know about its distribution?*" 
		1. We can make reasonable assumptions and hope that they hold. E.g. we can assume that users’ behaviors tomorrow won’t be too different from users’ behaviors today.
6. **Its repetitive**: Despite exciting progress in few-shot learning research, most ML algorithms still require many examples to learn a pattern. When a task is repetitive, each pattern is repeated multiple times, which makes it easier for machines to learn it.
7. **The cost of wrong predictions is cheap**
8. **Its at scale**: 
	1. Making a lot of predictions
	2. A problem might appear to be a singular prediction, but it’s actually a series of predictions. For example, a model that predicts who will win a US presidential election seems like it only makes one prediction every four years, but it might actually be making a prediction every hour or even more frequently because that prediction has to be continually updated to incorporate new information.
	3. Also, there's a lot of data for you to collect, which is useful for training ML models.
9. **The patterns are constantly changing**:
	1. If your problem involves one or more constantly changing patterns, hardcoded solutions such as handwritten rules can become outdated quickly. 
	2. Because ML learns from data, you can update your ML model with new data without having to figure out how the data has changed. 

### Conditions to not use ML:
1. Its unethical.
2. Simpler solutions do the trick
3. Its not cost-effective.

However, even if ML can’t solve your problem, it might be possible to break your problem into smaller components, and use ML to solve some of them.

E.g. If you can’t build a chatbot to answer all your customers’ queries, it might be possible to build an ML model to predict whether a query matches one of the frequently asked questions. If yes, direct the customer to the answer. If not, direct them to customer service.

## Machine Learning Use cases insights

- There are many exceptions, but for most cases, enterprise applications might have stricter accuracy requirements but be more forgiving with latency requirements. At the same time, latency of a second might get a consumer distracted and opening something else, but enterprise users might be more tolerant of high latency.
- ![[Pasted image 20240129085654.png]]

## Understanding ML systems

### ML in Research vs ML in Production

![[Pasted image 20240129085903.png]]

#### Different Stakeholders and requirements

- To edge out a small improvement in performance, researchers often resort to techniques that make models too complex to be useful.
- There are many stakeholders involved in bringing an ML system into production. Each stakeholder has their own requirements. Having different, often conflicting, requirements can make it difficult to design, develop, and select an ML model that satisfies all the requirements.
- When developing an ML project, it’s important for ML engineers to understand requirements from all stakeholders involved and how strict these requirements are.
- Production having different requirements from research is one of the reasons why successful research projects might not always be used in production
#### Computational Priorities

- When designing an ML system, people who haven’t deployed an ML system often make the mistake of focusing too much on the model development part and not enough on the model deployment and maintenance part.
- During model development, training is the bottleneck. Once the model has been deployed, however its job is to generate predictions, so inference is the bottleneck. *Research usually prioritizes fast training, whereas production usually prioritizes fast inference*.
- Research prioritizes high throughput whereas production prioritizes low latency.

 ![[Pasted image 20240129090703.png]]
 - To reduce latency in production, you might have to reduce the number of queries you can process on the same hardware at a time. If your hardware is capable of processing many more queries at a time, using it to process fewer queries means underutilizing your hardware, increasing the cost of processing each query

##### Latency considerations

- Latency is not an individual number, but a distribution. Its usually better to think in percentiles, as they tell you something about a certain percentage of your requests. The most commonly used is p50(50th percentile).
- Higher percentiles help you detect outliers, which might be symptoms of something wrong. They are important to look at because even though they account for a small percentage of users, sometimes they can be the most important users.
#### Data

- During the research phase, the datasets you work with are often clean and well-formatted, freeing you to focus on developing models. 

- In production, data, if available, is a lot more messy. It’s noisy, possibly unstructured, constantly shifting. It’s likely biased, and you likely don’t know how it’s biased. Labels, if there are any, might be sparse, imbalanced, or incorrect. Changing project or business requirements might require updating some or all of your existing labels

- ![[Pasted image 20240129091236.png]]

#### Fairness

During the research phase, a model is not yet used on people, so it’s easy for researchers to put off fairness as an afterthought: “Let’s try to get state of the art first and worry about fairness when we get to production.” When it gets to production, its too late.

ML algorithms don’t predict the future, but encode the past, thus perpetuating the biases in the data and more. When ML algorithms are deployed at scale, they can discriminate against people at scale. If a human operator might only make sweeping judgments about a few individuals at a time, an ML algorithm can make sweeping judgments about millions in split seconds. 
#### Interpretability

Since most ML research is still evaluated on a single objective, model performance, researchers aren’t incentivized to work on model interpretability. 

However, interpretability isn’t just optional for most ML use cases in the industry, but a requirement.

1. interpretability is important for users, both business leaders and end users, to understand why a decision is made so that they can trust a model and detect potential biases mentioned previously.

2. It’s important for developers to be able to debug and improve a model.

## ML Systems vs Traditional Software

In SWE, there is an underlying assumption that code and data are separated. In fact, in SWE, we want to keep things as modular and separate as possible.

On the contrary, ML systems are part code, part data and part artifacts create from the data.

In traditional SWE, you only need to focus on testing and versioning your code. With ML, we have to test and version our data too, and that’s the hard part. 

The size of ML Models is another challenge. Then there is the question of how to get these models to run fast enough to be useful. 

Monitoring and debugging these models in production is also non-trivial. As ML models get more complex, coupled with the lack of visibility into their work, it’s hard to figure out what went wrong or be alerted quickly enough when things go wrong.


