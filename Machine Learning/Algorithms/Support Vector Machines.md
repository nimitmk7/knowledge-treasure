# Support Vector Machines
## Core Idea

To find a hyperplane in a N-dimensional space(N = no of features) that distinctly classifies the data points ==between 2 classes==. This concept is rooted in the desire for robust classification.

Our objective is to find a plane that has the maximum margin, i.e the maximum distance between the plane and data points of both classes. Maximizing the margin distance provides some reinforcement so that future data points can be classified with more confidence.

Therefore, ==Margin = degree of confidence in classifying new data points.==

A 2-D example:
![[SVM_1 1.png]]

## The Hyperplane

A hyperplane is a decision boundary which separates given set of data points having different class labels.

The SVM classifier separates data points using a hyperplane with the maximum amount of margin and this special hyperplane is known as a maximum margin hyperplane. 

The general equation of a hyperplane in $\mathbb{R}^n$ is given by $w.x+b=0$, where $w \epsilon \mathbb{R}^n$ is a non-zero normal vector to the hyperplane and $b \epsilon \mathbb{R}$ is a scalar. 

A hypothesis of the form $x \rightarrow sign(w.x+b)$ labels all points falling on one side of the hyperplane $w.x+b=0$ as positive, and points falling on another side as negative.

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
not be able to perfectly categorize the test cases. (Remember that these are just training points)

Now, we can't visually determine which is better from A and D.

**SVM's suggestion**: The best possible hyperplane will be the one with the maximum geometric margin and is thus known as the maximum-margin hyperplane. **But why ?** 

Points near the decision surface represent very uncertain classification decisions. So we want to have a hyperplane with max possible distance from the nearest boundary point in both the classes.

So, 

$$\rho = \underset{w,b: y_i(w.x_i+b) \ge 0}{max} \;\;\; \underset{i \epsilon [m]}{min} = {}\frac{y_i(w.x_i+b)}{||w||} = \underset{w,b}{max} \;\;\;\underset{i \epsilon [m]}{min}  {}{\frac{y_i(w.x_i+b)}{||w||}}$$


## Hands-on example
### Data Points

**Negative samples:**
-  $x_1 : (1,2)$
-  $x_2 : (-1,2)$

**Positive sample**:
- $x_3 : (-1,-2)$

\begin{figure}[H]
\centering
\includegraphics[keepaspectratio=true,scale=0.3]{Screenshot 2023-10-11 at 3.03.39 AM.png}
\end{figure}


### Solve the constrained optimization problem
#### Define SVM Lagrangian(Primal Problem)

$$\mathcal{L}(\mathbf{w}, b, \alpha)=\frac{1}{2}\|\mathbf{w}\|_2^2-\sum_{i=1}^n \alpha_i\left(y_i\left(\mathbf{x}_i^{T} \mathbf{w}+b\right)-1\right)$$

$$\mathcal{L}(\mathrm{w}, b, \alpha)=\frac{1}{2} \mathrm{w}^T \mathrm{w}-\sum_{i=1}^n \alpha_i y_i \mathrm{x}_i^{T} \mathrm{w}+b\left(\sum_{i=1}^n \alpha_i y_i\right)+\sum_{i=1}^n \alpha_i$$

$$ \begin{aligned} & =\frac{1}{2}\left[\begin{array}{ll}w_1 & w_2\end{array}\right]\left[\begin{array}{l}w_1 \\ w_2\end{array}\right]\\ &
+\alpha_1\left[\begin{array}{ll}1 & 2\end{array}\right]\left[\begin{array}{l}w_1 \\ w_2\end{array}\right]+\alpha_2\left[\begin{array}{ll}-1 & 2\end{array}\right]\left[\begin{array}{l}w_1 \\ w_2\end{array}\right]-\alpha_3\left[\begin{array}{ll}-1 & -2\end{array}\right]\left[\begin{array}{l}w_1 \\ w_2\end{array}\right] \\ 
& +b\left(-\alpha_1-\alpha_2+\alpha_3\right) \\ & 
+\alpha_1+\alpha_2+\alpha_3\end{aligned}$$

$$ \begin{aligned} & = \frac{1}{2}\left(w_1^2+w_2^2\right)+\alpha_1\left(w_1+w_2\right)+\alpha_2\left(-w_1+2 w_2\right) -\alpha_3\left(-w_1-2 w_2\right)\\ & 
+b\left(-\alpha_1-\alpha_2+\alpha_3\right) +\alpha_1+\alpha_2+\alpha_3\end{aligned} $$

#### Compute the partial derivatives of Lagrangian w.r.t. its primal variables
$$ \tag{a}
\frac{\partial \mathcal{L}}{\partial \mathrm{w}}=\mathbf{0}_d \rightarrow \mathrm{w}^{\star}=\sum_{i=1}^n \alpha_i y_i \mathbf{x}_i
$$

