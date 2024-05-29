*Convolutional neural networks (CNNs)* emerged from the study of the brain’s visual cortex, and they have been used in computer image recognition since the 1980s. 

>[!question] Why not simply use a deep neural network with fully connected layers for image recognition tasks ? 
> It works fine for small images, but breaks down for larger images due to the huge number of parameters it requires. CNNs solve this problem using partially connected layers and weight sharing.

## Convolutional Layers

- The convolutional layer is the most important building block of a CNN.
- Neurons in the 1st convolutional layer are not connected to every single pixel in the input image, but only pixels in their receptive fields.
- In turn, each neuron in the 2nd convolutional layer is connected only to neurons located within a small rectangle in the 1st layer.

![[CNN_Receptive_fields.png]]
The architecture allows the network to concentrate on small low-level features in the first hidden layer, and then assemble them into large higher-level features in the next hidden layer, and so on.

This hierarchical structure is common in real-world images, which is one of the reasons why CNNs work so well for image recognition.
### Layer-to-layer connection
1. Input layer and output layer have same size
	- Outputs of Rows $i, i + f_h-1$, columns $j, j+f_w-1$ $\Rightarrow$  Input to neuron (row,column)$(i,j)$ 

2. Input layer much larger than input layer. 
	- To connect a large input layer to a small input layer, space out the receptive fields. Dramatically reduces the model's computational complexity.
	- The horizontal or vertical step size from one receptive field to the next is called the stride.

	- Outputs of rows $i\times s_h$ to $i\times s_h + f_h - 1$, columns $j\times s_w$ to $j\times s_w + f_w -1$, where $s_h$ and $s_w$ are vertical and horizontal strides. 

3. In order for a layer to have the same height and width as the previous layer, it is common to add zeros around the inputs, as shown in the diagram. This is called zero padding.
![[CNN_Strides.png]]

## Filters

A neuron’s weights can be represented as a small image the size of the receptive field. A layer full of neurons using the same filter outputs a feature map, which highlights the areas in an image that activate the filter the most.

But we won’t have to define the filters manually: instead, during training the convolutional layer will automatically learn the most useful filters for its task, and the layers above will learn to combine them into more complex patterns.

![[CNN_filters.png]]
## Stacking Multiple Feature Maps

