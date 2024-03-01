# Support Vector Machines
## Core Idea

To find a hyperplane in a N-dimensional space(N = no of features) that distinctly classifies the data points ==between 2 classes==. This concept is rooted in the desire for robust classification.

Our objective is to find a plane that has the maximum margin, i.e the maximum distance between data points of both classes. Maximizing the margin distance provides some reinforcement so that future data points can be classified with more confidence.

Therefore, ==Margin = degree of confidence in classifying new data points.==

A 2-D example:
![[SVM_1 1.png]]

## The Hyperplane

A hyperplane is a decision boundary which separates given set of data points having different class labels.

The SVM classifier separates data points using a hyperplane with the maximum amount of margin and this special hyperplane is known as a maximum margin hyperplane. 

The general equation of a hyperplane in $\mathbb{R}^n$ is given by $w.x+b=0$, where $w \epsilon \mathbb{R}^n$ is a non-zero normal vector to the hyperplane and $b \epsilon \mathbb{R}$ is a scalar. 

A hypothesis of the form $x \rightarrow sign(w.x+b)$ labels all points falling on one side of the hyperplane $w.x+b=0$ and negatively all others.

### Deriving the equation

Let us assume here that $H_0$ is the Hyperplane under consideration and let $w$ be a normal vector to it. For any point $x$ on $H_0$, its projection on $w$ will remain constant. So, we get:
$$ \frac{w.x}{||w||}=\mathbb{C} $$
Let us multiply by $||w||$ on both sides:
$$w.x=\mathbb{C}||w||$$
Put $\mathbb{C}.||w||=-b$. This gives us:
$$w.x+b=0$$
which is the equation of our hyperplane.
![[SVM_2.png]]

## Support Vectors

These are the vectors that determine the hyperplane or we can say that these are the extreme data points in the data set which helps in defining the hyperplane.

In above figure, $H_1$ and $H_2$ represent the support vectors. These data points will define the separating line or hyperplane better by calculating margins. The support vectors lie parallel to and symmetrically away from the Hyperplane. 

So their equation becomes
$w.x+b=\pm k$
Where k is another constant value. Now, we can rescale $w$ and $b$ in order to obtain the general form of the equation:
$w.x+b= \pm 1$

## The Margin

Margin = Distance between Hyperplane and the nearest data points from each class(positive/negative)

It therefore represents the separation between classes, and is the most fundamental concept of SVM.

The geometric margin $\rho_h (x)$ of a linear classifier $h: x \rightarrow w.x+b$ at a point x is its euclidean distance to the hyperplane $w.x+b=0$.
$$\rho_h (x)=\frac{|w.x+b|}{||w||}$$

The geometric margin $\rho_h$ of a linear classifier for a sample S $=(x_1,...x_m)$ is the minimum geometric margin over the points in the sample, $$\rho_h = min_{i \epsilon [m]}(\rho_h (x))$$
 that is the distance of the hyperplane defining h to the closest sample points. 

The SVM solution is the separating hyperplane with the maximum geometric margin and is thus known as the maximum-margin hyperplane.

![[SVM_3.png]]
## Intuition

For simplicity's sake, Let's assume that the cases are linearly separable. Example:
![[SVM_4.png]]
However there are several lines that can divide these 2 points into separate categories.

![[SVM_Options.png]]

**Question**: Which is the best demarcation ?

**Answer**: . So, if we look at (B) and (C), these lines does not seem to be the perfect ones as they are too close to the boundary and these lines might  
not be able to perfectly categorise the test cases. (Remember that these are just training points)

Now, we can't visually determine which is better from A and D.

**SVM's suggestion**: The best possible hyperplane will be the one with the maximum geometric margin and is thus known as the maximum-margin hyperplane. **But why ?** 

Points near the decision surface represent very uncertain classification decisions. So we want to have a hyperplane with max possible distance from the nearest boundary point in both the classes.

So, 

$$\rho = \underset{w,b: y_i(w.x_i+b) \ge 0}{max} \;\;\; \underset{i \epsilon [m]}{min} = {}\frac{y_i(w.x_i+b)}{||w||} = \underset{w,b}{max} \;\;\;\underset{i \epsilon [m]}{min}  {}{\frac{y_i(w.x_i+b)}{||w||}}$$




