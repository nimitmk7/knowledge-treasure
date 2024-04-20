## Overview

![[Pasted image 20240419111911.png | 500]]

We will refer to this piece from Wikipedia:

> London is the capital and most populous city of England and the United Kingdom. Standing on the River Thames in the south east of the island of Great Britain, London has been a major settlement for two millennia. It was founded by the Romans, who named it Londinium.
> 
> (Source: [Wikipedia article “London”](https://en.wikipedia.org/wiki/London))

## 1. Segmentation

It is the ==task of splitting text into meaningful segments==. These segments can be composed of words, sentences, or topics.

### Example
If, we break the text apart into separate sentences. That gives us:

1. “London is the capital and most populous city of England and the United Kingdom.”
2. “Standing on the River Thames in the south east of the island of Great Britain, London has been a major settlement for two millennia.”
3. “It was founded by the Romans, who named it Londinium.”

We can assume that each sentence in English is a separate thought or idea. It will be a lot easier to write a program to understand a single sentence than to understand a whole paragraph.

> [!INFO] Coding a Sentence Segmentation model can be as simple as splitting apart sentences whenever you see a punctuation mark. But modern NLP pipelines often use more complex techniques that work even when a document isn’t formatted cleanly.
### Types
1. Word segmentation: Divides a sentence or continuous string of text into individual words
2. Sentence segmentation: Divides a paragraph or document into individual sentences
3.  Phrase segmentation: Identifies and extracts meaningful phrases or multi-word expressions from a text
4.  Chunking: Identifies and groups together syntactically related words in a sentence
5. Subword segmentation: Divides words into smaller units

> [!faq] What is the difference between tokenization and segmentation ?
> 
> **All tokenization is segmentation, but not all segmentation is tokenization**. While segmentation is a more generic concept of splitting the input text, tokenization is a type of segmentation and it is carried out based on a well defined criteria.

## 2. Tokenization
Tokenization is ==a process which breaks down a sequence of text into smaller units called tokens==. These tokens can be characters, words, phrases, sentences, or even dates and punctuation marks

Tokenization is important because it helps machines understand human language by **breaking it down into smaller meaningful pieces** that are **easier to analyze**. Tokenization also reduces the size of raw text so that it can be handled more easily for processing and analysis.

### Example
Let’s start with the first sentence from our document:

> “London is the capital and most populous city of England and the United Kingdom.”

Let us break this sentence into separate words or _tokens_. This is the result:

> “London”, “is”, “ the”, “capital”, “and”, “most”, “populous”, “city”, “of”, “England”, “and”, “the”, “United”, “Kingdom”, “.”

>[!INFO] Tokenization is easy to do in English. We’ll just split apart words whenever there’s a space between them. And we’ll also treat punctuation marks as separate tokens since punctuation also has meaning.

### Types
1. Word Tokenization
	1. Intuitive
	2. Suffer from unknown words problem(OOV: Out of Vocabulary)
	3. Large Vocabulary sizes.
	4. Issues with abbreviations(like U.S.A and can’t vs cannot)
2. [[Character Tokenization]]
3. [[Subword Tokenization]]

## 3. PoS(Part of Speech) Tagging
It is the process of marking up a word in a text (corpus) as corresponding to a particular part of speech, based on both its definition and its context. Parts of speech include noun, verb, adjective, adverb, etc.

PoS tagging can help algorithms understand the grammatical structure and meaning of a text, which can help improve the accuracy of NLP tasks such as named entity recognition and text classification.

We can do this by feeding each word (and some extra words around it for context) into a pre-trained part-of-speech classification model.

### Example

![[Pasted image 20240419114731.png]]

![[Pasted image 20240419114752.png]]
## 4. Lemmatization
It is ==a text pre-processing technique that breaks down words into their base form, or lemma, while preserving their meaning==. Also the first stage of **Vocabulary normalization**.

For example, the verb "to walk" may appear as "walk", "walked", "walks", or "walking". The base form, "walk", is the lemma for the word. Lemmatization groups these words as its lemma, "walk".

Lemmatization is typically done by having a l**ook-up table of the lemma forms of words based on their part of speech** and possibly **having some custom rules to handle words that you’ve never seen before**.

### Example
Here’s what our sentence looks like after lemmatization adds in the root form of our verb:

![[Pasted image 20240419131210.png]]

The only change we made was turning “is” into “be”.

## 5. Identifying Stop Words
Stop words are ==words that are commonly used in a language but are considered irrelevant for analysis==. When doing statistics on text, these words introduce a lot of noise since they appear way more frequently than other words. This is the second part of **Vocabulary Normalization**.

The specific stop words used can vary depending on the context. For example, in English, stop words include:

- Articles: a, an, the
- Conjunctions: and, but, or
- Prepositions: in, on, at, with
- Pronouns: he, she, it, they
- Common verbs: is, am, are, was, were, be, being, been 
- Some others are: of, this, we, and not

Stop words are usually identified by just by checking a hardcoded list of known stop words. But there’s no standard list of stop words that is appropriate for all applications. The list of words to ignore can vary depending on your application.

For example if you are building a rock band search engine, you want to make sure you don’t ignore the word “The”. Because not only does the word “The” appear in a lot of band names, there’s a famous 1980’s rock band called _The The_!

### Example
![[Pasted image 20240419131844.png]]

## 6. Dependency Parsing
It is the process to analyze the grammatical structure in a sentence and find out related words as well as the type of the relationship between them.

Each relationship:
1. Has one **head** and a **dependent** that modifies the **head**.
2. Is labeled according to the nature of the dependency between the **head** and the **dependent**

The goal is to build a tree that assigns a single **parent** word to each word in the sentence. The root of the tree will be the main verb in the sentence. But we can go one step further. In addition to identifying the parent word of each word, we can also predict the type of relationship that exists between those two words.

Just like how we predicted parts of speech earlier using a machine learning model, dependency parsing also works by feeding words into a machine learning model and outputting a result. 

### Example
![[Pasted image 20240419132232.png | 200]]

There exists a relationship between _car_ and _black_ because _black_ modifies the meaning of _car_. Here, _car_ acts as the **head** and _black_ is a **dependent** of the head. The nature of the relationship here is **amod** which stands for “Adjectival Modifier”. It is an adjective or an adjective phrase that modifies a noun.

![[Pasted image 20240419132528.png]]
This parse tree shows us that the subject of the sentence is the noun “_London_” and it has a “_be_” relationship with “_capital_”. We finally know something useful — _London_ is a _capital_! And if we followed the complete parse tree for the sentence (beyond what is shown), we would even found out that London is the capital of the _United Kingdom_.

## 7. Named Entity Recognition(NER)
Named entity recognition (NER)—also called entity chunking or entity extraction—is a component of natural language processing (NLP) that identifies predefined categories of objects in a body of text.

Entity Identification → Entity Classification → Contextual Analysis

But NER systems aren’t just doing a simple dictionary lookup. Instead, they are using the **context of how a word appears in the sentence** and a **statistical model to guess which type of noun a word represents**. A good NER system can tell the difference between “_Brooklyn Decker_” the person and the place “_Brooklyn_” using context clues.

### Types
1. ML based methods
2. Rules-based systems
3. Hybrid systems

Here are just some of the kinds of objects that a typical NER system can tag:
- People’s names
- Company names
- Geographic locations (Both physical and political)
- Product names
- Dates and times
- Amounts of money
- Names of events

### Example
![[Pasted image 20240419134911.png]]

## 8. Coreference Resolution

It is ==the task of finding and grouping all linguistic expressions(mentions) in a text that refer to the same real-world entity==. After finding and grouping these mentions we can resolve them by replacing, as stated above, pronouns with noun phrases.

Pronouns are essentially shortcuts that we use instead of writing out names over and over in each sentence. Humans can keep track of what these words represent based on context. But our NLP model doesn’t.

![[Pasted image 20240419135325.png]]

### Example

![[Pasted image 20240419135444.png]]

## References
1. [NLP is Fun! by Adam Geitgey](https://medium.com/@ageitgey/natural-language-processing-is-fun-9a0bff37854e)
2. [Difference between tokenization and segmentation - Stack Overflow](https://stackoverflow.com/questions/70048302/difference-between-tokenization-and-segmentation)
3. 