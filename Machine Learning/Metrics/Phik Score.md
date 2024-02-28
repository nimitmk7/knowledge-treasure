Phik (𝜙k) is a new and practical correlation coefficient that works consistently between categorical, ordinal, interval (continuous) and even mixed variables, captures non-linear dependency and reverts to the Pearson correlation coefficient in case of a bivariate normal input distribution.

The Phik algorithm contains a built-in noise reduction technique against statistical fluctuations.

## Advantages
- It is easy to interpret as it ranges between 0 and 1.
- It can be used to evaluate linear and non-linear models.
- It works for all kinds of variables.
- Another advantage is that it is possible to calculate its statistical significance by calculating its p-value.

## Drawbacks
- The calculation of $𝜙_k$ is computationally expensive (due to calculation of some integrals under the hood),
- There is no closed-form formula
- There is no indication of direction
- When working with numeric-only variables, other correlation coefficients will be more precise, especially for small samples

