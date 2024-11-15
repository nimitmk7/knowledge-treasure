## Core Idea : Detection as Classification

We can think about the detection problem as a **classification problem of _all possible portions_ (windows/masks) of the input image** since an object can be located at any position and scale in the image.

So, we need to Try out different image regions, run a classifier on each one, it will solve our variable size input problem. 

What about window size of those regions? Try them all.

**Problem**: Need to test many positions and scales on all image regions, while using a computationally demanding classifier(CNN). 
**Solution**: Only look at a tiny subset of possible portions of the image

## Region Proposals
- Find image regions that are more likely to contain objects
- "Class agnostic" object detector
- Look for "blob-like" regions

### Selective Search
- Merge adjacent pixels with similar color and texture to form connected regions, and keep merging up these regions. 
- Convert these regions into a box by just drawing boxes around it.
- Doing these over multiple scales, you end up with a lot of boxes around all the blobby stuff in the image.
- Reasonably fast to compute, cuts down search space by a lot.

![[Pasted image 20240313151014.png]]



## R-CNN(Region Based CNN)
- RCNN can accommodate a number of efficient algorithms that can produce region proposals.
- The baseline RCNN employs **Selective Search via Hierarchical Grouping**.
![[Pasted image 20240314081807.png]]
### Graph-based Segmentation

We perform segmentation in the image using an [efficient graph-based algorithm](http://cs.brown.edu/people/pfelzens/segment/) to obtain the set $R=\{r_1, \dots, r_n \}$ of initial regions. The segmentation algorithm starts by formulating the image as a graph. 

Let G = (V, E) be an undirected graph with vertices $v_i \in V$ , the set of elements to be segmented, and edges $(v_i, v_j) ∈ E$ corresponding to pairs of neighboring vertices. Each edge has a corresponding weight $w((v_i, v_j ))$, which is a non-negative measure of the _dissimilarity_ between neighboring elements $v_i$ and $v_j$. In the case of image segmentation, the elements in V are _pixels_ and the weight of an edge is some measure of the dissimilarity between the two pixels connected by that edge (e.g., the difference in intensity, color, motion, location or some other local attribute).

In the graph-based approach, a segmentation $S$ is a partition of $V$ into components such that each component (or region) $C ∈ S$ corresponds to a connected component in a graph $G' = (V, E')$, where $E' ⊆ E$. In other words, any segmentation is induced by a subset of the edges in $E$. There are different ways to measure the quality of a segmentation but in general we want the elements in a component to be similar, and elements in different components to be dissimilar. This means that edges between two vertices in the same component should have relatively low weights, and edges
between vertices in different components should have higher weights.

For example, a pixel $p_i$ corresponds to a vertex $v_i$ and it has 8 neighboring pixels. We can define the weight to be the absolute value of the dissimilarity between the pixel intensity $I(p_i)$ and its neighbors. 

$$w(v_i, v_j ) = |I(p_i) − I(p_j )|$$

Before we compute the weights we use the 2D Gaussian kernel / filter we met in the introductory section to CNNs  - the end result being a smoothing of the image that helps with noise reduction. For color images we run the algorithm for each of the three channels. 

There is one runtime parameter for the algorithm, which is the value of $k$ that is used to compute the threshold function $\tau$ . Recall we use the function $τ(C) =k/|C|$ where $|C|$ is the number of elements in C. Thus k effectively sets a scale of observation, in that a larger k causes a preference for larger components. 

The graph algorithm can also accommodate dissimilarity weights between neighbors at a _feature space_ rather at _pixel level_ based on a Euclidean distance metric with other distance functions possible. 

![Graph Based Segmentation Example](images/graph-based-segmentation1.png) 
*Graph representation of the segmentation problem*

Notice that the initial segments may be many and do not necessarily accurately represent distinct objects as shown below:

![Result of the initial segments produced by the graph-based algorithm, k=300](images/graph-based-segmentation2.png) 


### Grouping 

After the initial regions are produced, we use a greedy algorithm to iteratively group regions together. This is what gives the _hierarchical_ in the name of the algorithm. 

First the similarities between all neighboring regions are calculated. The two most similar regions are grouped together, and new similarities are calculated between the resulting region and its neighbors. The process of grouping the most similar regions is repeated until the whole image becomes a single region.  

