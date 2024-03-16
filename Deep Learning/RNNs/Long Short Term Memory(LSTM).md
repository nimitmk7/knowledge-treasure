## Motivation
1. Avoid the exploding/vanishing gradient problem of vanilla RNNs.

## Core Idea
- Instead of using the same feedback loop connection for events that happened long ago, and events that happen a few intervals ago, to make a prediction about the next sequence,  LSTM uses 2 separate paths to make predictions:
  1. For long term memory
  2. For short term memory

## Concepts

### Long-term memory

### Short-term memory



The first stage in a LSTM unit determines what % of the Long-Term memory is remembered, and it is usually called the **Forget gate**.

The second stage in a LSTM unit : One block creates a potential long term memory from the short-term memory and the input,  and other block determines the % of the potential memory to add to the long term memory. It is usually called the **Input gate**.

The third stage in a LSTM unit: Updating the short-term memory with the help of the new long term memory. Called the **output gate**.

The new short-term memory is also the output of the LSTM unit.



