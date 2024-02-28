
|  | Underfitting | Overfitting | Unknown Fit | Good Fit |
| ---- | ---- | ---- | ---- | ---- |
| **Validation Error** | High | High | Low | Low (slightly higher than training error) |
| **Training Error** | High | Low | High | Low |
| **Bias** | High | Low | Balanced | Balanced |
| **Variance** | Low | High (or highly sensitive to random outliers) | Balanced | Balanced |

•There are four different categories of fitting:

  1. Underfitting – Validation and training error is high (high bias or systematic error),

  2. Overfitting – Validation error is high, training error is low (high variance or sensitivity to random outliers),

  3. Good fit – Validation error is low, slightly higher than the training error, and

  4. Unknown fit - Validation error is low, training error is 'high’.

### **Underfitting models**
- They are models that are too simple to capture the pattern in the training data well. 
- Underfitting models suffer from high bias, which, following Raschka, measures how far off the predictions are from the correct values in general

## Overfitting models
- They suffer from high variance. 
- Variance measures the variability of the model prediction for a particular sample instance if we were to retrain the model multiple times, on different subsets of the training dataset.
- Overfitting models are too sensitive to the randomness (meaning, the outliers) in the training data