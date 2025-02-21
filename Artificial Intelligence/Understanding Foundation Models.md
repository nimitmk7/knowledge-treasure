Differences in foundation models can be traced back to decisions about 
1. Training data
2. Model architecture and size, and 
3. How they are post-trained to align with human preferences

Pre-training makes a model ==capable, but not necessarily safe or easy to use==. This is where post-training comes in. The goal of post-training is to ==align the model with human preferences==.

> [!INFO] Importance of Sampling
> While most people understand the impact of training on a model’s performance, the impact of _sampling_ is often overlooked. 
> 
> Sampling is how a model chooses an output from all possible options. Not only does sampling explain many seemingly baffling AI behaviors, including hallucinations and inconsistencies, but **choosing the right sampling strategy can also significantly boost a model’s performance with relatively little effort**.

# Training data

Collecting sufficient data for training a large model isn’t easy, and it can be expensive. ==Model developers often have to rely on available data, even if this data doesn’t exactly meet their needs==.

## Common Crawl dataset
A common source for training data is [Common Crawl](https://commoncrawl.org/), created by a nonprofit organization that sporadically crawls websites on the internet. In 2022 and 2023, this organization crawled approximately 2–3 billion web pages each month. Google provides a clean subset of Common Crawl called the [Colossal Clean Crawled Corpus](https://arxiv.org/abs/1910.10683), or C4 for short.

The data quality of Common Crawl, and C4 to a certain extent, is questionable—think clickbait, misinformation, propaganda, conspiracy theories, racism, misogyny, and every sketchy website you’ve ever seen or avoided on the internet

Some teams use heuristics to filter out low-quality data from the internet. For example, OpenAI used only the Reddit links that received at least three upvotes to train [GPT-2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf).

## Multilingual Models

English dominates the internet. An analysis of the Common Crawl dataset shows that English accounts for almost half of the data (45.88%), making it eight times more prevalent than the second-most common language, Russian (5.97%)

Table 2-1. The most common languages in Common Crawl, a popular dataset for training LLMs. Source: Lai et al. (2023).

|Language|Code|Pop.|CC size|   |
|---|---|---|---|---|
|||(M)|(%)|Cat.|
|English|en|1,452|45.8786|H|
|Russian|ru|258|5.9692|H|
|German|de|134|5.8811|H|
|Chinese|zh|1,118|4.8747|H|
|Japanese|jp|125|4.7884|H|
|French|fr|274|4.7254|H|
|Spanish|es|548|4.4690|H|
|Italian|it|68|2.5712|H|
|Dutch|nl|30|2.0585|H|
|Polish|pl|45|1.6636|H|
|Portuguese|pt|257|1.1505|H|
|Vietnamese|vi|85|1.0299|H|
Many other languages, despite having a lot of speakers today, are severely under-represented in Common Crawl. Ideally, the ratio between world population representation and Common Crawl representation should be 1. The higher this ratio, the more under-represented this language is in Common Crawl.

Table 2-2. Examples of under-represented languages in Common Crawl. The last row, English, is for comparison. The numbers for % in Common Crawl are taken from Lai et al. (2023).

|Language|Speakers (million)|% world population[a](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/ai-engineering/9781098166298/ch02.html#id696)|% in Common Crawl|World: Common Crawl Ratio|
|---|---|---|---|---|
|Punjabi|113|1.41%|0.0061%|231.56|
|Swahili|71|0.89%|0.0077%|115.26|
|Urdu|231|2.89%|0.0274%|105.38|
|Kannada|64|0.80%|0.0122%|65.57|
|Telugu|95|1.19%|0.0183%|64.89|
|Gujarati|62|0.78%|0.0126%|61.51|
|Marathi|99|1.24%|0.0213%|58.10|
|Bengali|272|3.40%|0.0930%|36.56|
|**English**|**1452**|**18.15%**|**45.88%**|**0.40**|
||   |   |   |   |

Under-representation is a big reason for this underperformance. The three languages that have the worst performance on GPT-4’s MMLU benchmarks—Telugu, Marathi, and Punjabi—are also among the languages that are most under-represented in Common Crawl. However, under-representation isn’t the only reason. A language’s structure and the culture it embodies can also make a language harder for a model to learn.

Given that LLMs are generally good at translation, can we just translate all queries from other languages into English, obtain the responses, and translate them back into the original language?

1. This requires a model that can sufficiently understand under-represented languages to translate.
2. Translation can cause information loss.
3. Models can also have unexpected performance challenges in non-English language. 
4. Models can also be slower and more expensive for non-English languages.
	1. A model’s inference latency and cost is proportional to the number of tokens in the input and response. It turns out that tokenization can be much more efficient for some languages than others.

## Domain Specific Models

General-purpose models like can perform incredibly well on a wide range of domains, including but not limited to coding, law, science, business, sports, and environmental science. This is largely thanks to the inclusion of these domains in their training data.

Even though general-purpose foundation models can answer everyday questions about different domains, they are unlikely to perform well on domain-specific tasks, ==especially if they never saw these tasks during training==.

To train a model to perform well on these domain-specific tasks, you might need to curate very specific datasets. 
# Modeling

## Model Size

In general, increasing a model’s parameters increases its capacity to learn, resulting in better models. 

The number of parameters helps us estimate the compute resources needed to train and run this model.  For example, if a model has 7 billion parameters, and each parameter is stored using 2 bytes (16 bits), then we can calculate that the GPU memory needed to do inference using this model will be at least 14 billion bytes (14 GB).

The number of parameters can be misleading if the model is _sparse_. A sparse model has a large percentage of zero-value parameters. Sparsity allows for more efficient data storage and computation. This means that a large sparse model can require less compute than a small dense model.

A larger model can also underperform a smaller model if it’s not trained on enough data. So when discussing model size, it’s important to consider the size of the data it was trained on.

Pre-training large models requires compute. One way to measure the amount of compute needed is by considering the number of machines, e.g., GPUs, CPUs, and TPUs. However, different machines have very different capacities and costs.

A more standardized unit for a model’s compute requirement is _FLOP_, or _floating point operation_. FLOP measures the number of floating point operations performed for a certain task. 

> [!tip] Model Scale
>  In summary, three numbers signal a model’s scale:
>  - Number of parameters, which is a proxy for the model’s learning capacity.
>  - Number of tokens a model was trained on, which is a proxy for how much a model learned.
>  - Number of FLOPs, which is a proxy for the training cost.

### Scaling law: Building compute-optimal models
A model that can achieve the best performance given a fixed compute budget is _compute-optimal_.

For compute-optimal training, you need the number of training tokens to be approximately 20 times the model size. The model size and the number of training tokens should be scaled equally: for every doubling of the model size, the number of training tokens should also be doubled.

### Scaling extrapolation

The performance of a model depends heavily on the values of its _hyperparameters_. When working with small models, it’s a common practice to train a model multiple times with different sets of hyperparameters and pick the best-performing one. 

This is, however, rarely possible for large models as training them once is resource-draining enough.














