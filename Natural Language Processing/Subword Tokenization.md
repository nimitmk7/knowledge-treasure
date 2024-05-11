## Premise

Subword-based tokenization is a solution between word and character-based tokenization. The main idea is to solve the issues faced by word-based tokenization (very large vocabulary size, large number of OOV tokens, and different meaning of very similar words) and character-based tokenization (very long sequences and less meaningful individual tokens). 😎 

The subword-based tokenization algorithms do not split the frequently used words into smaller subwords. It rather splits the rare words into smaller meaningful subwords.

### Example
For example, “boy” is not split but “boys” is split into “boy” and “s”. This helps the model learn that the word “boys” is formed using the word “boy” with slightly different meanings but the same root word.

## Byte-Pair Encoding(BPE)
BPE is a simple form of data compression algorithm in which the most common pair of consecutive bytes of data is replaced with a byte that does not occur in that data.
### Example of Basic Variant

| Word        | Most frequent Pair | Rules                  |
| ----------- | ------------------ | ---------------------- |
| aaabdaaabac | aa                 | -                      |
| ZabdZabac   | ab                 | **Z = aa**             |
| ZYdZYac     | **ZY**             | Z = aa, **Y=ab**       |
| XdXac       | -                  | Z = aa, Y=ab, **X=ZY** |
It cannot be further compressed as there are no byte pairs appearing more than once. We decompress the data by performing replacements in reverse order.

### NLP Variant
BPE ensures that the most common words are represented in the vocabulary as a single token while the rare words are broken down into two or more subword tokens and this is in agreement with what a subword-based tokenization algorithm does.

#### NLP Variant Example
Corpus: **{“old”: 7, “older”: 3, “finest”: 9, “lowest”: 4}**

Let us add a special end token “</w>” at the end of each word.

**{“old</w>”: 7, “older</w>”: 3, “finest</w>”: 9, “lowest</w>”: 4}**

The “</w>” token at the end of each word is added to identify a word boundary so that the algorithm knows where each word ends. This helps the algorithm to look through each character and find the highest frequency character pairing.

Moving on next, we will split each word into characters and count their occurrence. The initial tokens will be all the characters and the “</w>” token. Since we have 23 words in total, so we have 23 “</w>” tokens. The second highest frequency token is “e”. In total, we have 12 different tokens

![[Pasted image 20240419150129.png | 500]]

##### Iterations

**Iteration 1**: The second most common token is ”e”, and the most common byte pair in our corpus with “e” is “e” and “s” (in the words finest and lowest) which occurred 9 + 4 = 13 times. We merge them to form a new token “es” and note down its frequency as 13. We will also reduce the count 13 from the individual tokens (“e” and “s”)

![[Pasted image 20240419150556.png|500]]

**Iteration 2:** We will now merge the tokens “es” and “t” as they have appeared 13 times in our corpus. So, we have a new token “est” with frequency 13 and we will reduce the frequency of “es” and “t” by 13.

![[Pasted image 20240419150637.png | 500]]

**Iteration 3:** Let’s work now with the “</w>” token. We see that byte pair “est” and “</w>” occurred 13 times in our corpus.

![[Pasted image 20240419150704.png|500]]

> Merging stop token </w> is very important. This helps the algorithm understand the difference between the words like “estimate” and “highest”. Both these words have “est” in common but one has an “est” token in the end and one at the start. Thus tokens like “est” and “est</w>” would be handled differently. If the algorithm will see the token “est</w>” it will know that it is the token for the word “highest” and not for the word “estate”.

**Iteration 4**: Looking at the other tokens, we see that byte pairs “o” and “l” occurred 7 + 3 = 10 times in our corpus.
![[Pasted image 20240419151028.png | 500]]

**Iteration 5:** We now see that byte pairs “ol” and “d” occurred 10 times in our corpus.
![[Pasted image 20240419151103.png | 500]]
If we now look at our table, we see that the frequency of “f”, “i”, and “n” is 9 but we have just one word with these characters, so we are not merging them. For the sake of the simplicity of this article, let us now stop our iterations and closely look at our tokens.

##### Encoding and Decoding

Let us now see how we will decode our example. To decode, we have to simply concatenate all the tokens together to get the whole word. For example, the encoded sequence [“the</w>”, “high”, “est</w>”, “range</w>”, “in</w>”, “Seattle</w>”], we will be decoded as [“the”, “highest”, “range”, “in”, “Seattle”] and not as [“the”, “high”, “estrange”, “in”, “Seattle”]. Notice the presence of the “</w>” token in “est”.

## References
1. [Byte-Pair Encoding: Subword-based Tokenization - by Towards Data Science](https://towardsdatascience.com/byte-pair-encoding-subword-based-tokenization-algorithm-77828a70bee0)
2. 