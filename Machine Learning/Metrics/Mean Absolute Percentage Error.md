## Introduction
Mean absolute percentage error measures the average magnitude of error produced by a model, or how far off predictions are on average.

## Calculation

$$MAPE = \frac{\sum\frac{|A-F|}{A} \times 100 }{N}  $$
where
$N$ is the number of fitted points
$A$ is the actual value
$F$ is the forecast value

## Interpretation

A lower MAPE value indicates a more accurate prediction – an MAPE of 0% means the prediction is the same as the actual, while a higher MAPE value indicates a less accurate prediction.

### Practical Assessment of prediction

![[Pasted image 20240401095937.png]]

## Limitations

In determining whether to use MAPE, one of the most important things to understand is how prominent and consequential outliers are to your use case. MAPE is ideal in the cases where there are little to no: outliers, values near zero, values at zero, and low volume / sparse datasets. 

Mean absolute error and mean absolute percentage error may be less sensitive to outliers than alternative metrics, such as root mean squared error (RMSE), but even actual values that are on the small end of the value range in a dataset can explode MAPE towards infinity.

## References
1. https://arize.com/blog-course/mean-absolute-percentage-error-mape-what-you-need-to-know/
2. 