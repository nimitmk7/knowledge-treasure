[LoRA (Low-Rank Adaptation of Large Language Models)](https://hf.co/papers/2106.09685) is a popular and lightweight training technique that ==significantly reduces the number of trainable parameters==. 

It works by ==inserting a smaller number of new weights into the model and only these are trained==. 

This makes training with LoRA much faster, memory-efficient, and produces smaller model weights (a few hundred MBs), which are easier to store and share. 

LoRA can also be combined with other training techniques like DreamBooth to speedup training.