For the similarity $s(r_i ,r_j)$ between region $r_i$ and $r_j$ we apply a variety of complementary measures under the constraint that they are
fast to compute. In effect, this means that the similarities should be based on features that can be propagated through the hierarchy, i.e.
when merging region $r_i$ and $r_j$ into $r_t$, the features of region $r_t$ need to be calculated from the features of $r_i$ and $r_j$ without accessing the image pixels. Such feature similarities include color, texture, size, fill - we create a binary sum of such individual similarities. 

$$ s(r_i ,r_j) = a_1 s_{color}(r_i ,r_j)+a_2 s_{texture}(r_i ,r_j) + a_3 s_{size}(r_i ,r_j) + a_4 s_{fill}(r_i ,r_j) $$

where $a_i ∈ {0,1}$ denotes if the similarity measure is used or not.

The end result of the grouping is a hierarchy of regions ranked according to creation time. As earlier created regions may end up being the largest some permutation in the ranking is applied.  

![hierarchical-grouping-example](images/hierarchical-grouping-example.png)
*Example of hierarchical grouping*

The above selective search strategy is diversified (1) by using a variety of colour spaces with different invariance properties, (2) by using different similarity measures $s(r_i, r_j)$, and (3) by varying our initial regions. Each strategy results in a different hierarchy and after a permutation that randomizes the ranking the final list of regions is produced. For RCNN we use 2000 such region proposals. 

![region-proposals](images/region-proposals.png)
*Final proposals - in reality we have 2000 of such regions.*

### CNN features and SVM Classification
### Architecture
Refer to [[RCNN_architecture.png]].
## Visual Overview
![[Pasted image 20240314090242.png]]
## Training

RCNN training is complex as involves a multi-stage pipeline. 

1. R-CNN first fine pretrains a CNN on a classification task without using any bounding box ground truth. 
2. It then adapts the trained CNN by replacing the classification layer with another randomly initialized layer of span K+1, where K are the number of classes in the domain of interest (+1 is due to the fact that we consider the background). We continue training with SGD using (a) as inputs the warped images as determined by the region proposals (b) using this time the ground truth bounding boxes and declaring each region proposal with IoU > 0.5 relative to the ground truth bounding box as true positive. 
3. Then, it fits SVMs to CNN features. These SVMs act as object detectors, replacing the softmax classifier learnt by fine-tuning.

### Problems
1. Slow at test-time: Need to run full forward pass of CNN for each region proposal.
2. SVM and regressors are post-hoc: CNN features not updated in response to SVM and regressors.
3. The selective search algorithm is a fixed algorithm. Therefore, no learning is happening at that stage. This could lead to the generation of bad candidate region proposals
4. Complex multi-stage training pipeline
## Fast RCNN

### Difference from RCNN
- Swap the order of extracting regions and running the CNN. 
- **Sol'n to problem 1 of RCNN**: Sharing computation of convolutional layers between proposals for an image.
- **Sol'n to problem 2, 3 of RCNN**: Just train the whole system end-to-end all at once.

**Critical concept**: ROI pooling
### Architecture
![[Pasted image 20240314184457.png]]
Refer to: [[Fast_RCNN.png]]

A Fast RCNN network takes as input an entire image and a set of proposals $R$. The set of proposals is produced by the selective search algorithm used in RCNN and its similarly around 2000 per image. 

1. The network first **processes the whole image with a CNN** having several convolutional (experiments were done for 5-13 layers) and 5 max pooling layers to produce a feature map.
2. The selective search algorithm **produces region proposals** for the image.
3. _For each proposal_, a **region of interest (RoI) pooling layer** (see below) extracts a fixed-length feature vector from the feature map. 

>[!INFO] Difference from RCNN
> This is in contrast to the RCNN that fed the different proposals to the CNN. Now we only have one feature map and we select regions of interest from that.  

4.  Each feature vector is fed into a sequence of **fully connected (FC) layers** that finally branch into two sibling output layers: 
	1. It **produces soft-max probability estimates** over K object classes plus a catch-all “background” class. 
	2. It ==outputs four real-valued numbers for each of the K object classes==. Each set of 4 values encodes refined bounding-box positions for one of the K classes.
5. Output from both the layers are passed into the NMS algorithm, to produce the final bbox.

The key element in the architecture is the **RoI pooling layer**. 

#### RoI(Region of Interest) Pooling Layer
An RoI is a rectangular window into a feature map. Each RoI is **defined by a four-tuple (r, c, h, w)** that specifies its top-left corner (r, c) and its height and width (h, w).

The RoI pooling layer uses max pooling to convert the features inside any valid region of interest into a small feature map with a fixed spatial extent of H × W (e.g. 7 × 7), where H and W are layer hyper-parameters that are independent of any particular RoI. 

