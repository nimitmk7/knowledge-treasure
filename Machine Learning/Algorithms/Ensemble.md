![[Pasted image 20240331150849.png]]

- A very simple way to create an even better classifier is to aggregate the predictions of each classifier, and predict the class that gets the most votes.

- **This majority-vote classifier is called a hard voting classifier.**

- Somewhat surprisingly, this voting classifier often achieves higher accuracy than the best classifier in the ensemble. 

- In fact, even if each classifier is a weak learner (meaning it does only slightly better than random guessing), the ensemble can still be a strong learner (achieving high accuracy), provided there are a sufficient number of weak learners and they are sufficiently diverse.

## How ?
