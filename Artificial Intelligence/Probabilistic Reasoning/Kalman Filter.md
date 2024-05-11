## Introduction
If the variables are normally distributed and transitions are linear, the Bayes Filter is equal to the Kalman Filter.
## Gaussian choice
The commitment to represent the posterior by a Gaussian has important ramifications. Most importantly, Gaussians are unimodal, that is, **they posses a single maximum**. Such a posterior is characteristic of many tracking problems in robotics, in which the **posterior is focused around the true state with a small margin of uncertainty**.
## History
The Kalman filter was invented in the 1950s by Rudolph Emil Kalman, as a 
technique for filtering and prediction in linear systems.
## Formulation
It implements belief computation for ==continuous states==. It is not applicable to discrete or hybrid state spaces. It represents beliefs by the moments representation. 

At time $t$, the belief is represented by the mean $\mu_t$ and the covariance $\Sigma_t$. Posteriors are Gaussian if the following 3 properties hold(in addition to the Markov assumptions of the Bayes Filter):
### Assumptions
1. The next state probability $p(x_{t}|u_{t}, x_{t-1})$ must be a linear function in its arguments with added Gaussian Noise. $$x_{t} = A_{t}x_{t-1} + B_{t}u_{t} + \epsilon_{t} $$
Here $x_t$ and $x_{t-1}$ _are state vectors, and $u_t$  is the control vector at time_ $t$. In our
notation, both of these vectors are vertical vectors, that is, they are of the form:

$$ x_{t} = \begin{bmatrix} \space x_{1,t} \\ x_{2,t} \\ \dots \\ x_{n,t}\end{bmatrix} \space \textbf {and} \space \space u_{t} = \begin{bmatrix} \space u_{1,t} \\ u_{2,t} \\ \dots \\ u_{m,t}\end{bmatrix} $$
$A_t$ and $B_t$ are matrices. $A_t$ is a square matrix of size  $n\times n$ , where  $n$ _is the_
dimension of the state vector  $x_t$. $B_t$ _is of size_ $n \times m$, with $m$ being the dimension of the control vector $u_t$. 

 By multiplying the state and control vector with the matrices $A_t$  and $B_t$, respectively, the state transition function becomes linear in its arguments. 
 Thus, Kalman filters assume linear system dynamics. 
 
 The random variable $\epsilon_t$ is a Gaussian random vector that models the randomness in the state transition. It is of the same dimension as the state vector. Its mean is zero and its covariance will be denoted $R_t$. 

The above state transition probability equation is called _**a linear Gaussian**_ to reflect the fact that it is linear in its arguments with additive Gaussian Noise. It defines the state transition probability $p(x_t | u_{t}, x_{t-1})$ which can be obtained by plugging the equation into multi-variate normal distribution definition.

Mean of posterior state: $A_{t}x_{t-1} + B_{t}u_{t}$ and covariance by $R_t$:
$$ \begin{flalign}
&p(x_t | u_{t}, x_{t-1}) \\
&= \det(2\pi R_{t})^{-\frac{1}{2}} \exp\left\{ -\frac{1}{2} (x_{t} - A_{t}x_{t-1} - B_{t})^T R_{t}^{-1} (x_{t} - A_{t}x_{t-1} - B_{t}) \right\}
\end{flalign}$$
>[!TIP] This is a representation of the Gaussian noise, which can be obtained by isolating it in the 1st equation. This is because probability is about modelling the uncertainty, which is Gaussian noise in this case. 

2. The measurement probability $p(z_{t}|x_{t})$ must also be **linear** in its arguments, with added Gaussian Noise: $$z_{t} = C_{t}x_{t} + \delta_{t}$$
Here $C_t$ _is a matrix of size_ $k \times n$, where  $k$  is the dimension of the measurement vector $z_t$. The vector $t$ _describes the measurement noise. The distribution of_  $t$
is a multivariate Gaussian with zero mean and covariance $Q_t$. The measurement
probability is thus given by the following multivariate normal distribution:

$$ \begin{flalign}
&p(z_t | \space x_{t}) \\
&= \det(2\pi Q_{t})^{-\frac{1}{2}} \exp\left\{ -\frac{1}{2} (z_{t} - C_{t}x_{t})^T Q_{t}^{-1} (z_{t} - C_{t}x_{t}) \right\}
\end{flalign}$$

3. Finally, the initial belief $bel(x_0)$ must be normal distributed. We will denote the _mean of this belief by_ $\mu_0$  and the covariance by $\Sigma_{0}$: 
$$bel(x_{0}) = p(x_{0}) = \det(2\pi\Sigma_{0})^{-\frac{1}{2}} \exp \left\{  -\frac{1}{2} (x_{0} - \mu_{0})^T \Sigma^{-1} (x_{0} - \mu_{0}) \right\}$$
These 3 assumptions are sufficient to ensure that the posterior $bel(x_t)$ is always a gaussian, for any point in time $t$. 

## The Algorithm

![[Pasted image 20240405104402.png]]
Kalman filters represent the belief $bel(x_t)$ _at time_ $t$ by the mean $\mu_{t}$ _and the covariance_ $\Sigma_t$. The input of the Kalman filter is the belief at time $t-1$, represented by $\mu_{t-1}$  and $\Sigma_{t-1}$.

To update these parameters, Kalman filters require the control  $u_t$ and the measurement $z_t$. The output is the belief at time $t$, represented by $\mu_t$ _and_  $\Sigma_t$.

In Lines 2 and 3, the predicted belief $\bar \mu$ and $\bar\Sigma$ is calculated representing the belief $bel(x_t)$ _one time step later, but before incorporating the measurement_ $z_t$. 

This belief is obtained by incorporating the control $u_t$. The mean is updated using the deterministic version of the state transition function, with the mean $\mu_{t-1}$  substituted for the state $x_{t-1}$. 

The update of the covariance considers the fact that states depend on
previous states through the linear matrix $A_t$. This matrix is multiplied twice into the covariance, since the covariance is a quadratic matrix.

_The belief_ $\bar {bel}(x_t)$ is subsequently transformed into the desired belief $bel(x_t)$ in Lines 4 through 6, by incorporating the measurement $z_t$. 

- The variable $K_t$, computed in Line 4 is called Kalman gain. It specifies the degree to which the measurement is incorporated into the new state estimate. 
- Line 5 manipulates the mean, by adjusting it in proportion to the Kalman gain $K_{t}$ and the deviation of the actual measurement, $z_t$, and the measurement predicted according to the measurement probability.
- Finally, the new covariance of the posterior belief is calculated in Line 6, adjusting for the information gain resulting from the measurement.

## Hands-on
```Python
from filterpy.kalman import KalmanFilter
from filterpy.common import Q_discrete_white_noise

# Create the filter object with dimensions of state and measurement vectors.
f = KalmanFilter(dim_x=2, dim_z=1)

# Assign the initial value for the state (position and velocity).
f.x = np.array([[2.],    # position
                [0.]])   # velocity

# Define the state transition matrix
f.F = np.array([[1.,1.],
                [0.,1.]])

# Define the measurement function:
f.H = np.array([[1.,0.]])

# Define the covariance matrix
f.P = np.array([[1000.,    0.],
                [   0., 1000.] ])

# Assign the measurement noise
f.R = np.array([[5.]])

# Finally, I will assign the process noise.
f.Q = Q_discrete_white_noise(dim=2, dt=0.1, var=0.13)

'''
Now just perform the standard predict/update loop while some_condition_is_true:
'''

while True:
    z, R = read_sensor()
    x, P = predict(x, P, F, Q)
    x, P = update(x, P, z, R, H)
    
```


Full Documentation:
https://filterpy.readthedocs.io/en/latest/kalman/KalmanFilter.html

## References
1. [Probabilistic Robotics book by Sebastian Thrun and Wolfram Burgard](http://www.probabilistic-robotics.org/)
2. 
3. 