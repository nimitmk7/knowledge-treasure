## 1. Introduction
 Support Vector Machines (SVM) are a powerful class of supervised learning algorithms that have applications in classification and regression. In this document, we will explore the motivation, mathematics, handling non-linearly separable data, and the use of kernel methods in SVM.

## 2. Concept of Lagrange Multipliers

### 2.1 Premise
Consider an optimization problem where we want to maximize (or minimize) a function $f(x, y)$ subject to an equality constraint $g(x, y) = c$. The Lagrange multiplier technique introduces a new variable, $\lambda$ (the Lagrange multiplier), to the objective function. The augmented objective function is given by:
$$
\mathcal{L}(x, y, \lambda) = f(x, y) - \lambda[g(x, y) - c]
$$

The Lagrange multiplier, $\lambda$, acts as a scalar that helps incorporate the constraint into the optimization problem. The goal is to find values of $x$, $y$, and $\lambda$ that maximize (or minimize) $\mathcal{L}(x, y, \lambda)$.
### 2.2 Geometric Interpretation

The geometric basis of the Lagrange multiplier method can easily be explained and visualized when the functions are of two variables, so let us simplify our example to finding a maximum or minimum of a function $F(x, y)$ subject to a constraint of the form $G(x, y) = 0$. 

We start by trying to find the extreme values of $F(x, y)$ subject to a constraint of the form $G(x, y) = 0$. In other words, we seek the extreme values of $F(x, y)$ when the point $(x, y)$ is restricted to lie on the level curve $G(x, y) = 0$. 


Figure 1 shows this curve together with several level curves of $F(x, y) = c$, where c is a constant. To maximize $F(x, y)$ subject to $G(x, y)=0$ is to find the largest value of c such that the level curve, $F(x, y) = c$, intersects $G(x, y) = 0$. It appears from figure that this happens when these curves just touch each other,
that is, when they have a common tangent line. This means that the normal lines at the point $(x_0, y_0)$ where they touch are identical. So the gradient vectors are parallel. That is $$\nabla F(x_0,y_0)=-\lambda \nabla G (x_0,x_0)$$
For some scalar $\lambda$ which is the Lagrange multiplier. 
![Figure 1](xc.png)

### 2.3 Algebraic Formulation
The procedure based on the above equation is as follows:
$$dF= \frac{\partial F}{\partial x} dx + \frac{\partial F}{\partial y} dy=0 $$
$$dG= \frac{\partial G}{\partial x} dx + \frac{\partial G}{\partial y} dy=0$$

Multiplying the second equation with $\lambda$ and addition to the first yields:

$$(\frac{\partial F}{\partial x}+\lambda \frac{\partial G}{\partial x}dx)+(\frac{\partial F}{\partial y}+\lambda \frac{\partial G}{\partial y})dy=0 $$

As can be seen, the above two equations are components of the vector equation:
$$\nabla F - \lambda \nabla G = 0$$
Thus, the maximum and minimum values of F(x, y) subject to the constraint G(x, y) = 0 can be found by solving the following set of equations:

$$\frac{\partial F}{\partial x}+\lambda \frac{\partial G}{\partial x}=0$$
$$\frac{\partial F}{\partial y}+\lambda \frac{\partial G}{\partial y }=0$$
$$G(x,y)=0$$
This is a system of three equations in the three unknowns x, y, and $\lambda$, but it is not necessary to find explicit values for $\lambda$.

These set of equations can be obtained using Lagrange method. We define our Lagrange $\mathbb{L}$ as:
$$\mathbb{L}(x,y,z,\lambda)=f(x,y,z)+\lambda g(x,y,z)$$

Putting $\frac{\partial}{\partial x}\mathbb{L}=0$, $\frac{\partial}{\partial y}\mathbb{L}=0$, $\frac{\partial}{\partial z}\mathbb{L}=0$, $\frac{\partial}{\partial \lambda}\mathbb{L}=0$ gives us the same 4 equations.

If the function to be extremized F and the side condition G are function of three independent variables x, y, and z, the following system of equation is solved to obtain the minimum or maximum value of F:

$$\frac{\partial F}{\partial x}+\lambda \frac{\partial G}{\partial x}=0$$
$$\frac{\partial F}{\partial y}+\lambda \frac{\partial G}{\partial y}=0$$
$$\frac{\partial F}{\partial z}+\lambda \frac{\partial G}{\partial z}=0$$
$$G(x,y,z)=0$$
This is a system of four equations in the four unknowns x, y, z, and $\lambda$.

Putting $\frac{\partial}{\partial x}\mathbb{L}=0$, $\frac{\partial}{\partial y}\mathbb{L}=0$, $\frac{\partial}{\partial z}\mathbb{L}=0$, $\frac{\partial}{\partial \lambda}\mathbb{L}=0$ gives us the same 4 equations.

### 2.4 A Simple Example

Let's illustrate the concept of Lagrange multipliers with a simple example. Suppose we want to maximize the function $f(x, y) = x + y$  subject to the constraint $g(x, y) = x^2 + y^2 - 4 = 0$. Our objective is to find the maximum value of $f(x, y)$ while satisfying the constraint.

We can set up the Lagrangian as follows:

$$
\mathcal{L}(x, y, \lambda) = x + y - \lambda(x^2 + y^2 - 4)
$$

To find the maximum value of $f(x, y)$ subject to the constraint, we need to solve the following system of equations:

$$
\begin{cases}
\frac{\partial \mathcal{L}}{\partial x} = 1 - 2\lambda x = 0 \\
\frac{\partial \mathcal{L}}{\partial y} = 1 - 2\lambda y = 0 \\
g(x, y) = x^2 + y^2 - 4 = 0
\end{cases}
$$

