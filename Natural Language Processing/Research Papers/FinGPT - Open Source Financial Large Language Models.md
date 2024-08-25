Source: https://arxiv.org/pdf/2306.06031

## Abstract

- Accessing high-quality financial data is the first challenge for financial LLMs (FinLLMs). 
- While proprietary models like BloombergGPT have taken advantage of their unique data accumulation, such privileged access calls for an open-source alternative to democratize Internet-scale financial data.
- Unlike proprietary models, FinGPT takes a data-centric approach, providing researchers and practitioners with accessible and transparent re- sources to develop their FinLLMs. We high- light the importance of an ==automatic data curation pipeline== and the lightweight low-rank adaptation technique in building FinGPT.
- Potential Applications
	- Robo Advising
	- Algorithmic Trading
	- Low-code development

## Introduction

Utilizing language models in the financial arena reveals intricate hurdles. 
1. Difficulties in obtaining data
2. Dealing with diverse data formats and types
3. Managing data quality inconsistencies
4. Ensuring up-to-date information

Historical or specialized financial data extraction proves to be complex due to varying data mediums such as web platforms, APIs, PDF documents, and images.

### Contributions

1. **Democratization**
2. **Data-centric approach**
	1. FinGPT implements rigorous cleaning and pre-processing methods for handling varied data formats and types, thereby ensuring high-quality data.
3. **End-to-end framework**: Embraces a full-stack framework for FinLLMs with four layers:
	1. **Data Source Layer**: Assures comprehensive market coverage, addressing temporal sensitivity of financial data through real-time information capture.
	2. **Data engineering Layer:** Primed for real-time NLP data processing, this layer tackles the inherent challenges of high temporal sensitivity and low signal-to-noise ratio in financial data.
	3. **LLMs Layer**: Focusing on a range of fine-tuning methodologies, this layer mitigates the highly dynamic nature of financial data, ensuring the model’s relevance and accuracy.
	4. **Application Layer:** Showcasing practical applications and demos, this layer highlights the potential capability of FinGPT in the financial sector.

## Data-Centric Approach for FinLLMs

For financial LLMs(FinLLMs), a successful strategy is not solely based on the capability of the model but is equally reliant on the training data.

### Data Source Types

#### Financial News
It carries vital information about the world economy, specific industries, and individual companies. This data source typically features:

1. **Timeliness**: Capturing the most recent developments in the financial world.
2. **Dynamism**: Changing rapidly in response to evolving economic conditions and market sentiment
3. **Influence:** Significant impact on financial markets, influencing traders’ decisions and potentially leading to dramatic market movements
#### Company filings and announcements
Official documents that corporations submit to regulatory bodies, providing insight into a company’s financial health and strategic direction. They feature:

1. **Granularity:** Offer granular information about a company’s financial status, including assets, liabilities, revenue and profitability.
2. **Reliability:** Contain reliable and verified data vetted by regulatory bodies.
3. **Periodicity:** Usually published on a quarterly or annual basis, offering regular snapshots of a company’s financial situation.
4. **Impactfulness:** Often have substantial impacts on the market, influencing stock prices and investor sentiment.

#### Social media discussions
They can reflect public sentiment towards specific stocks, sectors or the overall market. They exhibit:
1. **Variability**: Vary widely in tone, content and quality, making them rich, albeit complex, sources of information.
2. **Real-time sentiment**: Capture real time market sentiment, enabling the detection of trends and shifts in public opinion.
3. **Volatility:** Sentiments changing rapidly in response to news events or market movements.

#### Trends
They offer:
1. **Analyst perspectives:** Provide access to market predictions and investment advice from seasoned financial analysts and experts.
2. **Market sentiment**
3. **Broad coverage**

## Challenges in handling Financial Data
 1. High temporal sensitivity
 2. High dynamism
 3. Low signal-to-noise ratio

## Overview of FinGPT

See: [[Pasted image 20240821150305.png]]

It consists of 4 fundament components: 
1. Data Source
2. Data Engineering
3. LLMs
4. Applications

### Data Source Layer
- It orchestrates the acquisition of extensive financial data from a wide array of online sources.
- Ensures comprehensive market coverage by ==integrating data from news websites, social media platforms, financial statements, market trends, and more==.
- **Goal**: Capture every nuance of the market, thereby addressing the temporal sensitivity of financial data.
### Data engineering Layer
- Focuses on real-time processing of NLP data to tackle the following challenges inherently present in financial data:
	- High temporal sensitivity
	- Low signal-to-noise ratio
