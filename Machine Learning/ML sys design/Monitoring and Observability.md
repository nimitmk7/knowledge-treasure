## Definition
Monitoring refers to the act of **tracking**, **measuring** and **logging** different **metrics** that can help us determine when something goes wrong. 

Observability means setting up our system in a way that gives us **visibility** into our system to **help us investigate** what went wrong. 

## Metrics
Monitoring is all about metrics. 

![[ml_system_metrics.png]]

### Operational Metrics
Because ML systems are software systems, the first class of metrics you’d need to monitor are operational metrics. These metrics are designed to convey the health of your systems. They are generally divided into 3 levels: 
1. Network metrics
2. Machine level metrics
3. Application level metrics

**Examples**: Latency, Throughput, The number of prediction requests your model receives in the last minute, hour, day, The percentage of requests that return with a 2xx code, CPU/GPU utilization, Memory utilization.

### ML-specific metrics

![[Pasted image 20240517174156.png]]
The deeper into the pipeline an artifact is, the more transformations it has gone through, which makes a change in that artifact more likely to be caused by errors in one of those transformations. 

However, the more transformations an artifact has gone through, the more structured it’s become and the closer it is to the metrics you actually care about, which makes it easier to monitor. 

#### Monitoring Accuracy metrics
- If the system receives any type of user feedback for the predictions it makes–click, hide, upvote, downvote, favorite, bookmark, share, etc. – you should definitely log and track it.
- Some feedback can be used to infer natural labels, which can then be used to calculate your model’s accuracy related metrics.

- Even if the feedback can’t be used to infer natural labels directly, it can be used to detect changes in your ML model’s performance.

> [!example]
> When you’re building a system to recommend to users what videos to watch next on YouTube, you want to track not only whether the users click on a recommended video (click-through rate), but also the duration of time users spend on that video and whether they complete watching it (completion rate). If, over time, the click-through rate remains the same but the completion rate drops, it might mean that your recommender system is getting worse.

- Its also possible to engineer your system so that you can collect users’ feedback. 

> [!example]
>  Google Translate has the option for users to upvote or downvote a translation, as shown in the figure. If the number of downvotes the system receives suddenly goes up, there might be issues. These downvotes can also be used to guide the labeling process, such as getting human experts to generate new translations for the samples with downvotes, to train the next iteration of their models.
![[Pasted image 20240517175357.png]]

#### Monitoring Predictions
- Most common artifact to monitor, easy to visualize, and straightforward to compute and interpret summary statistics.
- Can monitor to check [[Data Distribution Shift(DDS)]], prediction distribution shifts are also a proxy for input distribution shifts.
- Can monitor predictions for anything odd happening.

#### Monitoring Features
Changes in features: 
1. Input feature changes
2. Final feature(post-transformation) changes

Feature monitoring is appealing because compared to raw input data, features are well structured following a predefined schema. The first step of feature monitoring is feature validation: ensuring that your features follow an expected schema. The expected schemas are usually generated from training data or from common sense. If these expectations are violated in production, there might be a shift in the underlying distribution. 

For example, here are some of the things you can check for a given feature:
- If the min, max, or median values of a feature are within an acceptable range.
- If the values of a feature satisfy a regular expression format.
- If all the values of a feature belong to a predefined set.
- If the values of a feature are always greater than the values of another feature.

Further Read: 
1. https://aws.amazon.com/blogs/big-data/provide-data-reliability-in-amazon-redshift-at-scale-using-great-expectations-library/
2. https://aws.amazon.com/blogs/big-data/test-data-quality-at-scale-with-deequ/

![[Pasted image 20240517180927.png]]
##### Major Concerns
1. Excessively large number of features
2. Not useful for detecting model performance degradation
3. Feature extraction done in multiple steps, using multiple libraries on multiple services.
4. Change of feature schema over time

#### Monitoring raw inputs

The raw input data might not be easier to monitor, as ==it can come from multiple sources in different formats, following multiple structures==. 

The way many ML workflows are set up today also makes it impossible for ML engineers to get direct access to raw input data, as the raw input data is often managed by a data platform team who processes and moves the data to a location like a data warehouse, and the ML engineers can only query for data from that data warehouse where the data is already partially processed.

Therefore, monitoring raw inputs is often a responsibility of the data platform team, not the data science or ML team.

## Monitoring Toolbox

### Logs

### Dashboards

### Alerts

## Observability



