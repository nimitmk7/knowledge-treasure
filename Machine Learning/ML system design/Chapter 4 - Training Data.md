We use the term "training data" instead of "training dataset" because "dataset" denotes a set that is finite and stationary. Data in production is neither finite nor stationary.

Like other steps in building ML systems, creating training data is an iterative process. As your model evolves through a project lifecycle, your training data will likely also evolve.

**Word of caution**: Data is full of potential biases. Use data but don't trust it too much !

## Sampling

Sampling happens in many stages of an ML project lifecycle:
1. Sampling from all possible real-world data to create training data
2. Sampling from a given dataset to create splits for training, validation and testing
3. Sampling from all possible events that happen within your ML system for monitoring purposes.

When is sampling necessary ?
1. You don't have access to all possible data in real world.
2. Its infeasible to process all the data that you have access to -- limitation of time and resources.
3. Sampling allows us to accomplish a task faster and cheaper.

### Non-Probability Sampling
Selection of data isn't based on any probability criteria.

- *Convenience Sampling*: Samples of data are selected based on their availability. 
- *Snowball Sampling*: Future samples are selected based on existing samples.
- *Judgement Sampling*: Experts decided on what samples to include.
- *Quota Sampling*: You select samples based on quotas for certain slices of data without any randomization.

**Pro**: ==Non-probability sampling can be a quick and easy way to gather your initial data to get your project off the ground==.
**Con**: The samples selected by non-probability criteria are not representative of the real- world data and therefore are riddled with selection biases. Unfortunately, in many cases, the selection of data for ML models is still driven by convenience. 

E.g. LLMs, sentiment analysis.
Language models are often trained not with data that is representative of all possible texts but with data that can be easily collected—Wikipedia, Common Crawl, Reddit.

For sentiment analysis, data is collected from natural lables such as reviews and ratings. IMDB reviews and Amazon reviews are biased toward users who are willing to leave reviews online, and not necessarily representative of people who don’t have access to the internet or people who aren’t willing to put reviews online.

 ==**Non-probability sampling can be a quick and easy way to gather your initial data to get your project off the ground**==.

### Simple Random Sampling
**Pro:** Easy to implement
**Con**: Rare categories of data might not appear in your selection

### Stratified Sampling
Divide your population into groups you care about and then sample from each group separately.
**Pro**: Samples from all rare classes are included in the selection
**Con**:  It isn’t always possible, such as when it’s impossible to divide all samples into groups. This is especially challenging when one sample might belong to multiple groups, as in the case of multilabel tasks. For instance, a sample can be both class A and class B.

### Weighted Sampling
In weighted sampling, each sample is given a weight, which determines the probability of it being selected. 

This method allows you to leverage domain expertise. For example, if you know that a certain subpopulation of data, such as more recent data, is more valuable to your model and want it to have a higher chance of being selected, you can give it a higher weight.

This also helps with the case when the data you have comes from a different distribution compared to the true data.

### Reservoir Sampling
Especially useful for dealing with streaming data.

The algorithm involves a reservoir, which can be an array, and consists of three steps:
1. Put the first k elements into the reservoir.
2. For each incoming nth element, generate a random number i such that 1 ≤ i ≤ n.
3. If 1 ≤ i ≤ k: replace the ith element in the reservoir with the nth element. Else, do nothing.

This means that each incoming nth element has $\frac{k}{n}$ probability of being in the reser‐ voir. You can also prove that each element in the reservoir has $\frac{k}{n}$ probability of
being there.

![[Pasted image 20240203101604.png]]
### Importance Sampling
> [!DANGER] To be understood and done later

## Labeling

Data labeling has gone from being an auxiliary task to being a core function of many ML teams in production.

### Hand Labels
Acquiring hand labels is difficult for many reasons.
1. Hand-labeling data can be **expensive**, especially if subject matter expertise is required. 
2. Hand labeling poses a threat to **data privacy**.
3. Hand labeling is **slow**.
	-  Slow labeling leads to slow iteration speed and makes your model less adaptive to changing environments and requirements. If the task changes or data changes, you’ll have to wait for your data to be relabeled before updating your model
#### Label multiplicity
Often, to obtain enough labeled data, companies have to use data from multiple sources and rely on multiple annotators who have different levels of expertise. 

These different data sources and annotators also have different levels of accuracy. This leads to the problem of **label ambiguity** or **label multiplicity**: what to do when there are multiple conflicting labels for a data instance.

If human experts can't agree on a label, what does human-level performance even mean ?

**Solution**: 
1. Have a clear problem definition.
2. Incorporate that definiton into the annotators' training.

#### Data Lineage
It’s good practice to keep track of the origin of each of your data samples as well as its labels, a technique known as **data lineage**. Data lineage helps you both flag potential biases in your data and debug your models.

### Natural Labels
Natural labels are tasks where the model’s prediction can be evaluated by the system without human labeling. E.g. Google Maps, Stock Price Prediction, Recommender systems.

#### Feedback Loop length
For tasks with natural ground truth labels, the time it takes from when a prediction is served until when the feedback on it is provided is the feedback loop length. 

Choosing the right window length requires thorough consideration, as it involves the speed and accuracy trade-off. 

A short window length means that you can capture labels faster, which allows you to use these labels to detect issues with your model and address those issues as soon as possible. However, short window length also means that you might prematurely label a recommendation as bad before its clicked on.

But, No matter how long you set your window length to be, there might still be premature negative labels. For tasks with long feedback loops, natural labels might not arrive for weeks or even months.

### Handling the Lack of Labels

![[Pasted image 20240216085429.png]]
#### Weak Supervision
- **Core Idea**:  insight behind weak supervision is that people rely on ==heuristics==, which can be developed with subject matter expertise, to label data, a.k.a **Programmatic Labelling**
- Libraries like Snorkel are built around the concept of a labeling function (LF): a function that encodes heuristics.
Example:
``` Python
def labeling_function(note):
	if "pneumonia" in note:
		return "EMERGENT"
```
- LFs can encode many different types of heuristics like:
	1. Keyword
	2. RegEx
	3. DB lookup
	4. O/P of other models
- After you’ve written LFs, you can apply them to the samples you want to label. 
- Because LFs encode heuristics, and heuristics are noisy, labels produced by LFs are noisy. Multiple LFs might apply to the same data examples, and they might give conflicting labels.
- It’s important to combine, denoise, and reweight all LFs to get a set of most likely to be correct labels.
- **Use Case**: Weak supervision can be especially useful when your data has strict privacy requirements. You only need to see a small, cleared subset of data to write LFs, which can be applied to the rest of your data without anyone looking at it. With LFs, subject matter expertise can be versioned, reused, and shared

![[Pasted image 20240216090207.png]]
>[!FAQ] If heuristics work so well to label data, why do we need ML models  ?
>1. LFs might not cover all data samples, so we can train ML models on data programatically labelled with LFs and use the trained model to generate predictions for samples that aren't covered by any LF.
>2. In some cases, the labels obtained by weak supervision might be too noisy to be useful. But even in these cases, weak supervision can be a good way to get you started when you want to explore the effectiveness of ML without wanting to invest too much in hand labeling up front.

#### Semi-supervision











 

















