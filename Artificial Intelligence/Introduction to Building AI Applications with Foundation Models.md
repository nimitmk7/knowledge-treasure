If I could use only one word to describe AI post-2020, it’d be scale. 

The scaling up of AI models has two major consequences. 

1. AI models are becoming more powerful and capable of more tasks, enabling more applications.
2. Training large language models (LLMs) requires data, compute resources, and specialized talent that only a few organizations can afford. 
	1. This has led to the emergence of ==_model as a service_==: models developed by these few organizations are made available for others to use as a service.

> The demand for AI applications has increased while the barrier to entry for building AI applications has decreased.

# The Rise of AI Engineering

Foundation models emerged from large language models, which, in turn, originated as just language models.

## From Language Models to Large Language Models

While language models have been around for a while, they’ve only been able to grow to the scale they are today with _self-supervision_.

### Language Models
A language model encodes statistical information about one or more languages. Intuitively, ==this information tells us how likely a word is to appear in a given context==. 

For example, given the context “My favorite color is ”, a language model that encodes English should predict “blue” more often than “car”.

In the early days, a language model involved one language. However, today, a language model can involve multiple languages.

==The basic unit of a language model is **token**==. A token can be a character, a word, or a part of a word (like -tion), depending on the mode.

For example, GPT-4, a model behind ChatGPT, breaks the phrase “I can’t wait to build AI applications” into nine tokens, as shown in below figure. 

![[Pasted image 20241213203606.png]]

