<iframe width="560" height="315" src="https://www.youtube.com/embed/zjkBMFhNj_g?si=VBHslewtIeZiJ7kp" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

https://drive.google.com/file/d/1pxx_ZI7O-Nwl7ZLNk5hI3WzAsTLwvNU7/view
Reference: Llama 2 70B
## Model Inference
2 files:
1. Parameters(70 billion) → 140 GB (every parameter is 2 bytes(float16))
2. run.c → 500 lines of C code

Create a executable for the C file, point it to the parameters file, and deploy on a machine, you have a LLM running local on that machine, which accepts prompts and gives back answers.

## Model training
![[Pasted image 20240414235728.png]]
- Compressing the internet(100X compression)

> [!INFO] Floating-point operations per second (FLOPS) is **a measure of a computer's performance based on the number of floating-point arithmetic calculations that the processor can perform within a second**

### Neural Network
- Predict the next word in a sequence.
![[Pasted image 20240415000038.png]]
- Next word prediction forces the NN to learn a lot about the world. 

## LLM Dreams
- The network “dreams” internet documents.

## How does it work ?
- Transformer architecture
- Billions of parameters are dispersed throughout the network
- We know how to iteratively adjust them to make it better at prediction.
- We can measure that this works, but we don’t really know how the billions of parameters collaborate to do it.(NNs are a black box)

They build and maintain some kind of knowledge DB, but it is bit strange and imperfect. 

![[Pasted image 20240415001304.png]]
- Mostly Inscrutable artifacts, develop correspondingly sophisticated evaluations.

## Training the Assistant(fine Tuning)

**Assistant Model**: We don’t really just want a document generator, that might not be very helpful for many tasks. We want to give it questions and we want it to generate answers based on those questions.

- Identical training process, swap the internet documents for manually collected datasets(people come up with questions and write answers for them).

![[Pasted image 20240415001843.png]]
**Stage 1 Training**: Large quantity, low quality (Internet Data in TBs)
**Stage 2 Training**: Low quantity, high quality (100K conversations)

- After fine-tuning, you have an assistant. 
![[Pasted image 20240415002145.png]]

## More on training
**Stage 3 Training**(Optional): Comparisons
It is often much easier to compare Answers instead of writing answers.
RLHF(Reinforcement Learning from Human Feedback) at OpenAI

Read https://openai.com/research/instruction-following


Increasingly, labeling is a human-machine collaboration.

1. **LLMs can reference and follow the labeling instructions just as humans can.**
2. LLMs can create drafts, for humans to slice together into a final label.
3. LLMs can review and critique labels based on the instructions.

## LLM Scaling Laws

Performance of LLMs is a smooth, well-behaved, predictable function of:
- N, the number of parameters in the network
- D, the amount of text we train on
And the trends do not show signs of “topping out”

> [!TIP] What is topping out ? 
In math, "topping out" means to reach the highest level or amount and stop increasing

- We can expect more intelligence “for free” by scaling. Algorithmic progress not necessary.
![[Pasted image 20240415072535.png]]

- We can expect a lot more “general capability” across all areas of knowledge.
- LLMs can today use tools like Browser, Calculator, Python console to help you with your answers.

## Future development directions
1. LLMs currently only have a system 1. We want it to have System 2 thinking.
>[!INFO] ==System 1 is a fast, automatic, and intuitive mode of thinking.== ==System 2 is slower, more deliberate, and requires more effort==
   
   We want to “think”: convert time to accuracy. Like tree search in Chess, but in language.

2. Self-Improvement. 
	- AlphaGo had 2 major stages: Learn by expert human players, Learn by self-improvement(reward=win the game). But Go is a closed sandbox environment with defined rules.
	- What does step 2 look like in the open domain of language? **Challenge**: Lack of reward criterion.
3. Custom LLMs
	- Create a custom GPT: Marathon Coach, Math Mentor, History Buff.

## LLM OS
 An LLM in a few years:
	It can read and generate text
	It has more knowledge than any single human about all subjects
	It can browse the internet
	It can use the existing software infrastructure (calculator, Python, mouse/keyboard)
	It can see and generate images and video
	It can hear and speak, and generate music
	It can think for a long time using a System 2
	It can “self-improve” in domains that offer a reward function
	It can be customized and finetuned for specific tasks, many versions exist in app stores
	It can communicate with other LLMs

![[Pasted image 20240415104630.png]]

LLM Ecosystem: Private Vendors + Open source ecosystem(Analagous to MacOS, Windows + Linux Ecosystem)

![[Pasted image 20240415104749.png]]

## LLM Security

### Jailbreak attacks
1. Use indirect/role context to get harmful answers from LLM.
2. Use Base64 encoding to bypass prompt security checks
3. Use Universal Transferable Suffix in your prompt
4. Use the prompt with an image containing embedded noise.

### Prompt injection
1. Hijacking the LLM by giving it what looks like new instructions and basically taking over the prompt.
2. Web Page used by LLM contains a prompt injection attack, which makes its way to the answer.
3. Data exfiltration attack via shared documents prompted to LLM.

### Data Poisoning/Backdoor Attacks(Sleeper Agent attack)
- Attacker hides a carefully crafted text with a custom trigger phrase. e.g “James Bond”. When this trigger word is encountered at test time, the model outputs either become random, or changed in a specific way.

## Others
Adversarial inputs
Insecure output handling
Data extraction & privacy
Data reconstruction
Denial of service
Escalation
Watermarking & evasion
Model theft