$$ \tag{b}
\frac{\partial \mathcal{L}}{\partial b} =0 \rightarrow \sum_{i=1}^n y_i \alpha_i
$$ 

$$ \tag{c}
\begin{aligned}w^*=\sum_{i=1}^3 \alpha_i y_i x_i
=-\alpha_1\left[\begin{array}{l}1 \\ 2\end{array}\right]-\alpha_2\left[\begin{array}{c}-1 \\ 2\end{array}\right]+\alpha_3\left[\begin{array}{c}-1 \\ -2\end{array}\right] 
\end{aligned}
$$ 

$$ \tag{d}
-\alpha_1-\alpha_2+\alpha_3=0
$$ 

\subsubsection{Define the Dual problem to solve}


$$
\underset{\alpha_i \geq 0, \forall i}{\operatorname{maximize}} \sum_{i=1}^n \alpha_i-\frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y_i y_j \mathbf{x}_i^T \mathbf{x}_j$$

$$\text { subject to }  \sum_{i=1}^n \alpha_i y_i=0$$
$$ 0 \leq \alpha_i \leq C, \quad \forall i $$
Dot Products:
$$
\begin{array}{ll}
x_1^{T} x_2=-1+4=3 & x_1^{T} x_1=5 \\
x_1^{T} x_3=-1-4=-5 & x_2^{T} x_2=5 \\
x_2^{T} x_3=1-4=-3 & x_3^{T} x_3=5
\end{array}
$$
$$
\begin{aligned}
& \max _{\alpha_1, \alpha_2, \alpha_3} \mathcal{L}\left(w^*, b^*, \alpha\right) \\
& \begin{aligned} 
\Rightarrow \max _{\alpha_1, \alpha_2, \alpha_3} & \alpha_1+\alpha_2+\alpha_3  -\frac{1}{2}\left(5 \alpha_1^2+3 \alpha_1 \alpha_2+5 \alpha_1 \alpha_3+3 \alpha_1 \alpha_2+5 \alpha_2^2+3 \alpha_2 \alpha_3 +5 \alpha_1 \alpha_3+3 \alpha_2 \alpha_3+5 \alpha_3^2\right)
\end{aligned} \\
\\
& \text { Using (d)... } \alpha_3=\alpha_1+\alpha_2
\\&
\Rightarrow \max _{\alpha_1, \alpha_2} 2 \alpha_1+ 2 \alpha_2 - \left(20 \alpha_1^2+32 \alpha_1 \alpha_2+16 \alpha_2{ }^2\right)
\end{aligned}
$$

#### Compute the partial derivative of the dual w.r.t. all $\alpha$ and set them to 0

$$ \tag{i}
 \frac{\partial \mathcal{L}}{\partial \alpha_1}=0 \Rightarrow-20 \alpha_1-16 \alpha_2+2=0 
$$
$$ \tag{ii}
 \frac{\partial \mathcal{L}}{\partial \alpha_2}=0 \Rightarrow-16 \alpha_1-16 \alpha_2+2=0 
$$
$$ \tag{iii}
 \alpha_3=\alpha_1+\alpha_2
$$


Using (i), (ii) \& (iii)
$$
\begin{aligned}
& \alpha_1=0 \quad \alpha_2=\frac{1}{8} \quad \alpha_3=\frac{1}{8} \\
& \\
& \text{Calculating $\omega^*$}\\
& \begin{aligned}
& \omega^*=\sum_{i=1}^3 \alpha_i y_i x_i \\
& \omega^*=-\frac{1}{8}\left[\begin{array}{c}
-1 \\
2
\end{array}\right]+\frac{1}{8}\left[\begin{array}{c}
-1 \\
-2
\end{array}\right] \\
&=\left[\begin{array}{c}
0 \\
-1 / 2
\end{array}\right] 
& \\
& \text { Margin Width }=\frac{2}{\left\|w^*\right\|}=2 \times 2=4
\end{aligned}
\end{aligned}
$$


#### Finding $b^*$

The bias $b^*$ can be computed from any support vector:

$$\begin{aligned} &b^*=y_3-x_3^{\top} w^*\\
& =1-\left[\begin{array}{ll}-1 & -2\end{array}\right]\left[\begin{array}{c}0 \\ -1 / 2\end{array}\right] \\ & =1-1 \\ b^* & =0\end{aligned}$$




## Kernel Functions


Watch this video:
<iframe width="560" height="315" src="https://www.youtube.com/embed/OKFMZQyDROI?si=nyqxncoCGmHK1TjP" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

