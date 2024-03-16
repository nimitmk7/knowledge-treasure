
Take a pre-trained CNN for classification and locate a single object centered in the image, then slide the CNN across the image and make predictions at each step. 

The CNN was generally trained to predict not only class probabilities and a bounding box, but also an objectness score: this is the estimated probability that the image does indeed contain an object centered near the middle. 
	- This is a binary classification output; it can be produced by a dense output layer with a single unit, using the sigmoid activation function and trained using the binary cross-entropy loss.
![[Pasted image 20240223150911.png]]
### Non-max Suppression


#### Motivation
Object detection using a sliding CNN approach often detects the same object multiple times, at slightly different positions. Some post-processing is needed to get rid of all the unnecessary bounding boxes.

#### Procedure
1. Get rid of all bounding boxes for which the object-ness score is below some threshold.
2. Find the remaining bounding box with the highest object-ness score, and get rid of all the other remaining bounding box that overlap a lot with it.(e.g. IoU > 60%)
3. Repeat Step 2 until there are no more bounding boxes to get rid of.

### Disadvantages
1. Requires running the CNN many times(due to the sliding part), so it is quite slow. 


## References

1. Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow, 3rd Edition [Aurélien Géron](https://learning.oreilly.com/search/?query=author%3A%22Aur%C3%A9lien%20G%C3%A9ron%22&sort=relevance&highlight=true)
2. 