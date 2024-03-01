## What ?
Autoencoder is an unsupervised artificial neural network that learns how to efficiently compress and encode data then learns how to reconstruct the data back **from** the reduced encoded representation **to** a representation that is as close to the original input as possible.

==Autoencoder, by design, reduces data dimensions by learning how to ignore the noise in the data.==

An auto-encoder learns two functions: 
1. An encoding function that transforms the input data
2. A decoding function that recreates the input data from the encoded representation.

## More Detail

1. Autoencoders are **data-specific**, which means that they will only be able to compress data similar to what they have been trained on.
2. Autoencoders are **lossy**, which means that the decompressed outputs will be degraded compared to the original inputs (similar to MP3 or JPEG compression). This differs from lossless arithmetic compression.
3.  Autoencoders are **learned automatically** from data examples, which is a useful property: it means that it is easy to train specialized instances of the algorithm that will perform well on a specific type of input. It doesn't require any new engineering, just appropriate training data

## Example

![[Autoencoder_example.png.png]]


## Components

### Encoder
### Bottleneck

### Decoder
### Reconstruction Loss



## Use Cases

### Anomaly Detection

### Image Denoising

### Dimensionality Reduction


## Code
https://github.com/nimitmk7/knowledge-treasure/blob/main/Deep%20Learning/Autoencoders/autoencoders_in_keras.ipynb

## References
1. https://towardsdatascience.com/auto-encoder-what-is-it-and-what-is-it-used-for-part-1-3e5c6f017726
2. 