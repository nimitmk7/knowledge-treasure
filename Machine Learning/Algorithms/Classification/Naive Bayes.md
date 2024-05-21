A Naive Bayes classifier is a probabilistic machine learning model that’s used for classification task. The crux of the classifier is based on the **Bayes theorem**. 

## Formulation

### Assumption
The predictors/features are independent. That is presence of one particular feature does not affect the other. Hence it is called **naive**.

### Bayes Theorem and application

$$ P(A|B) = \frac{P(B|A)P(A)}{P(B)} $$

A → Hypothesis
B → Evidence

- We can re-write it for classification task as follows:

$$ P(y|X) = \frac{P(X|y)P(y)}{P(X)} $$

y → class variable
X → features, X = $(x_1,x_2….x_n)$ 

Here $x_1,x_2….x_n$ represent the features. By substituting for **X** and expanding using the chain rule we get:

$$P(y|x_{1},\dots, x_{n}) = \frac{P(x_{1}|y) P(x_{2}|y) \dots P(x_{n}|y)P(y)}{P(x_{1})P(x_{2})\dots P(x_{n})}$$
For all entries in the dataset, the denominator does not change, it remains static. Therefore, the denominator can be removed and a proportionality can be introduced.

$$P(y|x_{1},\dots, x_{n}) \propto P(y)\prod_{i=1}^n P(x_{i}|y)$$
Converting the probabilities to a classification result, we get
$$y = argmax_{y}\space P(y)\prod_{i=1}^n P(x_{i}|y)$$
## Types
1. Multinomial Naive Bayes
2. Bernoulli Naive Bayes
3. Gaussian Naive Bayes
## Pros
1. This classifier performs better than other models with less training data if the assumption of independence of features holds.
2. **Scales well**: Under-the-hood, Naive Bayes involves a multiplication (once the probability is known) which makes the algorithm simplistic and fast.
3.  It can also be used to solve multi-class prediction problems.
4. **Can handle high-dimensional data**

## Cons
1. **Unrealistic Core Assumption**: It assumes that all the features are independent. This is actually a big con because features in reality are frequently not fully independent.
2. **Subject to zero frequency**: Zero frequency occurs when a categorical variable does not exist within the training set. For example, imagine that we’re trying to find the maximum likelihood estimator for the word, “sir” given class “spam”, but the word, “sir” doesn’t exist in the training data. The probability in this case would zero, and since this classifier multiplies all the conditional probabilities together, this also means that posterior probability will be zero. To avoid this issue, laplace smoothing can be leveraged.

## Use Case
- When the assumption of independence holds between features, Naive Bayes classifier typically performs better than logistic regression and requires less training data.
- It performs well in case of categorical input variables compared to continuous/numerical variable(s).


## References
https://vinija.ai/concepts/ml-comp/#naive-bayes-classifier
https://towardsdatascience.com/naive-bayes-classifier-81d512f50a7c
https://www.ibm.com/topics/naive-bayes
