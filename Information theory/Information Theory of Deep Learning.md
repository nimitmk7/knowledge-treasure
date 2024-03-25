Input Layer: 1 high-d high entropy random variable X

Output Layer: Very simple

Training Data: Sample of joint dist of X, Y

Layers: Markov Chain of successive representations

>[!INFO] NOTE
> A Markov chain or Markov process is **a stochastic model describing a sequence of possible events in which the probability of each event depends only on the state attained in the previous event**


Output: Another random variable which is supposed to be as close  as possible to Y : $\hat Y$

Neural Network : Bottleneck problem 

I want to squeeze out of X everything I can except the thing that is relevant to Y. That is Deep Learning


**Minimal Sufficient Statistic**: S(X) is the minimal sufficient statistic of X, if the M.I. of S(X) and Y is almost the same as X and Y.

Each layer is characterized by its encoder and decoder information.
![[Pasted image 20240319162935.png]]

For a large typical X, the sample complexity of a DNN is completely determined by the encoder mutual information, I(X;T), of the last hidden layer. The accuracy is determined by the decoder information, I(T;Y) of the last hidden layer.


