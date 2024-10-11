Used for classification.

$$
L_{CE} = - \sum_{i=1}^nt_{i}\log(p_{i}), \space\space \text{for n classes}
$$

where $t_i$ is the truth label and $p_i$ is the softmax probability for the $i^{th}$ class.

> [!INFO] Intuition
>  $-\log{0} = \infty$ and $-\log(1) = 0$.  So the CE loss penalizes the model for having lower probability for the class of the truth label. 

## Practical Considerations

In PyTorch, the in-built CrossEntropyLoss function expects raw(unnormalized) logits, and not probabilities. 

As stated in the [`torch.nn.CrossEntropyLoss()`](https://pytorch.org/docs/stable/generated/torch.nn.CrossEntropyLoss.html#torch.nn.CrossEntropyLoss) doc:

> This criterion combines [`nn.LogSoftmax()`](https://pytorch.org/docs/stable/generated/torch.nn.LogSoftmax.html) and [`nn.NLLLoss()`](https://pytorch.org/docs/stable/generated/torch.nn.NLLLoss.html) in one single class.

Therefore, you should **not** use softmax before passing the output to CrossEntropyLoss in PyTorch.

For details
[[https://pytorch.org/docs/stable/generated/torch.nn.CrossEntropyLoss.html]]
