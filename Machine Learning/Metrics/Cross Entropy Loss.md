Used for classification.

$$
L_{CE} = - \sum_{i=1}^nt_{i}\log(p_{i}), \space\space \text{for n classes}
$$

where $t_i$ is the truth label and $p_i$ is the softmax probability for the $i^{th}$ class.


> [!INFO] Intuition
>  $-\log{0} = \infty$ and $-\log(1) = 0$.  So the CE loss penalizes the model for having lower probability for the class of the truth label. 

