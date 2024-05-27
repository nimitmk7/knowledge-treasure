- We are given a training set of examples, e.g. dog images. ![[Pasted image 20240522170247.png]]
- We want to learn a probability distribution $p(x)$ over images $x$ such that:
	- **Generation**: If we sample $x_{new} \sim p(x)$, $x_{new}$  should look like a dog.
	- **Density estimation**: $p(x)$ should be high if $x$ looks like a dog, and low otherwise(anomaly detection)
	- **Unsupervised Representation Learning**: We should be able to learn the structure of the data. For our case, it is what these images have in common, e.g. ears, tail, etc. (*features*)
- How to represent $p(x)$? 

## Modelling joint distribution

### Example: Single Pixel
Modeling a single pixel’s color. Three discrete random variables: 
- Red Channel $R$ : Val(R) = {0, · · · , 255}
- Green Channel $G$ : Val(G) = {0, · · · , 255}
- Blue Channel $B$ : Val(B) = {0, · · · , 255}
![[Pasted image 20240522171313.png]]

Sampling from the joint distribution $(r,g,b) \sim p(R,G,B)$ randomly generates a color for the pixel. How many parameters do we need to specify the join distribution $P(R=r, G=g, B=b)?$
$$256*256*256 - 1$$
