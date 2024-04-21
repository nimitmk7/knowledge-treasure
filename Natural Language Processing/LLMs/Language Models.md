## Introduction
A **language model** is a probabilistic model of a natural language. Thus, they are ==machine learning models that predict the next word in a sentence based on previous words==. They are trained using large text datasets, such as books or articles.


## n-gram Language Models

$n-gram$ is a chunk of n consecutive words.

Language models in general compute the probability of occurrence of a number of words in a particular sequence.  The probability of a sequence of $m$ words $\{\mathbf w_1, ..., \mathbf w_m \}$ is denoted as $p(\mathbf w_1,...,\mathbf w_m)$. 

For example, the probability of the sentence "The cat sat on the mat" is $p(\text{The}, \text{cat}, \text{sat}, \text{on}, \text{the}, \text{mat})$. 

We can simply use the chain rule of probability to compute the probability of a sequence of words without making any assumptions about the structure of the language. For example, the probability of the sentence “The cat sat on the mat” is:

$$p(\text{The}, \text{cat}, \text{sat}, \text{on}, \text{the}, \text{mat}) = p(\text{The}) p(\text{cat} | \text{The}) p(\text{sat} | \text{The}, \text{cat}) p(\text{on} | \text{cat}, \text{sat}) p(\text{the} | \text{sat}, \text{on}) p(\text{mat} | \text{on}, \text{the})$$

In general, 

$$ p(\mathbf w_1,...,\mathbf w_m) = \prod_{i=1}^{i=m} p(\mathbf w_{i} | \mathbf w_1, ..., \mathbf w_{i-1})$$
But the counting of all these probabilities is a bit difficult, so we will use a simpler model that makes some assumptions about the structure of the language. 

> **Assumption**: The probability of a word depends only on a fixed number of previous words. For example, we can assume that the probability of a word depends only on the previous n-words. This assumption is called the **n-gram assumption**. 
$$ p(\mathbf w_1,...,\mathbf w_m)  \approx \prod_{i=1}^{i=m} p(\mathbf w_{i} | \mathbf w_{i-1}, ..., \mathbf w_{i-n+1})$$

To compute these probabilities, the ==count of each n-gram would be compared against the frequency of each word==. 

For instance, if the model takes bi-grams, the frequency of each bi-gram, calculated via combining a word with its previous word, would be divided by the frequency of the corresponding unigram. So, for bi-gram and trigram models the above equation becomes:

$$p(\mathbf w_2 | \mathbf w_1) = \dfrac {count(\mathbf w_1,\mathbf w_2)}{count(\mathbf w_1)}$$	
$$p(\mathbf w_3 | \mathbf w_1, \mathbf w_2) = \dfrac {count(\mathbf w_1, \mathbf w_2, \mathbf w_3)}{count(\mathbf w_1, \mathbf w_2)}$$
### Working Example
Suppose we are learning a 4-gram language model. 

Try to predict:
*as the students started the clock, the students started their ___

![[Pasted image 20240420191405.png]]
As n = 4, we only need context of last n-1 = 3 words.

$$P(\mathbf w \space| \space\text{students opened their}) = \frac{\text{count(students opened their} \space \mathbf w)}{\text{count(students opened their)}}$$
For example, suppose in the corpus, 
- “students opened their” occurred 1000 times
- “students opened their books” occurred 400 times
	- →$P(\text{books} \space| \space\text{students opened their}) = 0.4$
- 

### Issues
1. **Storage**: Counting these probabilities is notoriously space (memory) inefficient as we need to store the counts for all possible grams in the corpus. 
2. **Sparsity** **problem**: 
	1. **Denominator** : If $w_1$ and $w_2$ never occurred together in the corpus, then no probability can be calculated for $w_3$. To solve this, we could condition on $w_2$ alone. This is called **backoff**.
	2. **Numerator**: If $w_1$, $w_2$ and $w_3$ never appear together in the corpus, the probability of $w_3$ is 0. To solve this, a small $\delta$ could be added to the count for each word in the vocabulary. This is called **smoothing**.
	3. Also, Increasing n makes sparsity problems worse. Typically, n ≤ 5.

**Solution**: Neural Network based models

## Fixed Window neural Language Model

![[Pasted image 20240420223255.png]]

### Improvements over n-gram LM
1. No sparsity problem
2. Don’t need to store all observed n-grams

### Unsolved issues
1. Fixed window is too small
2. Enlarging window enlarges $W$
3. Window can never be large enough! 
4. $x^{(1)}$ and $x^{(2)}$ are multiplied by completely different weights in $W$. **No symmetry in how the inputs are processed**.
	1. What you learn in weight matrix in one section is not shared by others
	2. You are learning a lot of similar functions 4 times separately.
	![[Pasted image 20240420224311.png | 200]]

**Solution**: We need a neural architecture that can process any input length.
## [[RNN Language Models]]





