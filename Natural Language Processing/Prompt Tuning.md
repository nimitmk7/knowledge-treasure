Prompt-tuning is an efficient, low-cost way of adapting an AI foundation model to new downstream tasks without retraining the model and updating its weights.

[Prompt tuning](https://hf.co/papers/2104.08691) adds task-specific prompts to the input, and these prompt parameters are updated independently of the pretrained model parameters which are frozen.

## Hard Prompts vs Soft prompts

- Hard prompts are manually handcrafted text prompts with discrete input tokens; the downside is that it ==requires a lot of effort to create a good prompt==

- Soft prompts are ==learnable tensors concatenated with the input embeddings that can be optimized to a dataset==; the downside is that ==they aren’t human readable== because you aren’t matching these “virtual tokens” to the embeddings of a real word.

## References
1. https://huggingface.co/docs/peft/en/conceptual_guides/prompting#soft-prompts
2. 