From the first two equations, we can solve for $\lambda$:

$$
2\lambda x = 1 \implies \lambda = \frac{1}{2x}
$$

$$
2\lambda y = 1 \implies \lambda = \frac{1}{2y}
$$

Since both equations must hold, we have:

$$
\frac{1}{2x} = \frac{1}{2y}
$$

Solving for $x$ and $y$, we find that $x = y$. Substituting this into the constraint equation:

$$
x^2 + x^2 - 4 = 2x^2 - 4 = 0
$$

Solving for $x$, we get $$x = y = \pm \sqrt{2}.$$

Now, we can calculate the value of $\lambda$:

$$
\lambda = \frac{1}{2x} = \frac{1}{2\sqrt{2}}
$$

Finally, we can find the maximum value of $f(x, y)$ by plugging these values into the objective function:

$$
f(x, y) = x + y = \sqrt{2} + \sqrt{2} = 2\sqrt{2}
$$

So, the maximum value of $f(x, y)$ subject to the constraint is $2\sqrt{2}$ when $x = y = \pm \sqrt{2}$.

### 2.5 Solved Problem 

Suppose you are running a factory, producing some sort of widget that requires steel as a raw material. Your costs are predominantly human labor, which is \$20 per hour for your workers, and the steel itself, which runs for \$ 2,000 per ton. 
Suppose your revenue R is loosely modeled by the following equation:
$$r(h,s)=200h^{2/3}s^{1/3}$$
At the same time we have a budgetary constraint of \$20,000.

**Solution**:

In this problem, we have to maximize our revenue but this is a constraint problem as we have a budget constraint of \$2,000. This translates to:
$$20h +2,000s=20,000$$
Since we need to maximize a function $R(h,s)$, subject to a constraint, 20h+2,000s=20,000, we begin by writing the Lagrangian function for this setup:

$$\mathbb{L}(h,s,\lambda)=200h^{2/3}s^{1/3}-\lambda(20h+2,000s-20,000)$$

Let us set $\nabla \mathbb{L}= 0$ vector as per the Lagrange multiplier theory. First, we handle the partial derivative with respect to h.
$$\frac{\partial \mathbb{L}}{\partial h}=0$$
$$\frac{\partial}{\partial h}200h^{2/3}s^{1/3}-\lambda(20h+2,000s-20,000)=0$$
$$200 \frac{2}{3}h^{-1/3}s^{1/3} -20\lambda=0$$
Next, we handle the partial derivative with respect to s:
$$\frac{\partial \mathbb{L}}{\partial s}=0$$
$$\frac{\partial}{\partial s}200h^{2/3}s^{1/3}-\lambda(20h+2,000s-20,000)=0$$
$$200 \frac{2}{3}h^{2/3}s^{-2/3} -2,000\lambda=0$$
Now, let us set-up the partial derivative wrt $\lambda$ =0. This will always give us our constraint equation as shown below:
$$\frac{\partial \mathbb{L}}{\partial \lambda}=0$$
$$\frac{\partial}{\partial \lambda}200h^{2/3}s^{1/3}-\lambda(20h+2,000s-20,000)=0$$
$$-20h-2,000s+20,000=0$$
$$20h+2,000s=20,000$$

Now we have the equations:
$$200 \frac{2}{3}h^{-1/3}s^{1/3} =20\lambda$$
$$200 \frac{2}{3}h^{2/3}s^{-2/3} =2,000\lambda$$
$$20h+2,000s=20,000$$
Let us put u=$\frac{s}{h}$:
$$\frac{200}{3}u^{1/3}=20\lambda$$
$$\frac{100}{3}u^{-2/3}=2,000 \lambda$$
This implies:
$$u^{1/3}=\frac{3}{10}\lambda$$
$$u^{-2/3}=60\lambda$$
Dividing the equations:
$$u=\frac{1}{200} \implies h=200s$$
Putting $h=200s$ in our constraint equation we get:
$$s=\frac{10}{3}$$
$$h=\frac{2000}{3}$$

## 3. Motivation behind SVM

SVM is motivated by the idea of **finding a hyperplane that maximizes the margin between two classes**. The Margin is the maximum distance between the plane and data points of both classes. The margin represents the degree of confidence in classifying new data points. This concept is rooted in the desire for robust classification

Maximizing the margin distance provides some reinforcement so that future data points can be classified with more confidence. 

Once the hyperplane is determined, new data can  be classified by determining on which side of the hyperplane it falls. SVMs are particularly useful when the data has many features, and/or when there is a clear margin of separation in the data

## 4. Key Terminology

![Figure 2](SVm_key_terms.png)

### 4.1 The Hyperplane

A hyperplane is a decision boundary which separates given set of data points having different class labels.

The SVM classifier separates data points using a hyperplane with the maximum amount of margin and this special hyperplane is known as a maximum margin hyperplane. 

The general equation of a hyperplane in $\mathbb{R}^n$ is given by $w.x+b=0$, where $w \epsilon \mathbb{R}^n$ is a non-zero normal vector to the hyperplane and $b \epsilon \mathbb{R}$ is a scalar. 

A hypothesis of the form $x \rightarrow sign(w.x+b)$ labels all points falling on one side of the hyperplane $w.x+b=0$ as positive, and points falling on another side as negative.

For a better understanding of the following text, let us try to understand how we get this equation for our hyperplane and refer to Figure 3.

#### 4.1.1 Deriving the Hyperplane equation

