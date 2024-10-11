Generating the target sentence by taking $argmax$ on each step of the decoder. This is **greedy decoding** as it takes the most probable word on each step and feeds it to the next step.

**Problem**: Greedy decoding has no way to undo decisions. So due to lack of backtracking, output can be poor.

**How to fix this?** 

1. **Exhaustive Search Decoding**
	1. Try computing all possible sequences, which means on every step $t$ of the decoder, we’re tracking $V^t$ possible partial translations, where $V$ is vocab size. 
	2. $O(V^t)$ is far too expensive.
2. Beam Search Decoding

[[Beam Search Decoding]]