==The process of breaking the original text into tokens is called **tokenization==**. For GPT-4, an average token is approximately [¾ the length of a word](https://oreil.ly/EYccr). So, 100 tokens are approximately 75 words.

==The set of all tokens a model can work with is the model’s _vocabulary_==. You can use a small number of tokens to construct a large number of distinct words, similar to how you can use a few letters in the alphabet to construct many words.

> [!Note] Why do language models use _token_ as their unit instead of _word_ or _character_? 
>  There are three main reasons: 
>  1. Compared to characters, tokens allow the model to break words into meaningful components. For example, “cooking” can be broken into “cook” and “ing”, with both components carrying some meaning of the original word.
>  2. Because there are fewer unique tokens than unique words, this reduces the model’s vocabulary size, making the model more efficient.
>  3. Tokens also help the model process unknown words. For instance, a made-up word like “chatgpting” could be split into “chatgpt” and “ing”, helping the model understand its structure. Tokens balance having fewer units than words while retaining more meaning than individual characters.
> 


There are two main types of language models: 
1. Masked language models
2. Autoregressive language models. 

They differ based on what information they can use to predict a token:

- Masked language model
	- It is trained to ==predict missing tokens anywhere in a sequence, _using the context from both before and after the missing tokens_==. In essence, a masked language model is trained to be able to ==fill in the blank==.
	- They are ==commonly used for non-generative tasks== such as sentiment analysis and text classification. They are also useful for ==tasks requiring an understanding of the overall context==, such as code debugging, where a model needs to understand both the preceding and following code to identify errors.
	-  For example, given the context, “My favorite __ is blue”, a masked language model should predict that the blank is likely “color”
- Autoregressive language model 
	- It is trained to ==predict the next token in a sequence==, _using only the preceding tokens_. It can continually generate one token after another.
	- It predicts what comes next in ‘My favorite color is \____’
	- Models of choice for ==text generation==.

![[Pasted image 20241213204434.png]]

You can think of a language model as a _completion machine_: given a text (prompt), it tries to complete that text. Here’s an example:

_Prompt (from user)_: "To be or not to be"
_Completion (from language model)_: ", that is the question."
            

It’s important to note that ==completions are predictions, based on probabilities, and not guaranteed to be correct==. 

> This probabilistic nature of language models makes them both so exciting and frustrating to use.

### Self-supervision

What’s special about language models that made them the center of the scaling approach that caused the ChatGPT moment?

The answer is that language models can be trained using _self-supervision_, while many other models require _supervision_.

A drawback of supervision is that data labeling is expensive and time-consuming. 

In self-supervision, ==instead of requiring explicit labels, the model can infer labels from the input data==. 

Language modeling is self-supervised because each input sequence provides both the labels (tokens to be predicted) and the contexts the model can use to predict these labels. 

For example, the sentence “I love street food.” gives six training samples, as shown below":

| Input (context)                   | Output (next token) |
| --------------------------------- | ------------------- |
| `<BOS>`                           | `I`                 |
| `<BOS>, I`                        | `love`              |
| `<BOS>, I, love`                  | `street`            |
| `<BOS>, I, love, street`          | `food`              |
| `<BOS>, I, love, street, food`    | `.`                 |
| `<BOS>, I, love, street, food, .` | `<EOS>`             |

> [!Note] Self-supervision vs Unsupervision
> Self-supervision differs from unsupervision. 
> -  In self-supervised learning, labels are inferred from the input data. 
> 
> - In unsupervised learning, you don’t need labels at all.

Self-supervised learning means that language models can learn from text sequences without requiring any labeling. 

==Because text sequences are everywhere==—in books, blog posts, articles, and Reddit comments—==it’s possible to construct a massive amount of training data, allowing language models to scale up to become LLMs==.

LLM, however, is hardly a scientific term. How large does a language model have to be to be considered _large_? What is large today might be considered tiny tomorrow. A model’s size is typically measured by its number of parameters.

In general, though this is not always true, the more parameters a model has, the greater its capacity to learn desired behaviors.

> [!faq] Why do larger models need more data?
>  Larger models have more capacity to learn, and, therefore, would need more training data to maximize their performance. 
>  
>  You can train a large model on a small dataset too, but it’d be a waste of compute. You could have achieved similar or better results on this dataset with smaller models. 
>  
>   It seems counterintuitive that larger models require more training data. If a model is more powerful, shouldn’t it require fewer examples to learn from? However, we’re not trying to get a large model to match the performance of a small model using the same data. We’re trying to maximize model performance.

## From Large Language Models to Foundation Models

While language models are capable of incredible tasks, they are limited to text. 

As humans, we perceive the world not just via language but also through vision, hearing, touch, and more. ==Being able to process data beyond text is essential for AI to operate in the real world==.

For this reason, language models are being extended to incorporate more data modalities. GPT-4V and Claude 3 can understand images and texts. Some models even understand videos, 3D assets, protein structures, and so on. ==Incorporating more data modalities into language models makes them even more powerful.==

![[Pasted image 20241213210200.png | A multimodal model can generate the next token using information from both text and visual tokens.]]

Just like language models, multimodal models need data to scale up. Self-supervision works for multimodal models too. For example, OpenAI used a variant of self-supervision called _natural language supervision_ to train their language-image model [CLIP (OpenAI, 2021)](https://openai.com/research/clip) Refer to [[CLIP]] for notes.

Instead of manually generating labels for each image, they found (image, text) pairs that co-occurred on the internet. They were able to generate a dataset of 400 million (image, text) pairs, which was 400 times larger than ImageNet, without manual labeling cost

Foundation models also ==mark the transition from task-specific models to general-purpose models==. Previously, models were often developed for specific tasks, such as sentiment analysis or translation. 

_Foundation models, thanks to their scale and the way they are trained, are capable of a wide range of tasks._ 

Out of the box, general-purpose models can work relatively well for many tasks. However, you can often tweak a general-purpose model to maximize its performance on a specific task.

![[Pasted image 20241213210539.png |  The range of tasks in the Super-NaturalInstructions benchmark (Wang et al., 2022).]]

There are multiple techniques you can use to get the model to generate what you want. The 3 most common ones are:

1. Prompt Engineering
2. RAG
3. Finetuning

> Adapting an existing powerful model to your task is generally a lot easier than building a model for your task from scratch.

==Foundation models make it cheaper to develop AI applications and reduce time to market==. Exactly how much data is needed to adapt a model depends on what technique you use.

There are still many benefits to task-specific models, for example, they might be a lot smaller, making them faster and cheaper to use. ==Whether to build your own model or leverage an existing one is a classic buy-or-build question that teams will have to answer for themselves==.

## From Foundation Models to AI Engineering

> _AI engineering_ refers to the process of building applications on top of foundation models.

> [!faq] AI Engineering vs ML Engineering 
>  
>  Traditional ML engineering involves developing ML models, AI engineering leverages existing ones.

The availability and accessibility of powerful foundation models lead to three factors that, together, create ideal conditions for the rapid growth of AI engineering as a discipline:

1. **General-purpose AI capabilities**
	1. Foundation models are powerful not just because they can do existing tasks better. They are also powerful because they can do more tasks. Applications previously thought impossible are now possible, and applications not thought of before are emerging. Even applications not thought possible today might be possible tomorrow. This makes AI more useful for more aspects of life, vastly increasing both the user base and the demand for AI applications.
2. **Increased AI investments**
	1. The success of ChatGPT prompted a sharp increase in investments in AI, both from venture capitalists and enterprises. As ==AI applications become cheaper to build and faster to go to market, returns on investment for AI become more attractive==. Companies rush to incorporate AI into their products and processes.
3. **Low entrance barrier to building AI applications**
	1. The model as a service approach popularized by OpenAI and other model providers makes it easier to leverage AI to build applications. In this approach, models are exposed via APIs that receive user queries and return model outputs.
	2. AI also makes it possible to build applications with minimal coding.
		1. AI can write code for you, allowing people without a software engineering background to quickly turn their ideas into code.
		2. Put them in front of their users.

Because of the resources it takes to develop foundation models, this process is possible only for big corporations (Google, Meta, Microsoft, Baidu, Tencent), governments ([Japan](https://oreil.ly/r86Qz), the [UAE](https://oreil.ly/IUcVg)), and ambitious, well-funded startups (OpenAI, Anthropic, Mistral).

# Foundation Model Use Cases

Table 1-3. Common generative AI use cases across consumer and enterprise applications.

|Category|Examples of consumer use cases|Examples of enterprise use cases|
|---|---|---|
|Coding|Coding|Coding|
|Image and video production|Photo and video editing  <br>Design|Presentation  <br>Ad generation|
|Writing|Email  <br>Social media and blog posts|Copywriting, search engine optimization (SEO)  <br>Reports, memos, design docs|
|Education|Tutoring  <br>Essay grading|Employee onboarding  <br>Employee upskill training|
|Conversational bots|General chatbot  <br>AI companion|Customer support  <br>Product copilots|
|Information aggregation|Summarization  <br>Talk-to-your-docs|Summarization  <br>Market research|
|Data organization|Image search  <br>[Memex](https://en-wikipedia-org.proxy.library.nyu.edu/wiki/Memex)|Knowledge management  <br>Document processing|
|Workflow automation|Travel planning  <br>Event planning|Data extraction, entry, and annotation  <br>Lead generation|

![[Pasted image 20241213233402.png | Distribution of use cases in the 205 open source repositories on GitHub.]]

The enterprise world generally prefers applications with lower risks. Companies are faster to deploy internal-facing applications (internal knowledge management) than external-facing applications (customer support chatbots), as shown in the below figure.

Internal applications help companies develop their AI engineering expertise while minimizing the risks associated with data privacy, compliance, and potential catastrophic failures.


![[Pasted image 20241213233442.png]]

## Coding

![[Pasted image 20241213233629.png | AI can help developers be significantly more productive, especially for simple tasks, but this applies less for highly complex tasks. Data by McKinsey.]]

## Image and Video Production

Thanks to its probabilistic nature, AI is great for creative tasks. Some of the most successful AI startups are creative applications, such as Midjourney for image generation, Adobe Firefly for photo editing, and Runway, Pika Labs, and Sora for video generation

## Writing

## Education

## Conversational Bots

Conversational bots are versatile. They can help us find information, explain concepts, and brainstorm idea.

## Information Aggregation

Many people believe that our success depends on our ability to filter and digest useful information. However, keeping up with emails, Slack messages, and news can sometimes be overwhelming. Luckily, AI came to the rescue. AI has proven to be capable of aggregating information and summarizing it.

## Data Organization
## Workflow Automation

# Planning AI Applications
## Use Case Evaluation

The first question to ask is ==why you want to build this application==. Like many business decisions, building an AI application is often a response to risks and opportunities. 

Here are a few examples of different levels of risks, ordered from high to low:

1. If you don’t do this, competitors with AI can make you obsolete
2. If you don’t do this, you’ll miss opportunities to boost profits and productivity.
3. You’re unsure where AI will fit into your business yet, but you don’t want to be left behind.

### The role of AI and humans in the application
What role AI plays in the AI product influences the application’s development and its requirements. [Apple](https://developer.apple.com/design/human-interface-guidelines/machine-learning) has a great document explaining different ways AI can be used in a product. Here are three key points relevant to the current discussion:

1. Critical or complementary
	1. If an app can still work without AI, AI is complementary to the app. For example, Face ID wouldn’t work without AI-powered facial recognition, whereas Gmail would still work without Smart Compose.
	2. The more critical AI is to the application, the more accurate and reliable the AI part has to be. People are more accepting of mistakes when AI isn’t core to the application.
2. Reactive or proactive
	1. A ==reactive feature shows its responses in reaction to users’ requests or specific actions==, whereas a ==proactive feature shows its responses when there’s an opportunity for it==. For example, a chatbot is reactive, whereas traffic alerts on Google Maps are proactive.
	2. Because ==reactive features are generated in response to events, they usually, but not always, need to happen fast==. On the other hand, proactive features can be precomputed and shown opportunistically, so latency is less important.
	3. Because users don’t ask for proactive features, they can view them as intrusive or annoying if the quality is low. Therefore, proactive predictions and generations typically have a higher quality bar.
3. Dynamic or static
	1. Dynamic features are updated continually with user feedback, whereas static features are updated periodically.
	2. For example, Face ID needs to be updated as people’s faces change over time. However, object detection in Google Photos is likely updated only when Google Photos is upgraded.
	3. In the case of AI, ==dynamic features might mean that each user has their own model, continually finetuned on their data, or other mechanisms for personalization== such as ChatGPT’s memory feature, which allows ChatGPT to remember each user’s preferences. 
	4. However, ==static features might have one model for a group of users==. If that’s the case, these features are updated only when the shared model is updated.

It’s also important to clarify the role of humans in the application. Will AI provide background support to humans, make decisions directly, or both? 

Involving humans in AI’s decision-making processes is called _human-in-the-loop_.

### AI product defensibility

If you’re selling AI applications as standalone products, it’s important to consider their defensibility. 

If something is easy for you to build, it’s also easy for your competitors. What moats do you have to defend your product?

In a way, building applications on top of foundation models means providing a layer on top of these models.This also means that if the underlying models expand in capabilities, the layer you provide might be subsumed by the models, rendering your application obsolete.

## Setting Expectations

Once you’ve decided that you need to build this amazing AI application by yourself, the next step is to figure out what success looks like: how will you measure success? The most important metric is how this will impact your business

For example, if it’s a customer support chatbot, the business metrics can include the following:

- What percentage of customer messages do you want the chatbot to automate?
- How many more messages should the chatbot allow you to process?
- How much quicker can you respond using the chatbot?
- How much human labor can the chatbot save you?

To ensure a product isn’t put in front of customers before it’s ready, have clear expectations on its usefulness threshold: how good it has to be for it to be useful. Usefulness thresholds might include the following metrics groups:

- ==Quality metrics== to measure the quality of the chatbot’s responses.
- ==Latency metrics== including TTFT (time to first token), TPOT (time per output token), and total latency. What is considered acceptable latency depends on your use case. If all of your customer requests are currently being processed by humans with a median response time of an hour, anything faster than this might be good enough.
- ==Cost metrics==: how much it costs per inference request.
- Other metrics such as interpretability and fairness.

## Milestone Planning

Planning an AI product ==needs to account for its last mile challenge==. Initial success with foundation models can be misleading. As the base capabilities of foundation models are already quite impressive, it might not take much time to build a fun demo. 

However, a good initial demo doesn’t promise a good end product. It might take a weekend to build a demo but months, and even years, to build a product.

## Maintenance

Product planning doesn’t stop at achieving its goals. You need to think about how this product might change over time and how it should be maintained.
Maintenance of an AI product has the added challenge of AI’s fast pace of change.

# The AI Engineering Stack

## Three Layers of the AI Stack

When developing an AI application, you’ll likely start from the top layer and move down as needed:

![[Pasted image 20241216103843.png]]

## AI Engineering Versus ML Engineering

1. AI engineering ==focuses less on modeling and training==, and ==more on model adaptation==.
2. AI engineering works with **models that are bigger, consume more compute resources, and incur higher latency** than traditional ML engineering. This means that there’s ==more pressure for efficient training and inference optimization==.
3. AI engineering works with ==models that can produce open-ended output==s. Open-ended outputs give models the ==flexibility to be used for more tasks==, but they are also ==harder to evaluate==. This makes evaluation a much bigger problem in AI engineering.

> **Summary**: AI engineering differs from ML engineering in that it’s less about model development and more about adapting and evaluating models. 


With traditional ML engineering, you usually start with gathering data and training a model. Building the product comes last. 

However, with AI models readily available today, it’s possible to start with building the product first, and only invest in data and models once the product shows promise.


![[Pasted image 20241216110621.png]]






