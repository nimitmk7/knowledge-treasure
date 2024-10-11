## Definition
They encode text into vector representations that can be used for downstream classification tasks. 

An encoder takes a sequence of tokens and output their contextualized representations:
 $$h_1, . . . , h_n = \text{Encoder}(x_1, . . . , x_n)$$
We can then use $h_1, . . . , h_n$ for other tasks. 

## Training
• Use any supervised task: $y =f(h_1,...,h_n)$
• Use self-supervised learning: predict a word from its context


## Example
1. [[BERT]]
2. 