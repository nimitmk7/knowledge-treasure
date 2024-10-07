## Cause
The primary cause of exposure bias is the use of teacher forcing during training:
- **Training**: The model receives perfect, ground truth inputs at each step.
- **Inference**: The model must rely on its own potentially imperfect predictions as inputs.

![[Pasted image 20241001195715.png]]

## Issues
1. **Error Accumulation**: Mistakes early in the generation process can compound, leading to increasingly poor quality output as the sequence length grows
2. **Distribution Shift**: The model may struggle to handle the distribution of its own generated tokens, which can differ from the ground truth distribution it was trained on
3. **Reduced Robustness**: The model may become overly reliant on perfect inputs and struggle to recover from errors or handle unexpected inputs during inference.

## Solutions
1. **Scheduled Sampling**: Gradually introducing model-generated tokens during training.
2. **Reinforcement Learning**: Using techniques like REINFORCE to optimize for sequence-level metrics.
3. **Adversarial Training**: Employing generative adversarial networks (GANs) to align training and inference distributions[
4. **Multi-Range Reinforcing**: Amplifying and denoising reward signals to improve model robustness.

