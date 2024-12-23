As LLMs become more capable, we need to evaluate their capabilities more broadly and comprehensively. 

But what are some areas we would want to evaluate ?
- Summarization
- Translation
- General Knowledge
- Creativity
- Coding Performance
- Planning
- Mathematical Reasoning

## Summarization
- Key Qualities
	- **Understanding**: Does the model understand the text it needs to summarize
	- **Generation**: Can the model generate a good summary of that text
- But how do we measure summarization quality ?

### ROUGE
- Compute N-gram overlap with a gold reference summary/sequence
- Either with a variety of fixed N,  or using the longest common subsequence between source and reference

$$\text{ROUGE-N} = \frac{\sum_{S \in \text{ReferenceSummaries}}\sum_{gram_{n} \in S} Count_{match}(gram_{n})}{\sum_{S \in \text{ReferenceSummaries}}\sum_{gram_{n} \in S} Count(gram_{n})}$$
### BLEU
- Modified N-gram precision metrics
- **Clipping:** Truncate each word’s count to not exceed the largest count observed in any single reference for that word.

$$p_{n} = \frac{\sum_{C \in {Candidates}}\sum_{n-gram \in C} Count_{clip}(n-gram)}{\sum_{C' \in \text{Candidates}}\sum_{n-gram' \in C'} Count(gram_{n})}$$

### BERTScore

![[Pasted image 20241208181306.png]]

 BERTScore computes a similarity score for each token in the candidate sentence with each token in the reference sentence. However, instead of exact matches, we compute token similarity using contextual embeddings.

1. **Contextual Embeddings**: Reference and candidate sentences are represented using contextual embeddings based on surrounding words, computed by models like BERT, Roberta, XLNET, and XLM.
2. **Cosine Similarity**: The similarity between contextual embeddings of reference and candidate sentences is measured using cosine similarity.
3. **Token Matching for Precision and Recall**: Each token in the candidate sentence is matched to the most similar token in the reference sentence, and vice versa, to compute Recall and Precision, which are then combined to calculate the F1 score.
4. **Importance Weighting**: Rare words’ importance is considered using Inverse Document Frequency (IDF), which can be incorporated into BERTScore equations, though it’s optional and domain-dependent.
5. **Baseline Rescaling**: BERTScore values are linearly rescaled to improve human readability, ensuring they fall within a more intuitive range based on Common Crawl monolingual datasets.

## [GLUE](https://gluebenchmark.com/)
![[Pasted image 20241208182339.png]]
The **General Language Understanding Evaluation** (GLUE) benchmark is a collection of resources for training, evaluating, and analyzing natural language understanding systems.

GLUE consists of:
- A benchmark of nine sentence- or sentence-pair language understanding tasks built on established existing datasets and selected to cover a diverse range of dataset sizes, text genres, and degrees of difficulty,
- A diagnostic dataset designed to evaluate and analyze model performance with respect to a wide range of linguistic phenomena found in natural language, and
- A public leaderboard for tracking performance on the benchmark and a dashboard for visualizing the performance of models on the diagnostic set.
### Drawbacks
GLUE performance saturates, so we need a harder benchmark. 
![[Pasted image 20241208183315.png | 700]]
So, we need a harder benchmark.
## [SuperGLUE](https://super.gluebenchmark.com/ )
![[Pasted image 20241208221149.png]]
SuperGLUE also began to saturate, so we need an even harder benchmark.
## MMLU
Covers 57 tasks. 
![[Pasted image 20241208221652.png]]

## Coding

### SWE-Bench

![[Pasted image 20241208221854.png]]

![[Pasted image 20241208221917.png]]
#### Pass@K
- Since we can sample multiple completions for a given prompt, an LLM can have multiple attempts to solve a given coding problem.
- **Metric definition:** Given a problem and K completions/solutions sampled, we consider it a **pass** if at least one correctly solves the problem.
## Mathematics

### GSM8K
- Grade School Math word problems
![[Pasted image 20241208222640.png]]
### AIMO/AIME
- Complex Math problems with difficult solutions, taken from places like Math Olympiad.
![[Pasted image 20241208222747.png]]

- But even complex math benchmarks are increasingly becoming solvable.![[Pasted image 20241208222843.png]]
- So how do we find more difficult problems ????
## Contaminated and Outdated Benchmarks
- Frontier LLMs are trained on more and more data both synthetically generated and curated from the internet.
- Over time, the training data can become contaminated with examples from the test-set of existing benchmarks, unintentionally or otherwise.
- Data can be synthetically generated to capture the test set while being distinct.
- Particularly true for closed-source models

## Daily Oracle, A Continually Generated Dataset
![[Pasted image 20241208230242.png]]
- A benchmark continually generated from news each day, thus it never goes out of date.
- Focused on forecasting events after the knowledge cutoff date of existing LLMs → **tests temporal reasoning and generalization**




## References
1. https://medium.com/@abonia/bertscore-explained-in-5-minutes-0b98553bfb71
2. 


