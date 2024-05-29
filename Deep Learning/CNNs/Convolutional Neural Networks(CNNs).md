## Convolutional Layer

In the convolutional layer, the first operation is to convolve a 3D image with a 3D filter, piece by piece.

![[CNN_layer.png]]


When we convolve the filter over the full image, we get a activation map($28\times28\times1$  dimensional in above example). 

We'll actually apply a full set of filters in the convolution layer. So for $n$ filters, we get $n$ activation maps.

![[CNN_activation_maps.png]]

- **ConvNet** is a sequence of Convolutional Layers, interspersed with activation functions.

### Hyperparameters
#### Stride
Stride refers to ==the number of pixels by which the filter moves across the input image==.

#### Padding
- Padding is ==the process of adding extra pixels(with 0 values mostly) to the input image before applying convolution==

### Output layer size
- If input is of size $W_1\times H_1\times D_1$, and there are $K$ channels in the layer, P = amount of padding, F = filter kernel size, S = stride
$W_2$ = $\lfloor \frac{W_1+2*P-F} {S} + 1 \rfloor$
$H_2$ = $\lfloor \frac{H_1+2*P-F} {S} + 1 \rfloor$
$D_2$ = $K$

### Parameters 
- With parameter sharing, we have $F\cdot F \cdot D_1$ weights per filter, and we have $K$ filters in the layer, so we have  $(F\cdot F \cdot D_1) \cdot K$ weights and $K$ biases.

## Pooling Layer

### Motivation
Pooling was introduced to reduce redundancy of representation and reduce the number of parameters, recognizing that ==precise location is not important for object detection.==

### Function
The pooling function is a form of non-linear function that further modifies the result of the ReLU result. The pooling function accepts as input pixel values surrounding (a rectangular region) a feature map location (i,j) and returns one of the following:
- Max value
- Weighted average
- L2 Norm

A pooling layer typically works on every input channel independently, so the output depth is the same as the input depth. You may alternatively pool over the depth dimension, in which case the image’s spatial dimensions (height and width) remain unchanged, but the number of channels is reduced.

### Example
![[Max_Pool.png]]

### Downfall
Despite receiving ample treatment in Ian Goodfellows’ book, pooling has fallen out of favor. Some reasons are:

- Datasets are so big that we’re more concerned about under-fitting.
- Dropout is a much better regularizer.
- Pooling results in a loss of information - think about the max-pooling operation as an example shown in the figure below.
- [All convolutional networks](https://arxiv.org/pdf/1412.6806.pdf) where the pooling is replaced by a CNN with larger stride can do better.
## Fully Connected(FC) layer
- Contains neurons that connect to the entire input volume, as in ordinary Neural Networks. Also known as a dense layer, it will compute the class scores.
## Preview of what's happening
![[CNN_Overview.png]]
## High-level View

![[CNN_example.png]]

## Why are CNNs preferred over other nets for image classification or segmentation?

Mainly because of these 3 distinct properties 

1. Translational invariance(because of shared parameters, max pooling)
2. Hierarchical feature learning(local receptive fields)
3. Parameter sharing(using the same kernel for the entire image)

**Translation Invariance**: CNNs detect features like edges or shapes regardless of their position in the image. If a feature is learned at one location, the CNN can recognize it anywhere, thanks to applying the same filters across the entire image.

**Hierarchical Feature Learning**: Starting from simple patterns (edges) in early layers to complex objects in deeper layers, CNNs learn features in a layered manner. This approach allows the network to understand images in a progressively sophisticated way.

**Parameter Sharing**: A single set of parameters (weights and biases) is used across different parts of an image. This not only reduces the model's complexity but also helps in detecting the same feature throughout the image, making the CNN efficient and less prone to overfitting

## CNN architectures
1. [[ResNet]]
2. [[Object Detection using R-CNN]]
3. 

## References
1. https://x.com/drummatick/status/1762009052601806872?s=46
2. 