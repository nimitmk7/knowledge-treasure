## Premise
Word2vec is a two-layer neural net that processes text by “vectorizing” words. Its input is a text corpus and its output is a set of vectors: feature vectors that represent words in that corpus. 

While Word2vec is not a deep neural network, it turns text into a numerical form that deep neural networks can understand.

**Core Idea** : By training a model to predict the occurrence of words, we can find the optimal weights with which we can convert the words to a special embedding space.

![[Pasted image 20240419223143.png]]
### Benefit
1. Captures meaning of words
2. Low dimensional representation

## Algorithms

![[Pasted image 20240419223609.png]]

### CBOW(Continuous Bag of Words)
The CBOW model tries to predict the current target word (the center word) based on the source context words (surrounding words).

#### Architecture
##### Input Layer
- It is used to represent the context words as one-hot vectors.
- Input one-hot vectors: $x^{(c)}$


1. Hidden/Projection Layer: Learn the word embeddings
2. Output Layer: Predict the target word
#### Inference
1. We generate our one hot word vectors $(x^{(c−m)} , . . . , x^{(c−1)} , x^{(c+1)} , . . . , x^{(c+m)} )$ for the input context of size $m$. 
2. We get our embedded word vectors for the context $$(v_{c−m} = Vx^ {(c−m)} , v_{c−m+1} = Vx^{(c−m+1)} , . . ., v_{c+m} = Vx ^{(c+m)})$$
3. Average these vectors to get $$\hat v = \frac {v_{c−m}+v_{c−m+1}+...+v_{c+m}} {2m} $$
4. Generate a score vector $z = U\hat v$
5. Turn the scores into probabilities $\hat y = softmax(z)$
6. We desire our probabilities generated, $\hat{y}$, to match the true probabilities, $y$, which also happens to be the one hot vector of the actual word. 

![[CBOW_architecture.png]]
#### Training
How do we learn matrices $V$ and $U$ ?

Well, we need to create an objective function. Very often when we
are trying to learn a probability from some true probability, we look
to information theory to give us a measure of the distance between
two distributions. Here, we use a popular choice of distance/loss
measure, cross entropy H(yˆ, y).

The intuition for the use of cross-entropy in the discrete case can
be derived from the formulation of the loss function:
$$H(\hat{y}, y) = -\sum_{j=1}^{|V|} y_{j}\space log(\hat{y}_{j})$$
Let us concern ourselves with the case at hand, which is that y is
a one-hot vector. Thus we know that the above loss simplifies to
simply:

$$H(\hat{y}, y) = −y_{i} log(\hat{y}_i)$$
In this formulation, c is the index where the correct word’s one hot vector is 1. We can now consider the case where our prediction was perfect and thus $\hat{y}_c = 1$. We can then calculate $H(\hat{y}, y) = −1 \space log(1) = 0$. 

 We thus formulate our optimization objective as: 
 
 Minimize J
  = − $log \space P(w_c|w_{c−m}, . . . , w_{c−1}, w_{c+1}, . . . , w_{c+m})$
  = $− log\space P(u_{c}|\hat{v})$ 
  = − $log\space \frac {exp(u^T_{c} \hat{v})} {∑^{|V|}_{j=1} exp(u^T _{j} \hat{v})}$
  = $−u^T_c \hat{v} + log \sum^{|V|}_{j=1} exp(u^T _{j} \hat{v})$

We use gradient descent to update all relevant word vectors $u_c$ and $v_j$.

Once the training is done we use the weight matrix of hidden layer to get the Word2vec embedding. The weight matrix is multiplied by the one hot encoding to get the word2vec embedding.

>[!tip] The Fake Task
> Notice the we are developing a “fake” task for the neural network to learn, and are not even interested in the inputs and outputs (they are already known to us), instead we want to learn the weights of the hidden layer which would allow us to create embeddings.

#### Example
Sentence: “The cat jumped over the puddle”. We will treat the set of words: {"The", "cat", ’over", "the’, "puddle"} as a context and from these words, be able to predict or generate the center word "jumped".

### Skip-gram
The skip-gram tries to predict the surrounding/context words from a center word. 

![[Pasted image 20240420010304.png]]

#### Architecture
![[Pasted image 20240420011355.png |300]]

#### Inference
1. We generate our one hot word input vector $x$. 
2. We get our embedded word vectors for the word $v_{c} = Vx$
3. Set $\hat v = v_c$.
4. Generate $2m$ score vectors , $u_{c−m}, . . . , u_{c−1}, u_{c+1}, . . . , u_{c+m}$ using $u = Uv_c$
5. Turn the scores into probabilities $\hat y = softmax(u)$
6. We desire our probability vector generated to match the true probabilities, which is $y^{(c−m)} , . . . , y^{(c−1)} , y^{(c+1)} , . . . , y ^{(c+m)}$, the one hot vectors of the actual output.. 

#### Training
We need to generate an objective function for us to evaluate the model. A key difference here is that we invoke a Naive Bayes assumption to break out the probabilities, which is a strong (naive) conditional independence assumption. In other words, given the center word, all output words are completely independent.

minimize $J$ 
= $− log \space P(w_{c−m}, . . . , w_{c−1}, w_{c+1}, . . . , w_{c+m}|w_{c})$
= $− log\space \prod_{j=0,j\neq m}^{2m} P(w_{c−m+j} |w_{c})$ 
= $− log\space \prod_{j=0,j\neq m}^{2m} P(u_{c−m+j} |v_{c})$ 
= $− log\space \prod_{j=0,j\neq m}^{2m} \frac{\exp(u_{c-m+j}^Tv_{c})}{\sum_{k=1}^{|V|} \exp(u_{k}^Tv_{c})}$
= $− ∑^{2m}_{j=0, j \neq m}u_{c−m+j}^T v_{c} + 2m \space log \space∑_{k=1} ^{|V|} exp(u_{k}^T v_{c})$

With this objective function, we can compute the gradients with respect to the unknown parameters and at each iteration update them via Stochastic Gradient Descent.

Once the training is done we use the weight matrix of hidden layer to get the Word2vec embedding. The weight matrix is multiplied by the one hot encoding to get the word2vec embedding.

## Hands-On
Refer to: 

## References
1. https://wiki.pathmind.com/word2vec
2. https://cs224d.stanford.edu/lecture_notes/notes1.pdf
3. https://mlquant.notion.site/B-Vectorization-6a51d4e3cd834fafaa931ca58f16eeed
4. 
