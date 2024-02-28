Feature scaling is **the process of normalizing the range of features in a dataset**. Generally, it is a good idea to prepare data to the range of the different transfer functions in machine learning algorithms. 

So you need to scale the target data to within the range that the estimator is able to predict.
In addition, in the case of neural networks that use gradient descent, scaling the target is recommended to avoid gradient explosion.

## Methods
4 ways:
1. Standardization
2. Min-max scaling
3. Mean Normalization
4. Unit length scaling
2 additional ways(exclusive to finance):
1. Linear Detrending
2. Differencing


### Standardization



### Min-max scaling

$$
x' = \frac {x - min(x)}{max(x) - min(x)}
$$
- Scales all data between 0 and 1.

### Mean normalization

$$
x' = \frac {x - \mu}{max(x) - min(x)}
$$
- Ensures your data is between - 1 and 1 with mean as zero.
- Less frequently used method
### Unit length scaling

$$
x' = \frac{x}{||x||}
$$
- For some applications, it is better to not scale individual features, but instead vectors of features.In this case, you would apply unit length scaling by dividing each element in the vector by the total length of the vector, as we can see in the formula in the slide.

- The length of the vector in the denominator usually means the L2 norm of the vector, that is, the square root of the sum of squares.

- For some applications, the vector length means the L1 norm of the vector, which is the sum of vector elements.

### Differencing
- Standard method to achieve stationarity in financial data(especially prices).
- Taking the log: linearizes and normalizes prices, helps to limit changes in variance. 
$$
ln(price)
$$

- Differencing: removes stochastic trends. If you difference the log of prices you get log returns, which have a normal distribution with mean zero:
$$
ln(price_t) – ln(price_{t-1})
$$
### Linear Detrending
- Fit a linear model to the data(trend) and then then substract it from the data.
 **stationary_series = series - trend**

- Linear detrending is acceptable for price data if it is done on a rolling window that is not too long.

## Scaling Requirements for standard ML models

| Algorithm | Features might need scaling? |
| ---- | ---- |
| SVM | Yes |
| KNN | Yes |
| Linear regression | No (unless regularized, multiple) |
| Logistic regression | No (unless regularized, multiple) |
| Naive Bayes | No |
| Decision trees | No |
| Random Forests | No |
| AdaBoost | No |
| Neural networks | Yes |



