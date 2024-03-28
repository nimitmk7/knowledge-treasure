## Pros
1. This classifier performs better than other models with less training data if the assumption of independence of features holds.
2.  Under-the-hood, Naive Bayes involves a multiplication (once the probability is known) which makes the algorithm simplistic and fast.
3. It can also be used to solve multi-class prediction problems

## Cons
1. It assumes that all the features are independent. This is actually a big con because features in reality are frequently not fully independent.

## Use Case
- When the assumption of independence holds between features, Naive Bayes classifier typically performs better than logistic regression and requires less training data.
- It performs well in case of categorical input variables compared to continuous/numerical variable(s).


## References
https://vinija.ai/concepts/ml-comp/#naive-bayes-classifier