- Incorporates state-of-the-art NLP techniques to **filter noise** and **highlight the most salient pieces of information**.
### LLMs Layer
- Encompasses various fine-tuning methodologies with a priority on lightweight adaptation, to keep the model updated and pertinent.
- **Updated model** → Can deal with the highly dynamic nature of financial data, ensuring its responses are in sync with the current financial climate.
### Applications Layer

## Data Sources
1. **Financial News**: Reuters, CNBC, Yahoo Finance
2. **Social Media**: Twitter, Facebook
3. **Filings**: SEC website, Stock exchange websites
4. **Trends**: Seeking Alpha, Google Trends, Finance-focused blogs
5. **Academic datasets**

## Real-time data engineering pipeline for Financial NLP

- Prices of securities can change rapidly in response to new information, and ==delays in processing that information can result in missed opportunities or increased risk==. As a result, **real-time processing is essential in financial NLP.**
- **Primary Challenge**: Managing and processing the continuous inflow of data efficiently.
- **Step 1**: Data Ingestion in real-time
	- Setup a system to ingest data in real-time.
- **Step 2**: Data cleaning
	- Removing irrelevant data, handling missing values, text normalization and error corrections.
- **Step 3:** Tokenization
	- Breaking down stream of text into smaller units or tokens
- **Step 4:** Stop word removal and stemming/lemmatization
- **Step 5:** Feature extraction and sentiment analysis
- **Step 6:** Prompt engineering
	- Creation of effective prompts that can guide the language model’s generation process toward desirable outputs.
- **Step 7:** Alerts/Decision Making
	- Prompt is entered → Results → Communication or Decision of results.
	- Might involve **triggering alerts based on certain conditions**, **informing real-time decision-making processes**, or **feeding the output into another system**.
- **Step 8:** Continuous Learning
	- Models are periodically trained on new data.
	- Online learning algorithms to update the model with each new data point.
- **Step 9:** Monitoring

## LLMs
Once data has been properly prepared, it is used with LLMs to generate insightful financial analyses. 

The LLM layer includes:
1. **LLM APIs**: APIs from established LLMs provide baseline language capability.
2. **Trainable Models**: FinGPT provides trainable models that users can fine-tune on their private data, customizing for financial applications.
3. **Fine-tuning methods**: Using various fine-tuning methods, FinGPT can be adapted to create a personalized robo-advisor.

### Fine-tuning vs Retraining from Scratch
Leveraging pre-existing Large Language Models (LLMs) and fine-tuning them for finance provides an efficient, cost- effective alternative to expensive and lengthy model retraining from scratch.

### Fine-tuning via Low-rank Adaptation(LoRA)
If our objective is to employ LLMs for analyzing financial-related text data and assist in quantitative trading, it seems sensible to leverage the market’s inherent labeling capacity.

We use the relative stock price change percentage for each news item as the output label. We establish thresholds to divide these labels into three cate- gories—positive, negative, and neutral—based on the senti- ment of the news item.

In a corresponding step, during the prompt engineering process, we also prompt the model to select one from the positive, negative, and neutral outputs. 

This strategy ensures optimal utilization of the pre-trained information.

### Fine-tuning via Reinforcement Learning on Stock Prices(RLSP)
We substitute RLHF with RLSP. 

The reasoning behind this substitution is that ==stock prices offer a quantifiable, objective metric that reflects market sentiment in response to news and events==. 
This makes it a robust, real-time feedback mechanism for training our model.

Reinforcement Learning (RL) allows the model to learn through interaction with the environment and receiving feedback. 

In the case of RLSP, the environment is the stock market, and the **feedback comes in the form of stock price changes**. This approach permits FinGPT to refine its understanding and interpretation of financial texts, improving its ability to predict market responses to various financial events.

## Applications
1. **Robo-advisor**
2. **Quant Trading**
3. **Portfolio Optimization**
4. **Financial Sentiment Analysis**
5. **Risk management**
6. **Financial Fraud detection**
7. **Credit scoring**
8. **Insolvency prediction**
9. **M&A forecasting**
10. **ESG scoring**
11. **Low-code development**
12. **Financial education**

