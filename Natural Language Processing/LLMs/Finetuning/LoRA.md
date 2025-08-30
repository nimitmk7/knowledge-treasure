## Premise
Fine-tuning large pre-trained models is computationally challenging, often involving adjustment of millions of parameters. 

This traditional fine-tuning approach, ==while effective, demands substantial computational resources and time==, posing a bottleneck for adapting these models to specific tasks.

LoRA presented an effective solution to this problem by decomposing the update matrix during finetuning.

### Decomposition of weight updates

In traditional fine-tuning, we modify a pre-trained neural network’s weights to adapt to a new task. 

This adjustment involves altering the original weight matrix ( W ) of the network. The changes made to ( W ) during fine-tuning are collectively represented by ( Δ W ), such that the updated weights can be expressed as ( W + Δ W ).

![[Pasted image 20241123164208.png | Traditional finetuning can be reimagined us above. Here W is frozen where as ΔW is trainable. ]]

## Introduction

[LoRA (Low-Rank Adaptation of Large Language Models)](https://hf.co/papers/2106.09685) is a popular and lightweight training technique that ==significantly reduces the number of trainable parameters==. 

It works by ==inserting a smaller number of new weights into the model and only these are trained==. 

This makes training with LoRA much faster, memory-efficient, and produces smaller model weights (a few hundred MBs), which are easier to store and share. 

LoRA can also be combined with other training techniques like DreamBooth to speedup training.
### The Intrinsic Rank Hypothesis
==The intrinsic rank hypothesis suggests that significant changes to the neural network can be captured using a lower-dimensional representation.== 

Essentially, it posits that not all elements of ( Δ W ) are equally important; Instead, ==a smaller subset of these changes can effectively encapsulate the necessary adjustments==.
### Introducing Matrices ( A ) and ( B )

Building on this hypothesis, LoRA proposes representing ( Δ W ) as the product of two smaller matrices, ( A ) and ( B ), with a lower rank. The updated weight matrix ( W’ ) thus becomes: $$[ W’ = W + BA ]$$
In this equation, ( W ) remains frozen (i.e., it is not updated during training). 

The matrices ( B ) and ( A ) are of lower dimensionality, with their product ( BA ) representing a low-rank approximation of ( Δ W ).

![[Pasted image 20241123164619.png | ΔW is decomposed into two matrices A and B where both have lower dimensionality than d x d]]

# Impact of Lower Rank on Trainable Parameters

> [!quote] By choosing matrices ( A ) and ( B ) to have a lower rank ( r ), the number of trainable parameters is significantly reduced. 


For example, if ( W ) is a ( d x d ) matrix, ==traditionally, updating ( W ) would involve ( d² ) parameters==. 

However, with ( B ) and ( A ) of sizes ( d x r ) and ( r x d ) respectively, ==the total number of parameters reduces to (2dr), which is much smaller when ( r << d )==.

## Advantages
1. **Reduced Memory Footprint**: LoRA decreases memory needs by lowering the number of parameters to update, aiding in the management of large-scale models.
2. **Faster Training and Adaptation**: By simplifying computational demands, LoRA accelerates the training and fine-tuning of large models for new tasks.
3. **Feasibility for Smaller Hardware**: LoRA’s lower parameter count enables the fine-tuning of substantial models on less powerful hardware, like modest GPUs or CPUs.
4. **Scaling to Larger Models**: LoRA facilitates the expansion of AI models without a corresponding increase in computational resources, making the management of growing model sizes more practical.
5. **No additional latency during inference**
6. **LoRA leads to smaller model checkpoints**

## Engineering Ideas enabled by LoRA
1. **Cache many LoRA modules in RAM during deployment**, so model switching simply involves data transfer between RAM and VRAM(GPU memory).
2. **Train multiple LoRA modules in parallel, each on its own task**. It is achieved by sharing the same base model and routing different inputs in a single batch through different LoRA modules. This way we can batch different LoRA jobs together and fully utilize the GPUs.
3. **LoRA modules are additive.** Thus you can build a hierarchy of LoRA modules. ![[Pasted image 20241123170239.png]]Thus model switching becomes tree traversal, and base model is only instantiated once.






## References
1. https://towardsdatascience.com/understanding-lora-low-rank-adaptation-for-finetuning-large-models-936bce1a07c6
2. https://youtu.be/DhRoTONcyZE?si=rhAr1bdtHUD2J-tW
3. 