Let us assume here that $H_0$ is the Hyperplane under consideration and let $w$ be a normal vector to it. For any point $x$ on $H_0$, its projection on $w$ will remain constant. So, we get:
$$ \frac{w.x}{||w||}=\mathbb{C} $$
Let us multiply by $||w||$ on both sides:
$$w.x=\mathbb{C}||w||$$
Put $\mathbb{C}.||w||=-b$. This gives us:
$$w.x+b=0$$
which is the equation of our hyperplane.

![Figure 3](SVM_2.png)

### 4.2 Support Vectors

These are the vectors that determine the hyperplane or we can say that these are the extreme data points in the data set which helps in defining the hyperplane.

In Figure 3, points lying on $H_1$ and $H_2$ represent the support vectors. These data points will define the separating line or hyperplane better by calculating margins. The support vectors lie parallel to and symmetrically away from the Hyperplane. 

So their equation becomes
$w.x+b=\pm k$

Where k is another constant value. Now, we can rescale $w$ and $b$ in order to obtain the general form of the equation:

$w.x+b= \pm 1$

### 4.3 The Margin

#### 4.3.1 Formulation
The geometric margin $\rho_h (x)$ of a linear classifier $h: x \rightarrow w.x+b$ at a point x is its euclidean distance to the hyperplane $w.x+b=0$.
$$\rho_h (x)=\frac{|w.x+b|}{||w||}$$

The geometric margin $\rho_h$ of a linear classifier for a sample S $=(x_1,...x_m)$ is the minimum geometric margin over the points in the sample, $$\rho_h = min_{i \epsilon [m]}(\rho_h (x))$$
which can be simplified as:

$$\rho_h = \min\left(\frac{\left|w \cdot x_+\! + b\right|}{\|w\|}, \frac{\left|w \cdot x_-\! + b\right|}{\|w\|}\right)
$$

Where:
- $x_+$ represents the nearest data point from class $y = 1$.
- $x_-$represents the nearest data point from class $y = -1$.

#### 4.3.2 Derivation

For the support vector $(x_+, y_+)$, the distance to the hyperplane is given by:

$$
\text{distance}_+ = \frac{|w \cdot x_+ + b|}{\|w\|}
$$

Since this support vector lies on the margin, we have:
$$
w \cdot x_+ + b = 1 \quad \text{(1)}
$$

Substituting equation (1) into the expression for $\text{distance}_+$:
$$
\text{distance}_+ = \frac{1}{\|w\|}
$$

For the support vector $(x_-, y_-)$, the distance to the hyperplane is given by:

$$
\text{distance}_- = \frac{|w \cdot x_- + b|}{\|w\|}
$$

Since this support vector also lies on the margin, we have:

$$
w \cdot x_- + b = -1 \quad \text{(2)}
$$

Substituting equation (2) into the expression for $\text{distance}_-$:

$$
\text{distance}_- = \frac{1}{\|w\|}
$$

Now that we have expressions for $\text{distance}_+$ and $\text{distance}_-$, we can calculate the margin:

$$
\gamma = \text{distance}_+ + \text{distance}_- = \frac{1}{\|w\|} + \frac{1}{\|w\|} = \frac{2}{\|w\|}
$$

This concludes the derivation of the margin in Support Vector Machines.

### 4.4 Putting it all together
The SVM solution is the separating hyperplane with the maximum geometric margin and is thus known as the maximum-margin hyperplane. In summary, our goal is to maximize the margin while correctly classifying all data points.

![Figure 4](SVM_3.png)

Consider a binary classification problem and a dataset $\{(x_i, y_i)\}_{i=1}^m$, where $x_i$ is the feature vector, and $y_i$ is the class label (1 or -1). \\
The dataset is perfectly linearly separable if there exists a separating hyperplane $H_i$ such that 

- all $x \in C_i$ lie on its positive side
- all $x \in C_j, j \neq i$ lie on its negative side

For such a linearly separable dataset, the separating hyperplane has the following property for all $i = 1, 2, ... , n$:

$$
b + \sum_{j=1}^{p} w_{j}x_{ij} > 0 \texttt{ if }  y_{i} = 1
$$

$$
b + \sum_{j=1}^{p} w_{j}x_{ij} < 0 \texttt{ if }  y_{i} = -1
$$

Where $\mathbf{w}$ is the weight vector, $\mathbf{b}$ is the bias term.

Equivalently, we can combine the two properties into 1:

$$
y_{i} \biggl(b + \sum_{j=1}^{p} w_{j}x_{ij}\biggr) > 0
$$

To classify any new sample $\mathbf{x}$, we can use the sign of

$$
z = w_{0} + \sum_{j=1}^{p} w_{j}x_{ij}
$$

The sign of $z$ classifies the sample into one of two classes whereas the magnitude of $z$ determines how confident the classifier is with its classification, i.e., larger $z$ means the sample lies further away from the hyperplane and that the classifier is more confident about its classification.

## 5. Intuition

In this section, for the sake of simplicity assume that the cases are linearly separable. In Figure 5, the points are linearly separable as shown in the figure. 


![Figure 5](SVM_4.png)

However, we can visualize that there is not one but several lines that can divide the points into separate categories. So, now the question before us is of all the possible demarcations as shown in figure 6, which line would be the best to divide them. This is where SVM comes into picture and helps us identify the best possible hyperplane (line in this case) to demarcate the categories.

![Figure 6](SVM_Options.png)

Here, we need to understand that the data presented in the diagrams is only training dataset and it is just a random sample from our data set and some points in our test cases might lie a bit differently. 

