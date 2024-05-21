## Step 1: Clarify the Problem and Constraints
If you frame the business and product problem correctly, you’ve done half the work. 

Potential business-side requirement questions: 
- What is the dependent problem we are trying to model ? 
- How has the problem been approached in the past ? Is there a baseline performance and how much do we need to beat this baseline for success ?
- Is ML even needed ? 
- Is it ethical or legal to apply ML ?
- How do end users benefit from and use this solution ?
- Is there a clear value add to the business from this solution ?
- What is business impact of incorrect predictions ?
- Does ML need to solve the entire problem end-to-end or smaller decoupled systems be made to solve sub-problems, whose output is then combined?

Potential tech-side requirement questions:
- What’s the latency needed ? Does every part of the system need to be real-time–while inference may need to be fast, can training be slow?
- Are there any throughput requirements ?(Like Predictions per minute)
- Where is this model being deployed ? (Cloud/Edge Device) and what is the cost associated with deployment ?
## Step 2: Establish Metrics
Pick simple, observable, and attributable metrics that encapsulate solving the problem. In most cases, align business KPI with your model performance metrics. 

Best to opt for a single metric rather than picking multiple metrics to capture different sub-goals. Because a single metric makes it easier to rank model performance. Also easier to align the team around optimizing a single number. 

**Interview**: Start with a single metric, but hedge your answer by mentioning other potential metrics to track. 

After picking a metric, you need to establish what success looks like. 
## Step 3: Understand Your Data Sources

- How fresh is the data ? How often does it get updated ?
- Is there a data dictionary available? Have you talked to subject matter experts on it?
- How was the data collected ? Presence of sampling, selection, response bias?
## Step 4: Explore Your Data
- Profile the columns at first glance
	- Which ones might be useful ?
	- Which ones have practically no variance and thus wouldn’t offer up any real predictive value ?
	- Which ones are noisy?
	- Which ones have lot of missing or odd values ?
- Visualize the distributions of columns of interest, or through historgram with binning.
- Create a correlation matrix to visually inspect the basic relationships between variables.
## Step 5: Clean Your Data
- Drop irrelevant data or erroneously duplicated rows and columns. Handle incorrect values that don’t match up with supposed data schema. 
- For missing data:
	- Substitute with mean/median
	- Substitute using a model or distribution
	- Drop rows with missing values
- Deal with outliers:
	- Remove them outright
	- Truncate the values
	- Leave them as is
	- Check for multi-variate outliers as well
## Step 6: Feature Engineering
The art of presenting data to ML models in the best way possible. 
1. Feature selection
2. Feature Preprocessing
## Step 7: Model Selection
Factors to consider while selecting a model include:
1. Training and Prediction Speed
2. Budget
3. Volume and Dimensionality of Data
4. Categorical vs Numerical Features
5. Explainability
![[ML cheatsheet.png | 700]]
## Step 8: Model Training & Evaluation
- Train-test-validation split
- Cross Validation scores
- Hyper-parameter tuning
- Regularization
- Using learning curves to find performance plateaus
- Imbalanced Dataset: Random Over/under sampling
## Step 9: Deployment
- Online prediction vs batch prediction
- Handling model degradation due to DDS or other reasons
	- How often or when to Retrain the model
	- What events will trigger a model refresh
	- How much new data vs historical data to use
## Step 10: Iterate
- Error analysis: Analyzing wrong predictions manually, and bucketing them into the types of reasons they occurred, to prioritize next projects.
