ID3 stands for Iterative Dichotomiser 3 and is named such because the algorithm **iteratively** (repeatedly) **dichotomizes**(divides) features into two or more groups at each step.

Invented by [Ross Quinlan](https://en.wikipedia.org/wiki/Ross_Quinlan), ID3 uses a **top-down greedy** approach to build a decision tree **recursively**.

- **Top-Down**: We start building the tree from the top
- **Greedy**: At each iteration, We select the beast feature at the present moment to create a node.

## Metrics

ID3 uses **Information Gain** to find the best feature. 

Information Gain calculates the reduction in the entropy and measures how well a given feature separates or classifies the target classes. ==The feature with the **highest Information Gain** is selected as the **best** one==.

$$\text{Information Gain} = Entropy(before) - Entropy(after)$$
## Steps
1. Calculate Information Gain for each feature.
2. Considering that all rows don’t belong to the same class, split the dataset **S** into subsets using the feature for which the Information Gain is maximum.
3. Make a decision tree node using the feature with the maximum Information gain.
4. If all rows belong to the same class, make the current node as a leaf node with the class as its label.
5. Repeat for the remaining features until we run out of all features, or the decision tree has all leaf nodes.

## Hands-on example

### Data
We’ll be using a sample dataset of COVID-19 infection. A preview of the entire dataset is shown below.

![[Pasted image 20240525191241.png | 400]]

The columns are self-explanatory. Y and N stand for Yes and No respectively. The values or **classes** in Infected column Y and N represent Infected and Not Infected respectively.

The columns used to make decision nodes viz. ‘Breathing Issues’, ‘Cough’ and ‘Fever’ are the features and the column used for leaf nodes i.e. ‘Infected’ is called the target column.

### Steps
From the total of 14 rows in our dataset **S**, there are **8** rows with the target value **YES** and **6** rows with the target value **NO**. The entropy of **S** is calculated as:
$$\text{Entropy} = -\frac{8}{14} \log_{2}\left( \frac{8}{14} \right) - \frac{6}{14} \log_{2}\left(\frac{6}{14}\right) = 0.99$$
**IG Calculation for Fever Feature**

In this(Fever) feature there are **8** rows having value **YES** and **6** rows having value **NO**. 
As shown below, in the **8** rows with **YES for** Fever, there are **6** rows having target value **YES** and **2** rows having target value **NO.**

![[Pasted image 20240525191941.png | 400]]

As shown below, in the **6** rows with **NO**, there are **2** rows having target value **YES** and **4** rows having target value **NO.**

![[Pasted image 20240525192009.png | 400]]

Therefore, the calculation of Information Gain for **Fever** is:
|S| = 14

For v = YES, $|S_v|$ = 8.
Entropy($S_v$) = $-\frac{6}{8} \log_{2} \frac{6}{8} - \frac{2}{8} \log_{2} \frac{2}{8} = 0.81$

For v = NO, $|S_v|$ = 6.
Entropy($S_v$) = $-\frac{2}{6} \log_{2} \frac{2}{6} - \frac{4}{6} \log_{2} \frac{4}{6} = 0.81$

$IG = Entropy(S) - (|Sʏᴇꜱ| / |S|) * Entropy(Sʏᴇꜱ) - (|Sɴᴏ| / |S|) * Entropy(Sɴᴏ)$

$$IG(S, Fever) = Entropy(S) - \frac{8}{14} *0.81 - \frac{6}{14}*0.91  = 0.13$$

Next, we calculate the **IG** for the features **“Cough”** and **“Breathing issues”.**

**IG(S, Cough) = 0.04  
IG(S, BreathingIssues) = 0.40**

Since the feature **Breathing issues** have the highest Information Gain it is used to create the root node.

![[Pasted image 20240525194427.png]]

Next, from the remaining two unused features, namely, **Fever** and **Cough**, we decide which one is the best for the left branch of **Breathing Issues**.  

Since the left branch of **Breathing Issues** denotes **YES,** we will work with the subset of the original data i.e the set of rows having **YES** as the value in the Breathing Issues column. 

These **8 rows** are shown below:

![[Pasted image 20240525194508.png | 400]]

Next, we calculate the IG for the features Fever and Cough using the subset **S**ʙʏ (**S**et **Breathing** Issues **Y**es) which is shown above :

$IG(S_{BY}, Fever) = 0.20$
$IG(S_{BY}, Cough) = 0.09$

IG of Fever is greater than that of Cough, so we select **Fever** as the left branch of Breathing Issues:

![[Pasted image 20240525194632.png]]

Next, we find the feature with the maximum IG for the right branch of **Breathing Issues**. But, since there is only one unused feature left we have no other choice but to make it the right branch of the root node.  

So our tree now looks like this:

![[Pasted image 20240525194654.png]]

For the left leaf node of Fever, we see the subset of rows from the original data set that has **Breathing Issues** and **Fever** both values as **YES**.

![[Pasted image 20240525194724.png | 400]]

Since all the values in the target column are **YES,** we label the left leaf node as **YES**, but to make it more logical we label it **Infected.**

Similarly, for the right node of Fever we see the subset of rows from the original data set that have **Breathing Issues** value as **YES** and **Fever** as **NO**.

![[Pasted image 20240525194816.png | 400]]

Here not all but **most** of the **values** are **NO,** hence **NO** or **Not Infected** becomes our **right leaf node.**

![[Pasted image 20240525194841.png]]

We repeat the same process for the node **Cough**, however here both left and right leaves turn out to be the same i.e. **NO** or **Not Infected** as shown below:

![[Pasted image 20240525194859.png]]

Looks Strange, doesn’t it?  

I know! The right node of Breathing issues is as good as just a leaf node with class ‘Not infected’. This is one of the Drawbacks of ID3, it doesn’t do pruning.

## Drawbacks
1. No pruning built in the algorithm
2. Overfitting or high variance i.e. it learns the dataset it used so well that it fails to generalize on new data.






## References
1. [Decision Trees: ID3 Algorithm Explained](https://towardsdatascience.com/decision-trees-for-classification-id3-algorithm-explained-89df76e72df1)
