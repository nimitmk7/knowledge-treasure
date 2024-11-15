We introduce a new language representation model called BERT, which stands for **B**i-directional **E**ncoder **R**epresentations from **T**ransformers.

BERT is designed to pre-train deep bidirectional representations from unlabeled text by ==jointly conditioning on both left and right context in all layers==. 

As a result, ==the pre-trained BERT model can be fine- tuned with just one additional output layer to create state-of-the-art models for a wide range of tasks==, such as question answering and language inference, without substantial task- specific architecture modifications.

## Introduction

### Premise
There are two existing strategies for applying pre-trained language representations to downstream tasks: 
1. **Feature-based approach**
	1. Uses task-specific architectures that include the pre-trained representations as additional features.
	2. Example: ELMO
2. **Fine-tuning**
	1. Minimal task-specific parameters, trained on all downstream tasks by simply fine-tuning all pre-trained parameters. 
	2. Example: GPT

Current techniques restrict the power of the pre-trained representations, especially for the fine-tuning approaches. 

**Major limitation:** Standard language models are unidirectional, and this limits the choice of architectures that can be used during pre-training.

In OpenAI GPT, the authors use a left-to- right architecture, where every token can only attend to previous tokens in the self-attention layers of the [[Transformers]]. 

Such restrictions are sub-optimal for sentence-level tasks, and could be very harmful when applying fine-tuning based approaches to token-level tasks such as question answering, where it is crucial to incorporate context from both directions.

### Solution
BERT alleviates the previously mentioned unidirectionality constraint by using a “masked language model” (MLM) pre-training objective.

The masked language model randomly masks some of the tokens from the input, and the objective is to predict the original vocabulary id of the masked word based only on its context. 

Unlike left-to- right language model pre-training, the MLM objective enables the representation to fuse the left and the right context, which allows us to pre- train a deep bidirectional Transformer.
\
BERT results show that pre-trained representations reduce the need for many heavily-engineered task- specific architectures.

## The Model

There are two steps in our framework: 
1. Pre-training
	1. The model is trained on unlabeled data over different pre-training tasks
2. Fine-tuning. 
	1. The BERT model is first initialized with the pre-trained parameters, and all of the parameters are fine-tuned using labeled data from the downstream tasks. 
	2. ==Each downstream task has separate fine-tuned models==, even though they are initialized with the same pre-trained parameters

**Distinctive Feature:** Unified architecture across different tasks. Minimal difference between pre-trained and the final downstream architecture.

### The Architecture
#### Notation and Model Sizes
$L$: Number of Layers(Transformer Blocks)
$H$: Hidden Size
$A$: Number of self-attention heads

1. $\text{BERT}_\text{{BASE}}$ (L=12, H=768, A=12, Total Parameters=110M).
	1. Same model size as OpenAI GPT
2. $\text{BERT}_\text{{LARGE}}$ (L=24, H=1024, A=16, Total Parameters=340M).

#### Input-output representations
To make BERT handle a variety of down-stream tasks, our input representation is able to unambiguously represent both a single sentence and a pair of sentences (e.g., ⟨ Question, Answer ⟩) in one token sequence. 

##### CLS token
The first token of every sequence is always a special classification token $([CLS])$. The final hidden state corresponding to this token is used as the aggregate sequence representation for classification tasks. 
##### Separating sentences in sentence pairs
Sentence pairs are packed together into a single sequence. We differentiate the sentences in two ways. 

1. We separate them with a special token $([SEP])$.
2. We add a learned embedding to every token indicating whether it belongs to sentence $A$ or sentence $B$.

##### Embedding notations
We denote input embedding as $E$, the final hidden vector of the special $[CLS]$ token as $C ∈ R^H$, and the final hidden vector for the $i^{th}$ input token as $T_i \in R^H$.


![[Pasted image 20241024154341.png]]
#### Pre-training
![[Pasted image 20241024154613.png]]
##### Task 1: Masked Language Modeling
- Intuitively, it is reasonable to believe that a deep bidirectional model is strictly more powerful than either a left-to-right model or the shallow concatenation of a left-to- right and a right-to-left model. 

- In order to train a deep bidirectional representation, we ==simply mask some percentage(15% in this case) of the input tokens at random==, and then ==predict those masked tokens==.

- The final hidden vectors corresponding to the mask tokens are fed into an output softmax over the vocabulary, as in a standard LM.

- Although this allows us to obtain a bidirec- tional pre-trained model, a downside is that we are creating a mismatch between pre-training and fine-tuning, since the $[MASK]$ token does not appear during fine-tuning. 
	- To mitigate this, we do not always replace “masked” words with the ac- tual $[MASK]$ token.
	- The training data generator chooses 15% of the token positions at random for prediction. If the i-th token is chosen, we replace the i-th token with 
	1. The $[MASK]$ token 80% of the time,
	2. A random token 10% of the time 
	3. The unchanged $i^{th}$ token 10% of the time. 
	
	Then, $T_i$ will be used to predict the original token with cross entropy loss.

##### Task 2: Next Sentence Prediction

Many important downstream tasks such as Question Answering (QA) and Natural Language Inference (NLI) are based on ==understanding the relationship between two sentences==. 

This is not directly captured by language modeling. In order to train a model that understands sentence relationships, we pre-train for a binarized next sentence prediction task that can be ==trivially generated from any monolingual corpus==.

Specifically, when choosing the sentences A and B for each pre-training example, 

1. 50% of the time B is the actual next sentence that follows A (labeled as *IsNext*)
2. 50% of the time it is a random sentence from the corpus (labeled as *NotNext*)

$C$ is used for next sentence prediction (NSP). 

##### Formulation
$$max \sum_{x \sim D, x_{n} \sim p_{next}} \log p(y\space|\space x, x_{n}; \theta) $$

- $x_{n}:$ either the sentence following $x$ or a randomly sampled sentence.
- $y:$  binary label of whether $x_n$ follows $x$

> **Note**: Later work has shown that NSP objective is not necessary.

#### Fine-tuning

Fine-tuning is straightforward since the self-attention mechanism in the Transformer allows BERT to model many downstream tasks— whether they involve single text or text pairs—by swapping out the appropriate inputs and outputs.

For each task, we simply plug in the task- specific inputs and outputs into BERT and fine- tune all the parameters end-to-end. 

At the input, sentence A and sentence B from pre-training are analogous to 
1. Sentence pairs in paraphrasing,
2. Hypothesis-premise pairs in entailment
3. Question-passage pairs in question answering, and
4. A degenerate text-∅ pair in text classification or sequence tagging

At the output, the token representations are fed into an output layer for token- level tasks, such as sequence tagging or question answering, 

The $[CLS]$ representation is fed into an output layer for classification, such as entailment or sentiment analysis.

![[Pasted image 20241024163951.png]]

##### Classification tasks
Add a linear layer(randomly initialized) on top of the $[CLS]$ embedding $C$. 

![[Pasted image 20241024214910.png]]
$$p(y | x) = \text{softmax}(WC + b)$$
#### Sequence Labelling tasks
![[Pasted image 20241024215209.png]]

Add linear layers (randomly initialized) on top of every token.
$$p(y_i| x) = softmax(Wh_i + b)$$