So, if we look at Fig 6 (B) and Fig 6(C), these lines does not seem to be the perfect ones as they are too close to the boundary and these lines might not be able to perfectly categorize the test cases. So, let us drop these 2 from our consideration. 

Now, let us compare Fig 6(A) and 6(D). We visually cannot determine which one would be the better choice.

The SVM suggests that the best possible hyperplane will be the one with the maximum geometric margin and is thus known as the maximum-margin hyperplane. In very simple words, we can understand that best possible line would be where the margin is maximum. This is because **points near the decision surface represent very uncertain classification decisions**.

We choose the classifier which maximizes the least confidence it has on its classification, i.e., we choose the hyperplane which maximizes the sum of distance from it least confident classification of each class. 

Mathematically formulated, it looks like: 

$$\rho = \underset{w,b: y_i(w.x_i+b) \ge 0}{max} \;\;\; \underset{i \epsilon [m]}{min} = {}\frac{y_i(w.x_i+b)}{||w||} = \underset{w,b}{max} \;\;\;\underset{i \epsilon [m]}{min}  {}{\frac{y_i(w.x_i+b)}{||w||}}$$

## 6. Math behind SVM

Let us consider an input space X s.t. it is a subset of $\mathbb{R}^N$ with $N \ge 1$ and with target space $\mathbb{Y} = \{-1,1\}$. Given a Hypothesis function set $\mathbb{H}$, the binary classifier learns from a data set provided.

### 6.1 Primal Optimization Case

$$\rho = \underset{w,b: y_i(w.x_i+b) \ge 0}{max} \;\;\; \underset{i \epsilon [m]}{min} = {}\frac{y_i(w.x_i+b)}{||w||} = \underset{w,b}{max} \;\;\;\underset{i \epsilon [m]}{min}  {}{\frac{y_i(w.x_i+b)}{||w||}}$$ 
The second equality results from the fact that for the maximizing pair (w, b), the minimum of $y_i.(w.x_i+b)$ is 1.

Figure 2 illustrates the maximum optimal of solution (w,b). In addition to this maximum margin hyperplane, the figure also illustrates marginal hyperplanes, which are hyperplanes parallel to the separating hyperplane and passing through the closest points on either side. 

Since, they are parallel to the separating hyperplane, they have the same normal vector w. Furthermore, since $|w.x+b| = 1$ for the closest points, the equations for the marginal hyperplanes are $w.x+b= \pm 1$
\newline

