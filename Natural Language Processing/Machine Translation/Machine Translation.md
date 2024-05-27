## Definition
It is the task of translating a sentence **_x_** from one language(**source**) to a sentence **_y_** in another language(**target**).
### Example
***x***: L'homme est né libre, et partout il est dans les fers (**French**)
***y***: Man is born free, but everywhere he is in chains (**English**)

## 1950s: Early Machine Translation

Mostly rule-based systems, using a bilingual dictionary to map words from source language to target language. (Used for Russian → English)

## 1990s-2010s: Statistical Machine Translation

**Core idea** : Learn a probabilistic model from data 
- Suppose we’re translating French → English. 
- We want to find best English sentence ***y***, given French sentence ***x***.
$$ argmax_{y} P(y\space|\space x) $$
- Use Bayes Rule to break this down into two components to be learnt separately. ![[Pasted image 20240421170225.png]]

**Question**: How to learn translation model $P(x\space|\space y)$ ?
- First, we need large amount of **parallel data**. (e.g. pairs of human-translated French/English sentences) ![[Pasted image 20240421170512.png | 500]]
### Learning Alignment for SMT
**Question**: How to learn translation model from the parallel corpus?

- Break it down further: We actually want to consider $$P(x,a|y) $$ where $a$ is the alignment, i.e. word-level correspondence between French sentence _x_ and English Sentence _y_.

#### Alignment
-  It is the **correspondence between particular words** in the translated sentence pair.
- **Example**: 
	![[Pasted image 20240421183623.png | 400]]
	
	Note: Some words have no counterpart
- Alignment can be **many-to-one**.
	- ![[Pasted image 20240421183813.png | 400]]
- Alignment can be **one-to-many**.
	![[Pasted image 20240421190139.png | 400]]
- Alignment can be **many-to-many**.
	![[Pasted image 20240421190510.png | 400]]

- We learn $P(x,a|y)$ as a combination of many factors, including:
	- Probability of particular words aligning(also depends on position in sent)
	- Probability of particular words having particular fertility (number of corresponding words)
	- etc.

#### Decoding for SMT

![[Pasted image 20240421192048.png | 500]]
- We could enumerate every possible _y_ and calculate the probability → too expensive !!
- **Answer**: Use a heuristic search algorithm to search for the best translation, discarding hypotheses that are too low-probability
- This process is called **decoding**.

##### Example

![[Pasted image 20240421192308.png | 400]]

![[Pasted image 20240421192347.png | 600]]

### Issues
- The best systems were extremely complex
	- Systems had many separately-designed subcomponents
	- Lots of feature engineering 
		- Need to design features to capture particular language phenomena 
	- Require compiling and maintaining extra resources
		- Like tables of equivalent phrases 
	- Lots of human effort to maintain
		- Repeated effort for each language pair!

## 2014-present: [[Neural Machine Translation(NMT)]]

## Unsolved Challenges
1. Out-of-vocabulary words
2. Domain mismatch between train and test data
3. Maintaining context over longer text
4. Low-resource language pairs
5. Using common sense is still hard.
6. Removing biases in model inherited from training data.
7. Uninterpretable systems do strange things.



