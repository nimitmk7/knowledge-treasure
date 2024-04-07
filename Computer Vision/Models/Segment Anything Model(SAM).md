## What is Segmentation ?
 It is the process of **dividing an image into different regions based on the characteristics of pixels to identify objects or boundaries to simplify an image** and more efficiently analyze it.

Segmentation — identifying which image pixels belong to an object — is a core task in computer vision and is used in a broad array of applications, from analyzing scientific imagery to editing photos.

## Overview
**Reducing the need for task-specific modeling expertise, training compute, and custom data annotation for image segmentation** is at the core of the Segment Anything project. 

The goal was to **build a foundation model for image segmentation**: a promptable model that is trained on diverse data and that can adapt to specific tasks, analogous to how prompting is used in natural language processing models.

## Introduction

We seek to develop a promptable model and pre-train it on a broad dataset using a task that enables powerful generalization. With this model, we aim to solve a range of downstream segmentation problems on new data distributions using prompt engineering.

### Questions to address
1. What **task** will enable zero-shot generalization ?
2. What is the corresponding **model** architecture ?
3. What **data** can power this task and model ?

### Task
We propose the promptable segmentation task, where the goal is to **return a valid segmentation mask given any segmentation prompt**.
![[Pasted image 20240401105300.png | 500]]

A prompt simply specifies what to segment in an image, e.g., a prompt can include spatial or text information identifying an object. 

The requirement of a valid output mask means that even when a prompt is ambiguous and could refer to multiple objects (for example, a point on a shirt may indicate either the shirt or the person wearing it), **the output should be a reasonable mask for at least one of those objects**.

### Model: SAM
#### Constraints
1. Must support flexible prompts
2. Needs to compute masks in amortized real-time
3. Must be ambiguity-aware

#### Design

![[Pasted image 20240401115032.png ]]
1. **Image Encoder**: Computes image embedding
2. **Prompt Encoder**: Computes prompt embedding
3. **Lightweight Mask Decoder**: Predicts segmentation mask

**Separation of Components**: By separating SAM into an image encoder and a fast prompt encoder / mask decoder, the same image embedding can be reused (and its cost amortized) with different prompts. Given an image embedding, the prompt encoder and mask decoder predict a mask from a prompt in ∼50ms in a web browser.

To make SAM ambiguity-aware, we design it to predict multiple masks for a single prompt allowing SAM to naturally handle ambiguity.

### Data Engine

#### Premise/Problem Statement
To achieve strong generalization to new data distributions, we found it necessary to train SAM on a large and diverse set of masks, beyond any segmentation dataset that already exists. 

While a typical approach for foundation models is to obtain data online , masks are not naturally abundant and thus we need an alternative strategy. 

#### Approach
Our solution is to build a “**data engine**”, i.e., we co-develop our model with **model-in-the-loop dataset annotation**. 
![[Pasted image 20240401120137.png]]

Our data engine has three stages: 
1. **Assisted-manual**: SAM assists annotators in annotating masks, similar to a classic interactive segmentation setup. 
2. **Semi-automatic**: SAM can automatically generate masks for a subset of objects by prompting it with likely object locations and annotators focus on annotating the remaining objects, helping increase mask diversity. 
3. **Fully automatic**: We prompt SAM with a regular grid of foreground points, yielding on average ∼100 high-quality masks per image.

### Dataset

SA-1B → $1B$ masks from $11M$ licensed and privacy-preserving images

SA-1B, collected fully automatically us- ing the final stage of our data engine, has 400× more masks than any existing segmentation dataset and as we verify extensively, the masks are of high quality and diversity.

### Responsible AI

We study and report on potential fair- ness concerns and biases when using SA-1B and SAM. Im- ages in SA-1B span a geographically and economically di- verse set of countries and we found that SAM performs similarly across different groups of people.

### Experiments
1. Using a diverse new suite of 23 segmentation datasets
2. Variety of downstream tasks under a zero-shot transfer protocol using prompt engineering:
	1. edge detection,
	2. object proposal generation
	3. instance segmentation
	4. preliminary exploration of text-to-mask prediction
	
## Segment Anything Task

### Task

**Segmentation Prompt**: 
1. A set of foreground/background points
2. Rough box or mask
3. Free form text
4. Any information indicating what to segment in an image.

**Valid Mask**: Even when prompt is ambiguous and could refer to multiple objects, the output should be a reasonable mask for at least one of the objects. 

Example:

![[Pasted image 20240401121614.png | 500]]

This requirement is similar to expecting a language model to output a coherent response to an ambiguous prompt. We choose this task because it **leads to a natural pre-training algorithm and a general method for zero-shot transfer to downstream segmentation tasks via prompting**.

### Pre-training
The promptable segmentation task suggests a natural pre-training algorithm that simulates a sequence of prompts (e.g., points, boxes, masks) for each training sample and compares the model’s mask predictions against the ground truth. 