Since, maximizing $\frac{1}{||w||}$ is equivalent to minimizing $\frac{1}{2}||w||^2$, the pair (w,b) returned by the SVM in the separable case is the solution of the following convex optimisation problem:
$$\underset{w,b}{min}\;\;\;{\frac{||w||^2}{2}}$$
Subject to:
$$y_i(w.x_i+b \ge 1, \forall i \epsilon [m]$$
The objective function F: $w\rightarrow\frac{1}{||w||^2}$ is infinitely differentiable. Its gradient is given by $\nabla F(w)=w$ and its Hessian is the identity matrix whose Eigen values are strictly positive. Therefore, $\nabla ^2 F(w) \succ 0$ and F is strictly convex. 

### 6.2 Support Vectors (Hard Margin)
Since the constraints in our optimization problem as shown below are thus qualified. The objective function as well as the affine constraints are convex and differentiable. Thus, we can apply the conditions of KKT to the optimum.
$$\underset{w,b}{min}\;\;\;{\frac{||w||^2}{2}}$$
Subject to:
$$y_i(w.x_i+b) \ge 1, \forall i \epsilon [m]$$
The KKT conditions are obtained by setting the gradient of the Lagrangian with respect to the primal variables w and b to zero and by writing the complementarity conditions:
$$\nabla_w\mathbb{L}=w- \sum_{i=1}^m \alpha_i y_i x_i =0 \implies w= \sum_{i=1}^m \alpha_i y_i x_i$$
Let the above be equation (A).
$$\nabla_b\mathbb{L}=w- \sum_{i=1}^m \alpha_i y_i =0 \implies \sum_{i=1}^m \alpha_i y_i=0$$ 
Let the above be equation (B) and we will refer to these equations (A) and (B) later.
Also,
$$\forall i, \alpha_i[y_i.(w.x_i+b)-1]=0\implies \alpha_i =0 \vee y_i(w.x_i+b)=1 $$
### 6.3 Dual Optimization problem (Hard Margin)

To derive the dual form of the constraint optimization problem, we plug in the Lagrangian definition of $w$ in terms of dual variables as expressed in the Equation (A) and apply the constraint of Equation (B). This yields:

$$\mathbb{L}=\frac{1}{2}||\sum_{i=1}^m \alpha_i y_i x_i||^2-\sum_{i,j=1}^m \alpha_i \alpha_j y_i y_j (x_i.x_j)-\sum_{i=1}^m \alpha_i y_i b +\sum_{i=1}^m \alpha_i$$
which simplifies to:
$$\mathbb{L}=-\frac{1}{2}\sum_{i,j=1}^m \alpha_i \alpha_j y_i y_j (x_i.x_j) +\sum_{i=1}^m \alpha_i$$
which leads us to the dual optimization problem for SVM's in the separable case:
$$\underset{\alpha}{max}=-\frac{1}{2}\sum_{i,j=1}^m \alpha_i \alpha_j y_i y_j (x_i.x_j) +\sum_{i=1}^m \alpha_i$$
subject to conditions: 
$$\alpha_i \ge0 \wedge\sum_{i=1}^m \alpha_i y_i =0 \forall i \epsilon [m]	
$$

The solution $\alpha$ of the dual problem can be used to determine the hypothesis returned by the SVM:
$$h(x)=sgn(w.x+b)=sgn(\sum_{i=1}^m \alpha_i y_i (x_i.x)+b)$$
b can be obtained from any support vector $x_i$ lying on the marginal hyperplane as:
$$b=y_i-\sum_{i=1}^m \alpha_i y_i (x_j.x_i)$$
## 7. Soft Margin SVM

The above example was a special case where we are able to linearly separate the two categories. However, in most practical settings the training data is not linearly separable. In such cases, we make us of what is called soft margins. In simple words, we define a cost function that penalizes for every wrong classification made.

![Figure 7](SVM_soft_margin.png)

### 7.1 Primal optimisation problem (Soft Margin)
In cases where the categories are not linearly separable, it implies that for every hyperplane that exists, there exists $x_i \epsilon S$ such that:

$$y_i[w.x+b] < 1 $$
This means that we cannot simultaneously hold all the constrains for the given data set. So, we try to relax a bit by introducing slack variables. The slack variables, $\xi_i$ as shown in Figure 7 and the below equation, are commonly used in optimization problems to relax the constraints. 

With slack variables, our equation takes the following form for each $i \epsilon [m]$:

$$y_i(w.x+b)\ge1-\xi_i$$

A slack variable $\xi_i (\ge0)$  measures the distance by which the vector $x_i$ violates the desired inequality ($y_i(w.x+b)>=1$). 

Slack variables are  used in optimization to define relaxed versions of constraints. Each $x_i$ must be positioned on the correct side of the appropriate marginal hyperplane to ascertain that it is not considered an outlier.

As a consequence, a vector $x_i$ with $0<y_i.(w.x+b)<1$ is correctly classified but still it is labeled as an outlier, ie $\xi_i>0$

But now we are faced with a very serious question : In the case of non-separable variable, how should we then choose the hyperplane. We are faced with two conflicting objectives: we want to minimize the total amount of slack due to outliers and at the same time we seek a hyperplane with a large margin.

This leads us to a general optimisation problem in case of linearly-inseparable SVMs where the parameter $C \ge 0$ determines the trade-off between margin maximisation and the minimization of the slack penalty.
$$\underset{w,b,\xi_i}{min} \;\;\; \frac{1}{2} ||w||^2 +C \sum_{i=1}^m \xi_i^p $$
Subject to:
$$y_i. (w.x_i+b)>=1-\xi_i \wedge \xi_i \ge 0, i\epsilon [m]$$
The parameter C is typically determined by n-fold cross validation. The above optimisation problem obtained is a convex optimisation problem since the constraints are affine and the objective function is convex for any $p \ge 1$. 

There are many possible choices for p leading to more or less aggressive penalisation of the slack terms. The choices p = 1 and p = 2 lead to the most straightforward solutions and analyses. 

The loss functions associated with p = 1 and p = 2 are called the hinge loss and the quadratic hinge loss, respectively. 

Figure 8 shows the plots of these loss functions as well as that of the standard zero-one loss function. Both hinge losses are convex upper bounds on the zero-one loss, thus making them well suited for optimization. In what follows, the analysis is presented in the case of the hinge loss (p = 1), which is the most widely used loss function for SVMs.

![Figure 8](loss_fxns_diag.png)

### 7.2 Support Vectors (Soft Margin)

In this case as well, the support vectors are affine and thus qualified. The objective function as well as the affine constraints are convex and differentiable.Therefore, we can apply the KKT conditions at the optimum.

We introduce Lagrange variables $\alpha_i \ge 0, i \epsilon [m]$, associated to the first m constraints and $B_i \ge 0, i \epsilon [m]$ associated to the non-negativity constraints of the slack variables. We define the Lagrangian for all $w \epsilon \mathbb{R}^n, b\epsilon \mathbb{R}$ and $\xi, \alpha, \beta \epsilon \mathbb{R}_+^m$ as:

$$\mathbb{L}(w,b,\xi,\alpha,\beta)=\frac{1}{2}||w||^2+C \sum_{i=1}^m \xi_i-\sum_{i=1}^m \alpha_i(y_i.(w.x_i+b)-1+\xi_i)-\sum_{i=1}^m \beta_i \xi_i$$

The KKT conditions are obtained by setting the gradient of the Lagrangian with respect to the primal variables w, b, and $\xi$ is to zero and by writing the complementarity conditions:
$$\nabla_w \mathbb{L}= w-\sum_{i=1}^m \alpha_i y_i x_i = 0 \implies w=\sum_{i=1}^m \alpha_i y_i x_i$$
$$\nabla_b \mathbb{L}=-\sum_{i=1}^m \alpha_i y_i=0 \implies \sum_{i=1}^m \alpha_i y_i= 0$$
$$\nabla_{\xi_i}\mathbb{L}= C- \alpha_i -\beta_i=0 \implies \alpha_i + \beta_i = C$$
$$\forall i, \alpha_i(y_i.(w.x_i+b)-1+\xi_i)=0 \implies \alpha_i=0 \vee y_i.(w.x_i)+b=1-\xi_i $$
$$\forall i, \beta_i \xi_i=0 \implies \beta_i=0 \vee \xi_i=0$$
### 7.3 Dual Optimisation problem (Soft Margin)

Plugging in the values of $w_i$ and the constraint $\sum_{i=1}^m \alpha_i y_i= 0$, we get:
$$\mathbb{L}=\frac{1}{2}||\sum_{i=1}^m \alpha_i y_i x_i||^2 - \sum_{i,j=1}^m \alpha_i \alpha_j y_i y_j (x_i.x_j)+\sum_{i=1}^m\alpha_i$$

Since, $\frac{1}{2}||\sum_{i=1}^m \alpha_i y_i x_i||^2-\sum_{i,j=1}^m \alpha_i \alpha_j y_i y_j (x_i.x_j)= -\frac{1}{2}\sum_{i,j=1}^m \alpha_i \alpha_j y_i y_j (x_i.x_j)$, we get:
$$\mathbb{L}=\sum_{i=1}^m\alpha_i -\frac{1}{2}\sum_{i,j=1}^m \alpha_i \alpha_j y_i y_j (x_i.x_j)$$
Here, in addition to the condition $\alpha_i \ge0$, we must also impose $\beta_i \ge0$, which in turn implies that $\alpha_i <=C$ since $\alpha_i +\beta_i=C$. This leads to the following dual optimization problem for SVMs in the non-separable case, which only differs from that of the separable case by the constraint $\alpha_i \le C$:
$$\underset{\alpha}{max} \sum_{i=1}^m \alpha_i -\frac{1}{2}\alpha_i \alpha_j y_i y_j (x_i.x_j)$$
subject to:
$$0 \le \alpha_i \le C \wedge \sum_{i=1}^m \alpha_i y_i=0, i\epsilon[m]$$
The solution $\alpha$ of the dual problem can be used to determine the hypothesis returned by the SVM:
$$h(x)=sgn(w.x+b)=sgn(\sum_{i=1}^m \alpha_i y_i (x_i.x)+b)$$

$b$ can be obtained from any support vector $x_i$ lying on the marginal hyperplane as:
$$b=y_i-\sum_{i=1}^m \alpha_i y_i (x_j.x_i)$$
## 8. Kernels

### 8.1 Introduction
Kernel methods are widely used in machine learning. They are flexible techniques that can be used to extend algorithms such as SVMs to define non-linear decision boundaries. 

### 8.2 Premise
Kernels help us in mapping a lower dimensional data into a higher dimensional data which helps in achieving a solution. The main idea behind these methods is based on so-called kernels or kernel functions, which, under some technical conditions of symmetry and positive-definiteness, implicitly define an inner product in a high-dimensional space. 

### 8.3 Formulation and Geometric Interpretation
Replacing the original inner product in the input space with positive definite kernels immediately extends algorithms such as SVMs to a linear separation in that high-dimensional space, or, equivalently, to a non-linear separation in the input space.

In order to better comprehend the utility of Kernels, let us consider Figure 9. In this figure, we can tell that there is no single line that will be able to separate the 2 categories. So, here we will map the points in the figure  into a higher dimensional space using kernels and we will be able to separate the categories with the help of a 2-D hyperplane as shown in Figure 10 and 11.

![Figure 9: Non-linearly separable case](non_linear_separable.png)

![Figure 10: Projection into higher dimensions](projection_to_higher_dimension.png)

![Figure 11: Linear separation in higher dimensions](higher_dimension_2.png)

As shown in the part A of the figure, it is not possible to draw a linear boundary and separate the points into 2 different categories. However, one can use more complex functions to separate the two sets as in figure B. One way to define such a non-linear decision boundary is to use a non-linear mapping $\phi$ from the input space X to a higher-dimensional space $\mathbb{H}$, where linear separation is possible.

### 8.4 Importance
The dimension of $\mathbb{H}$ can truly be very large in practice. For example, in the case of document classification, one may wish to use as features sequences of three consecutive words, i.e., trigrams. Thus, with a vocabulary of just 100,000 words, the dimension of the feature space H reaches $10^{15}$.

The generalization ability of large-margin classification algorithms such as SVMs do not depend on the dimension of the feature space, but only on the margin $\rho$ and the number of training examples m. Thus, with a favorable margin $\rho$, such algorithms could succeed even in very high-dimensional space. However, determining the hyperplane solution requires multiple inner product computations in high-dimensional spaces, which can become be very costly. A solution to this problem is to use kernel methods, which are based on kernels or kernel functions.

The idea of non-linear mapping is a consequence of Cover's theorem which states that, A complex pattern-classification problem, cast in a high-dimensional space nonlinearly, is more likely to be linearly separable than in a low-dimensional space, provided that the space is not densely populated. Or in simple terms, given a set of training data that is not linearly separable, one can transform it into a training set that is linearly separable by mapping it into a possibly higher-dimensional space via some non-linear transformation.
### 8.5 Kernel Definition

A function K: $\mathbb{X}x\mathbb{X} \rightarrow \mathbb{R}$ is called a kernel over $\mathbb{X}$

The idea is to define a kernel $\mathbb{K}$ such that for any two points $x, x' \epsilon \mathbb{X}$ be equal to the inner product of vectors $\phi (x)$ and $\phi (y)$ such that:
$$\forall x,x' \epsilon \mathbb{X}, K(x,x')=<\phi (x), \phi (x')>$$for some mapping $\phi: \mathbb{X} \rightarrow \mathbb{H}$ to a hilbert space $\mathbb{H}$ called a feature space. 

Since an inner product is a measure of the similarity of two vectors, $K$ is often interpreted as a similarity measure between elements of the input space $\mathbb{X}$.

An important benefit of using kernels is that we do not need to explicitly calculate a mapping $\phi$. In fact the kernel K, can be arbitrarily chosen as long as the existence of $\phi$ is guaranteed, ie, K satisfies the Mercer's condition.

#### 8.5.1 Mercer’s Condition
Let X $\subset \mathbb{R}$ be a compact set and and let K: $\mathbb{X}x\mathbb{X}\rightarrow \mathbb{R} $ be a continuous and symmetric function. The K admits a uniformly convergent expansion form:
$$K(x,x')= \sum_{n=0}^{\infty} a_n \phi_n (x) \phi_n(x')$$
with $s_n > 0$ iff for any square integrable function c ($c \epsilon L_2 (\mathbb{X})$), the following condition holds:

$$ \int \int_{\mathbb{X}x\mathbb{X}}c(x) c(x') K(x,x')dxdx' \ge 0$$
This condition is important to guarantee the convexity of the optimization problem for algorithms such as SVMs, thereby ensuring convergence to a global minimum. 

A condition that is equivalent to Mercer’s condition under the assumptions of the theorem is that the kernel K be positive definite symmetric (PDS). This property is in fact more general since in particular it does not require any assumption about $\mathbb{X}$.

#### 8.5.2 Positive Definite Symmetric Kernels

A kernel $K: \mathbb{X}x \mathbb{X} \rightarrow \mathbb{R}$ is said to be a positive definite symmetric kernel if for any $x_1,x_{}2,.....,x_n 	\subseteq \mathbb{X}$, the matrix K= $[Kx_i,x_j)]_{i,j}\epsilon \mathbb{R}^{mxm}$ is symmetric positive semidefinite (SPSD).

