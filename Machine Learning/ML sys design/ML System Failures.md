A failure happens when one or more expectations of the system is violated. For an ML system, we care about 2 metrics: 

1. Operational Metrics: Latency, Availability, etc.
2. ML performance metrics: Accuracy, Error etc.

> ML performance expectation violations are harder to detect as doing so requires measuring and monitoring the performance of ML models in production.

A model that work well during development can fail in production primarily because of 2 types of failures:
![[ml_sys_f.png | 705]]

## Software System Failures

### Dependency Failures
A software package or codebase that our ML system depends on breaks, which leads our ML system to break. This failure mode is common when the dependency is maintained by a third party, and especially common if the third party that maintains the dependency no longer exists.
### Deployment Failures
Failures caused by deployment errors, such as 
- you accidentally deploy the binaries of an older version of your model instead of the current version, 
- your systems don’t have the right permissions to read or write certain files.
### Hardware Failures
When the hardware that you use to deploy your model, such as CPUs or GPUs, doesn’t behave the way it should. For example, the CPUs you use might overheat and break down.
### Downtime or Crashing
If a component of your system runs from a server somewhere, such as AWS or a hosted service, and that server is down, your system will also be down.
## ML specific failures

## [[Data Distribution Shift(DDS)]]

### Edge Cases
An ML Model which performs well on most cases but fails on a small number of edge cases might not be usable if these failures cause catastrophic consequences. 

Even though edge cases generally refer to data samples drawn from the same distribution, if there is a sudden increase in the number of data samples in which your model doesn’t perform well, it could be an indication that the underlying data distribution has shifted. 
### Degenerate Feedback loops
Feedback Loop = Time it takes from prediction being shown until the feedback on the prediction is provided. 

The feedback can be used to extract natural labels to evaluate the model’s performance and train the next iteration of the model. 

A *degenerate* feedback loop can happen when the predictions themselves influence the feedback, which, in turn, influences the next iteration of the model. 

More formally, a degenerate feedback loop is created when a system’s outputs are used to generate the system’s future inputs, which, in turn, influence the system’s future outputs.

In ML, a system’s predictions can influence how users interact with the system, and because users’ interactions with the system are sometimes used as training data to the same system, degenerate feedback loops can occur and cause unintended consequences. Degenerate feedback loops are especially common in tasks with natural labels from users, such as recommender systems and ads click-through-rate prediction.

Left unattended, degenerate feedback loops can cause your model to perform suboptimally at best. At worst, they can perpetuate and magnify biases embedded in data.
#### Example: Song Recommendation System

Songs ranked high by system → Shown first to users
Song shown first to users → Users click them more
Users click them more → System becomes more confident about the recommendations

#### Detecting Degenerate Feedback loops
For recommender systems,
- Metrics like aggregate diversity, average coverage of long-tail items
- Hit rate against popularity

#### Correcting Degenerate Feedback loops

1. Randomization
	- Show users random items and use their feedback to determine the true quality of these items.
	- Improves diversity, but at the cost of user experience.
2. Use of positional features
	- If the position in which a prediction is shown affects its feedback in any way, you might want to encode the position information using positional features(numerical or boolean).  ![[Pasted image 20240516172549.png]]
	- Use 2 different models. **Model 1** predicts the probability that the user will see and consider a recommendation taking into account the position at which that recom‐ mendation will be shown. **Model 2** then predicts the probability that the user will click on the item given that they saw and considered it. The second model doesn’t concern positions at all.

### Others
- Data collection and processing problems
- Poor hyperparameters
- Changes in training pipeline not correctly replicated in inference pipeline
