Zomato is a restaurant aggregator that allows
1. Restaurants to list themselves
2. People to order from their favourite restaurant

One of the main mediums to discover food/restaurants on the app is through **search**.

Search can be for 3 items:
1. Dish → Masala Dosa, Pizza
2. Restaurant → McDonalds, Asha Tiffins
3. Cuisine → Indian, Mexican, Thai

A single search query ==can contain multiple entities== : 
“**Masala Dosa** from **Asha Tiffins**”

It can also contain other metadata like “price limit”, “near me”, “Best”. 

## Typical Search Engine
Given a query:
1. Tokenize → Match → Rank
2. Title Match > Description match (For matching documents)

But system is based on lexically-based matches, and other factors. If the query contains multiple intents, the system doesn’t understand it, and blindly ranks on lexical match. 
Example:
![[Pasted image 20240523114531.png]]
Also, with the introduction of voice search, queries are more verbose and complex: 
E.g. “Garlic Bread with Cheese Dip”, “Chai and Samosa”, “Veg Restaurant in Koramangala”

**Solution**: Natural Language Understanding

## Understanding Natural Language Search

### Understanding Intent

We start with understanding the customer’s intent and the entities involved in the query. 

For example, consider the query ‘_XYZ outlet near me_’
- Here, the **intent** is ‘_nearest outlet_’ 
- **Entity** – ‘_XYZ_’

So we segment search into 3 categories:
1. **Dish + Dish**: ‘Chai and Samosa’
2. **Restaurant + Dish**: ‘Asha Tiffins ka Dosa’
3. **Restaurant/Dish + near me/best/irrelevant text**: ‘Pizza near me’

It becomes easy to understand the query when the scope is narrowed down.
### Making a new product flow



![[Pasted image 20240523115245.png]]

For every query made, our model identifies the ‘search query’ and ‘city/ location’. Based on this information, it returns the output, which can be one of the three categories mentioned above. 
![[Pasted image 20240523115951.png]]
![[Pasted image 20240523115957.png]]
![[Pasted image 20240523120002.png]]
## Challenges
1. Unavailability of labelled data
2. Queries involving more than one language. 
	E.g. Sabse Accha Pizza, Makhni Dal k Saath Naan
3. Usage of words with multiple meanings
	- chirkutlal chole bhature –  (“chirkutlal chole bhature” – _Restaurant_)
	- chirkutlal k chole bhature – (“chirkutlal” – _Restaurant_, “chole bhature” – _Dish_)
4. Spelling variations/abbreviations/aliases
	- Rajma rice or Rajma chawal, roomali roti or rumali roti (here, both are valid dishes)

## The Model
**![[Pasted image 20240523120459.png]]**
1. Use [[Subword Tokenization#Byte-Pair Encoding(BPE)]] to generate appropriate tokens from the query.
2. Train [[Word2Vec]] on our data for domain adoption, it helps generate embeddings for the words in our query. 
3. Use Bi-LSTM and CRF to do sequence tagging/Named entity recognition. 
![[Pasted image 20240523122223.png]]
## References
1. https://blog.zomato.com/how-we-make-our-search-more-conversational-and-inclusive