K is SPSD if it is symmetric and either of the 2 equivalent conditions hold:
1) The Eigenvalues of K are non-negative
\newline
2) For any column vector $c=(c_1,c_2,....c_m) \epsilon \mathbb{R}^{mx1}$,
$$c^TKc=\sum\_{i,j=1}^m c_i c_j K(x_i,x_j)>=0$$

### 8.6 Kernel Examples
Examples of Kernels:
1) Polynomial Kernels: For any constant $c>0$, a polynomial kernel of degree k is defined over $\mathbb{R}^n$ by:
\newline
$$\forall x,x' \epsilon \mathbb{R}^n, K(x,x')=(x.x'+b)^d$$
Polynomial kernels map the input space to a higher-dimensional space of dimension.

2) Gaussian Kernels: For any constant $\sigma>0$, a gaussian kernel or radial basis function is the kernel defined over $\mathbb{R}^n$ by:
$$\forall x,x' \epsilon \mathbb{R}^n, K(x,x')=exp(\frac{-||x'-x||^2}{2\sigma^2})$$
Gaussian kernels can be proved to be the limiting case of Polynomial kernels when the number of dimensions tend to infinity.

3) Sigmoid Kernels: For any real constants $a,b \ge0$, a sigmoid kernel is the kernel K defined over $\mathbb{R}^n$ by:
$$\forall x,x' \epsilon \mathbb{R}^n, K(x,x')=tanh(a(x.x')+b)$$
Using sigmoid kernels with SVMs leads to an algorithm that is closely related to learning algorithms based on simple neural networks, which are also often defined via a sigmoid function. When a < 0 or b < 0, the kernel is not PDS and the corresponding neural network does not benefit from the convergence guarantees of convex optimization

In order to do kernel mapping, we use kernel trick which is a computational trick to represent PD Kernels as inner products.

Proposition: Any algorithm to process finite-dimensional vectors can be expressed only in terms of pair-wise inner products can be applied to potentially infinite-dimensional vectors in the feature space of a PD Kernel by replacing each inner product evaluation with a kernel evaluation. The proof of this is fairly simple as the kernel is itself an inner product in the feature space.

### 8.7 Kernel Hands-on example
In order to better understand the kernel trick, let us take the example of XOR gate:

![Figure 12: XOR Mapping](kernel_XOR_example.png)


 Let us consider a polynomial kernel K such that:
 $K_{i,j}=k(x_i,x_j)=(1+x_i^Tx_j)^2$
  \$$0.02in]
So a few sample elements in our kernel matrix will be like:
$$K_{1,1}=k(\begin{bmatrix}
-1\\
-1
\end{bmatrix},
\begin{bmatrix}
-1\\
-1
\end{bmatrix})=(1+2)^2=9$$

$$K_{1,2}=k(\begin{bmatrix}
-1\\
-1
\end{bmatrix},
\begin{bmatrix}
-1\\
+1
\end{bmatrix})=(1+0)^2=1$$


This gives us our K matrix as:
  \$$0.02in]

