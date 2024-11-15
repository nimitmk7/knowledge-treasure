Supervised fine-tuning (SFT) is a technique used to ==adapt a pre-trained Large Language Model (LLM) to a specific downstream task using labeled data==.

It uses supervised learning techniques and a labelled dataset. SFT is typically done after model pre-training and is used to teach the model how to follow user-specified instructions. The model's weights are adjusted based on the gradients derived from the task-specific loss, which measures the difference between the LLM's predictions and the ground truth labels.

## How does supervised fine-tuning work?

**Step 1**: Collect demonstration/user-labelled data, and train a supervised policy.
**Step 2**: Collect comparison data, and train a reward model.
**Step 3**: 

