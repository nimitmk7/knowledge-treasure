Normalizing flows attempt to map the data into a simpler distribution, such as a Gaussian Distribution. 

The key difference is that normalizing flows place a constraint on the form of mapping function, so that it is invertible and can therefore be used to generate new data points. 

## Analogy: Jacob and the F.L.O.W Machine

Upon visiting a small village, you notice a mysterious-looking shop with a sign above the door that says JACOB’S. Intrigued, you cautiously enter and ask the old man standing behind the counter what he sells.

He replies that he offers a service for digitizing paintings, with a difference. After a brief moment rummaging around the back of the shop, he brings out a silver box, embossed with the letters F.L.O.W. He tells you that this stands for Finding Likenesses Of Watercolors, which approximately describes what the machine does. You decide to give the machine a try.

You come back the next day and hand the shopkeeper a set of your favorite paintings, and he passes them through the machine. The F.L.O.W. machine begins to hum and whistle and after a while outputs a set of numbers that appear randomly generated. The shopkeeper hands you the list and begins to walk to the till to calculate how much you owe him for the digitization process and the F.L.O.W. box. Distinctly unimpressed, you ask the shopkeeper what you should do with this long list of numbers, and how you can get your favorite paintings back.

The shopkeeper rolls his eyes, as if the answer should be obvious. He walks back to the machine and passes in the long list of numbers, this time from the opposite side. You hear the machine whir again and wait, puzzled, until finally your original paintings drop out from where they entered.

Relieved to finally have your paintings back, you decide that it might be best to just store them in the attic instead. However, before you have a chance to leave, the shop‐keeper ushers you across to a different corner of the shop, where a giant bell hangs from the rafters. He hits the bell curve with a huge stick, sending vibrations around the store.

Instantly, the F.L.O.W. machine under your arm begins to hiss and whirr in reverse, as if a new set of numbers had just been passed in. After a few moments, more beautiful watercolor paintings begin to fall out of the F.L.O.W. machine, but they are not the same as the ones you originally digitized. They resemble the style and form of your original set of paintings, but each one is completely unique!

You ask the shopkeeper how this incredible device works. He explains that the magic lies in the fact that he has developed a special process that ensures the transformation is extremely fast and simple to calculate while still being sophisticated enough to convert the vibrations produced by the bell into the complex patterns and shapes present in the paintings.

Realizing the potential of this contraption, you hurriedly pay for the device and exit the store, happy that you now have a way to generate new paintings in your favorite style, simply by visiting the shop, chiming the bell, and waiting for your F.L.O.W. machine to work its magic!

This is a depiction of a normalizing flow model. 

## Normalizing Flows
In a normalizing flow model, the decoding function is designed to be the exact inverse of the encoding function and quick to calculate, giving normalizing flows the property of tractability(ability to be easily controlled).

>[!help] **How is VAE different from Normalizing flows in this regard ?** 
> In contrast to Normalizing flows, in a VAE, the encoder and decoder are two completely distinct neural networks. 

However, neural networks are not by default invertible functions. Then how can we create an invertible process that converts between a complex distribution and a much simpler distribution while still making use of the flexibility and power of deep learning ?

The answer is **Change of Variables**. 

## Change of Variables 

### Equation

$$p_X(x) = p_Z(z) \left|det \left(\frac{\partial z}{\partial x}\right) \right|$$

How does this help in building a generative model ?

The key is understanding that if $p_{Z}(z)$ is a simple distribution from which we can easily sample (e.g., a Gaussian), then in theory, all we need to do is find an
1. appropriate invertible function $f(x)$ that can map from the data $X$ into $Z$, 
2. corresponding inverse function $g(z)$ that can be used to sample $z$ back to a point $x$ in an original domain.

We can use the preceding equation involving the Jacobian determinant to find an exact, tractable formula for the data distribution $p(x)$ .

#### Issues
1. Calculating the determinant of a high-dimensional matrix is computationally extremely expensive — specifically it is $O(n^3)$. This is completely impractical to implement in practice, as even small 32 × 32–pixel grayscale images have 1,024 dimensions.
2. It is not immediately obvious how we should go about calculating the invertible function $f(x)$ . We could use a neural network to find some function $f(x)$ but we cannot necessarily invert this network—neural networks only work in one direction!

The answer is RealNVP.

## RealNVP

It is a neural network that can transform a complex data distribution into a simple Gaussian, while also possessing the desired properties of 
1. being invertible
2. having a Jacobian that can be easily calculated

### Coupling Layers

A coupling layer produces a scale and translation factor for each element of its input. In other words, it produces two tensors that are ==exactly the same size as the input==, ==one for the scale factor== and ==one for the translation factor==. 
![[Pasted image 20240622170149.png]]

#### Passing data through a coupling layer

![[Pasted image 20240622170905.png]]
- First $d$ dimensions of the data are fed through to the first coupling layer. 
- The remaining $D-d$ dimensions are completely masked.

- The outputs from the layer are the scale and translation factors. These are again masked, but this time with the inverse mask to previously, so that only the second halves are let through.

$$z_{1:d} = x_{{1:d}}$$
$$z_{d+1:D} = x_{d+1:D} \space\odot \exp(s(x_{1:d})+t(x_{1:d})))$$
You may be wondering why we go to the trouble of building a layer that masks so much information. The answer is clear if we investigate the structure of the Jacobian matrix of this function:

$$\frac{\partial z}{\partial x} = 
\begin{bmatrix} \\
 \mathbf{I} & 0\\
 \frac{\partial z_{d+1:D}}{\partial x_{1:d}} & diag(\exp[s(x_{1:d})])\\
\end{bmatrix}$$

The top-left d × d submatrix is simply the identity matrix, because $z_{1:d} = x_{1:d}$. These elements are passed straight through without being updated. 

The top-right submatrix is therefore 0, because $z_{1:d}$ is not dependent on $x_{d + 1:D}$. 

The bottom-left submatrix is complex, and we do not seek to simplify this. 

The bottom-right submatrix is simply a diagonal matrix, filled with elements of $\exp(s(x_{1:d}))$, because $z_{d+1:D}$ is linearly dependent on $x_{d+1:D}$ and the gradient is dependent only on the scaling factor.

![[Pasted image 20240622184248.png]]

- The benefit of having a lower triangular matrix is that its determinant is just equal to product of the diagonal elements. ==The determinant is not dependent on any of the complex derivatives in the bottom-left submatrix!==

$$\det(J) = \exp\left[ \sum_{j}s(x_{1:d})_{j} \right]$$


This is easily computable, which was one of the two original goals of building a normalizing flow model.

The other goal was that the function must be easily invertible. We can see that this is true as we can write down the invertible function just by rearranging the forward equations, as follows:

$$x_{1:d} = z_{1:d}$$
$$x_{d+1:D} = (z_{d+1:D} - t(x_{1:d}))\odot \exp(-s(x_{1:d})) $$

![[Pasted image 20240622185019.png]]