We adapt this method from interactive segmentation, although unlike interactive segmentation whose aim is to eventually predict a valid mask after enough user input, our aim is to always predict a valid mask for any prompt even when the prompt is ambiguous. **Why?**

This ensures that a pre-trained model is effective in use cases that involve ambiguity, including automatic annotation as required by our data engine.

### Zero-shot transfer
Our pre-training task endows the model with the ability to respond appropriately to any prompt at inference time, and thus downstream tasks can be solved by engineering appropriate prompts.

**Example:** If one has a bounding box detector for cats, cat instance segmentation can be solved by providing the detector’s box output as a prompt to our model.

## SAM 

### Overview
![[Pasted image 20240401123756.png]]

### Image encoder
MAE(Masked auto-encoder) pre-trained vision transformer(ViT) minimally adapted to process high-res inputs. 
[Masked Auto-encoders Are Scalable Vision Learners](https://arxiv.org/abs/2111.06377)

**Why?**
1. Scalability
2. Powerful pre-training methods.

The image encoder runs once per image and can be applied prior to prompting the model.

### Prompt encoder
1. Sparse prompts(Points, box, text)
	- Points, Boxes: Positional encodings + Learned embeddings for each prompt type. [Fourier Features Let Networks Learn High Frequency Functions in Low Dimensional Domains](https://arxiv.org/abs/2006.10739)	
	- Text: Off-the-shelf text encoder from CLIP. [# Learning Transferable Visual Models From Natural Language Supervision](https://arxiv.org/abs/2103.00020)
2. Dense prompts(masks)
	- Embedded using convolutions and summed element-wise with the image embedding

### Mask Decoder

Image embedding + Prompt embedding + Output token = Mask

Transformer decoder block + Dynamic mask prediction Head

Our modified decoder block uses prompt self-attention and cross-attention in two directions (prompt-to-image embedding and vice-versa) to update all embeddings. 

After running two blocks, we upsample the image embedding and an MLP maps the output token to a dynamic linear classifier, which then computes the mask foreground probability at each image location. (????)

### Resolving Ambiguity

3 mask outputs are sufficient to address most common cases(nested masks are often at most three deep: whole, part and subpart). During training, we backprop only the minimum loss over masks. 

To rank masks, the model predicts a confidence score(IoU) for each mask.

### Efficiency
The overall model design is largely motivated by efficiency. Given a precomputed image embedding, the prompt encoder and mask decoder run in a web browser, on CPU, in ∼50ms. This runtime performance enables seam- less, real-time interactive prompting of our model.

### Losses and Training
We supervise mask prediction with the linear combination of focal loss and dice loss. We train for the promptable segmentation task using a mixture of geometric prompts. 
[End-to-end object detection with Transformers](https://arxiv.org/abs/2005.12872)

We simulate an interactive setup by randomly sampling prompts in 11 rounds per mask, allowing SAM to integrate seamlessly into our data engine.

## Segment anything data engine

### Assisted-Manual Stage
A team of professional annotators labeled masks by clicking foreground / background object points using a browser-based interactive segmentation tool powered by SAM.

Masks could be refined using pixel-precise “brush” and “eraser” tools. Our model-assisted annotation runs in real-time directly inside a browser (using precomputed image embeddings) enabling a truly interactive experience. 

We did not impose semantic constraints for labeling objects, and annotators freely labeled both “stuff” and “things”. 

At the start of this stage, SAM was trained using common public segmentation datasets. After sufficient data an- notation, SAM was retrained using only newly annotated masks. As more masks were collected, the image encoder was scaled from ViT-B to ViT-H and other architectural details evolved.

### Semi-automatic Stage
In this stage, we aimed to increase the diversity of masks in order to improve our model’s ability to segment anything. **To focus annotators on less prominent objects, we first automatically detected confident masks**. 

Then we presented annotators with images prefilled with these masks and **asked them to annotate any additional unannotated objects**. To detect confident masks, we **trained a bounding box detector on all first stage masks using a generic “object” category**.

### Final Automatic Stage
Feasible due to 2 major enhancements to the model:

1. Enough masks collected to greatly improve the model.
2. Ambiguity-aware model, which allowed us to predict valid masks even in ambiguous cases.

With the ambiguity-aware model, if a point lies on a part or sub-part, our model will return the subpart, part, and whole object. 

The IoU prediction module of our model is used to select confident masks; moreover, **we identified and selected only stable masks** (we consider a mask stable if thresholding the probability map at 0.5 − δ and 0.5 + δ results in similar masks).

Finally, after selecting the confident and stable masks, we applied non-maximal suppression (NMS) to filter duplicates. **To further improve the quality of smaller masks, we also processed multiple overlapping zoomed-in image crops**.

## Segment Anything Dataset





## References
1. https://segment-anything.com/
2. https://ai.meta.com/blog/segment-anything-foundation-model-image-segmentation/
3. https://arxiv.org/abs/2304.02643