$$\begin{bmatrix}
9 & 1 &1 &1\\
1 & 9 &1 &1\\
1 & 1 &9 &1\\
1 & 1 &1 &9\\

\end{bmatrix}$$
Our polynomial kernel $K=(x^Tx'+1)^2=(x_1x_1'+x_2x_2'+1)^2$
which is equal to:
$$x_1^2x_1'^2+x_2^2x_2'^2+2x_1x_1'x_2x_2'+2x_1x_1'+2x_2x_2'+1$$
If we define our $\phi(x)$ as:
$$\phi(x)=[x_1^2,x_2^2,\sqrt{2}x_1x_2,\sqrt{2}x_1,\sqrt{2}x_2,1]^T$$
Then, we have $$k(x,x')=\phi^T(x) \phi(x')$$

Let us calculate $\phi(x)$ for our data points:
$$\phi(x_1)=\phi\begin{bmatrix}
-1\\
-1
\end{bmatrix}=\begin{bmatrix}
1 & 1 & \sqrt{2} & -\sqrt{2} & -\sqrt{2} &1
\end{bmatrix}^T$$


$$\phi(x_1)=\phi\begin{bmatrix}
-1\\
+1
\end{bmatrix}=\begin{bmatrix}
1 & 1 & -\sqrt{2} & -\sqrt{2} & \sqrt{2} &1
\end{bmatrix}^T$$
Therefore, we have:
$$K_{11}=\phi^T(x_1) \phi(x_1)=1+1+2+2+2+1=9$$
$$K_{12}=\phi^T(x_2) \phi(x_1)=1+1-2+2-2+1=1$$

So, using inner products we are able to achieve the same matrix K, but much more computationally efficiently.

## 9. SVM Hands-on example
### 9.1 Data Points

**Negative samples:**
-  $x_1 : (1,2)$
-  $x_2 : (-1,2)$

**Positive sample**:
- $x_3 : (-1,-2)$

![{Screenshot 2023-10-11 at 3.03.39 AM.png}

###  9.2 Solve the constrained optimization problem
#### 9.2.1 Define SVM Lagrangian(Primal Problem)

$$\mathcal{L}(\mathbf{w}, b, \alpha)=\frac{1}{2}\|\mathbf{w}\|_2^2-\sum_{i=1}^n \alpha_i\left(y_i\left(\mathbf{x}_i^{T} \mathbf{w}+b\right)-1\right)$$

$$\mathcal{L}(\mathrm{w}, b, \alpha)=\frac{1}{2} \mathrm{w}^T \mathrm{w}-\sum_{i=1}^n \alpha_i y_i \mathrm{x}_i^{T} \mathrm{w}+b\left(\sum_{i=1}^n \alpha_i y_i\right)+\sum_{i=1}^n \alpha_i$$

$$ \begin{aligned} & =\frac{1}{2}\left[\begin{array}{ll}w_1 & w_2\end{array}\right]\left[\begin{array}{l}w_1 \\ w_2\end{array}\right]\\ &
+\alpha_1\left[\begin{array}{ll}1 & 2\end{array}\right]\left[\begin{array}{l}w_1 \\ w_2\end{array}\right]+\alpha_2\left[\begin{array}{ll}-1 & 2\end{array}\right]\left[\begin{array}{l}w_1 \\ w_2\end{array}\right]-\alpha_3\left[\begin{array}{ll}-1 & -2\end{array}\right]\left[\begin{array}{l}w_1 \\ w_2\end{array}\right] \\ 
& +b\left(-\alpha_1-\alpha_2+\alpha_3\right) \\ & 
+\alpha_1+\alpha_2+\alpha_3\end{aligned}$$

$$ \begin{aligned} & = \frac{1}{2}\left(w_1^2+w_2^2\right)+\alpha_1\left(w_1+w_2\right)+\alpha_2\left(-w_1+2 w_2\right) -\alpha_3\left(-w_1-2 w_2\right)\\ & 
+b\left(-\alpha_1-\alpha_2+\alpha_3\right) +\alpha_1+\alpha_2+\alpha_3\end{aligned} $$

#### 9.2.2 Compute the partial derivatives of Lagrangian w.r.t. its primal variables
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
#### 9.2.3 Define the dual problem to solve

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

#### 9.2.4 Compute the partial derivative of the dual w.r.t. all alphas and set them to 0

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


#### 9.2.5 Finding b

The bias $b^*$ can be computed from any support vector:

$$\begin{aligned} &b^*=y_3-x_3^{\top} w^*\\
& =1-\left[\begin{array}{ll}-1 & -2\end{array}\right]\left[\begin{array}{c}0 \\ -1 / 2\end{array}\right] \\ & =1-1 \\ b^* & =0\end{aligned}$$

## 10. Conclusion
Support Vector Machines are a powerful class of machine learning algorithms. They find an optimal hyperplane to maximize the margin between classes. The mathematics behind SVM involves formulating the primal and dual problems, and SVM's versatility in handling non-linearly separable data is achieved through the use of kernel functions.


## 11. References

### 11.1 Books
1. Support Vector Machines by Nello Cristianini and John Shawe-Taylor.  
	This comprehensive book provides an in-depth understanding of Support Vector  Machines and their applications.  

2. Learning with Kernels: Support Vector Machines, Regularization, Optimization, and  Beyond by Bernhard Sch ̈olkopf and Alexander J. Smola.  
	An authoritative text that covers both theoretical foundations and practical aspects  of Support Vector Machines.  
	
3. Introduction to Machine Learning with Python: A Guide for Data Scientists by  Andreas C. M ̈uller and Sarah Guido.  
	A beginner-friendly book with a chapter on Support Vector Machines, including  practical examples in Python.  

### 11.2 Websites  
1. SVMlight: A Support Vector Machine Tutorial by Thorsten Joachims.  
	This tutorial offers a detailed introduction to SVMs, their implementation, and  usage.  
2. Scikit-Learn SVM Documentation by Scikit-Learn.  
	The official documentation for Scikit-Learn’s SVM implementation, which is a valuable resource for practical SVM applications in Python.  
	
3. SVM Tutorial by S.V.N. Vishwanathan.  
	A website with a series of tutorials on Support Vector Machines, covering both theory and implementation.  
4. Interactive demo of Support Vector Machines (SVM) by Jonas Greitemann.  
	This interactive tool allows demonstration of how SVM works, including the working  of a few common kernels.  
	
### 11.3 Beginner Materials  
1. Support Vector Machines (SVM) Tutorial (YouTube) by Simplilearn.  
	An introductory video tutorial explaining the basic concepts of SVMs.  
2. SVM Classification with Python by DataCamp.  
	A beginner-friendly tutorial on SVM classification using Scikit-Learn.  
3. Understanding Support Vector Machine(SVM) by Analytics Vidhya.  
	An easy-to-follow blog post explaining SVMs with code examples.