RoI max pooling works by dividing the h × w RoI window into an H × W grid of sub-windows of approximate size h/H × w/W and then max-pooling the values in each sub-window into the corresponding output grid cell. Pooling is applied independently to each feature map channel, as in standard max pooling. 

#### Simplified Training
 As we have replaced the SVM,  the training can happen end to end. We can starting from a pre-trained CNN and use a multi-task (classification, regression) loss function. 
 
### Results

![[Pasted image 20240313153134.png]]
>[!WARNING] Test time speeds don't include region proposals.
### Drawback
Region proposal is the bottleneck for Fast RCNN, and as it takes 2 seconds, so it can’t be used for real-time systems.

![[Pasted image 20240313153358.png]]
## Faster RCNN

Instead of using a slow selective search algorithm for region proposal, we insert a **RPN**(Region Proposal Network) after the last convolutional layer of our feature map generating CNN.

RPN is a CNN trained to produce region proposals directly from the feature map, so we now don't need for external methods for regional proposals.
### Architecture

![[Pasted image 20240314191915.png]]
Refer to: [[Faster_RCNN.png]]

#### RPN(Region Proposal Network)
- RPN includes both regression and classification.
- We slide a window of size $n \times n$ over the feature map. At each sliding window location, we simultaneously predict multiple image proposals, where the no. of maximum possible proposals for each location is denoted as $k$.
- So the regression layer has $4k$ outputs encoding the coordinates of $k$ boxes, and the classification layer outputs $2k$ scores that represent the probability of the presence of an object or not an object for each proposal.
- The $k$ proposals are parameterized relative to $k$ reference boxes, which we call **anchor boxes**. 
##### Anchor Boxes

Their size can be changed but the original paper used anchor size of (128 × 128, 256 × 256, 512 × 512) and three aspect ratios (1:1, 1:2 and 2:1). An anchor is centered at the sliding window in question, and is associated with a scale and aspect ratio. 

By default we use 3 scales and 3 aspect ratios, yielding k = 9 anchors at each sliding position. For a feature map of a size $W × H$ (typically ∼2,400), there are $W \times H \times k$ anchors in total.

The RPN network produces a classification score i.e. how confident we are that there is an object for each of the anchor boxes as well as the regression on the anchor box coordinates.

###### Intuition
The anchor boxes are just references, they are selected to have different aspect ratios and scales in order to accommodate different types of objects, elongated objects like buses, for example, cannot be properly represented by a square bounding box.

Each regressor in the RPN only computes 4 offset values (w, h, x, y) to the corresponding reference anchor box.where w = width, h = height, (x,y) = center.

Thus, 
**Regression** = offset of anchor box from Ground truth
**Classification** = probability that each anchor shows an object

#### Training
To the train the RPN, we need anchor-level training data. But we only have class/object level training data.

Given a IoU threshold(e.g. $t_p = 0.75, t_n = 0.3$), for an anchor:
$IoU > t_p$ is positive anchor
$IoU < t_n$ is negative anchor
and $t_n \le IoU \le t_p$ is neutral anchor

We train the network end to end using a loss function that is per anchor and can be expressed as:

$$L(p, \hat p, y, \hat y) =  L_{\text{localization}} + \lambda L_{\text{classification}}$$
where
$$
L_{localization} =  \sum y_k L_{Huber}
$$
and 
$$
L_{classification} = \sum_k CE(\hat y_k, y_k)\space
$$
where
1. $\hat y_k$ is the probability that anchor contains object
2. We have $y_k$ term in the localization loss, so **only positive anchors are penalizing the regression loss.**
3. **Huber Loss** is a popular loss function used in machine learning for regression tasks, particularly when dealing with outliers in the data. It combines the properties of both quadratic loss (squared error) and absolute loss (absolute error) to provide a more robust solution. The key feature of Huber Loss is its ability to transition smoothly between quadratic and absolute loss functions, controlled by a parameter that needs to be selected carefully.

## References
1. Refer to this video(it already starts from the required timestamp)
<iframe width="560" height="315" src="https://www.youtube.com/embed/GxZrEKZfW2o?si=dCzPJjAxiAjq_L2K&amp;start=1520" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
2. https://pantelis.github.io/artificial-intelligence/aiml-common/lectures/scene-understanding/object-detection/faster-rcnn-object-detection/index.html
3. https://kikaben.com/faster-r-cnn-rpn/
4. 
