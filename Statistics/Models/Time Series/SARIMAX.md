## Definition
SARIMAX stands for ==Seasonal Autoregressive Integrated Moving-Average with Exogenous Regressors==. It is a statistical model for forecasting that considers seasonality and exogenous variables(like gold price, outdoor temperature, etc.)

**SARIMAX** is an extension of the **ARIMA** class of models. **ARIMA** models are made up of two parts: the autoregressive term (AR) and the moving-average term (MA). **SARIMAX** includes seasonal effects and exogenous factors with the autoregressive and moving average component in the model.

## Formulation

$$
y_t = c + \sum_{n=1}^{p}\alpha_n y_{t-n} + \sum_{n=1}^{q}\theta_{n}\epsilon_{t-n} + \sum_{n=1}^{r}\beta_{n}x_{n_t} + \sum_{n=1}^{P}\phi_{n}d_{t-sn} +
\sum_{n=1}^{Q}\eta_{n}\epsilon_{t-sn} + \epsilon_t
$$

