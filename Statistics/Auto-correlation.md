## Definition
_Autocorrelation, also known as serial correlation, is the correlation of a signal with a delayed copy of itself as a function of the delay. Informally, it is the similarity between observations as a function of the time lag between them._

### Simpler version: 
_Autocorrelation gives you an idea of how data points at different points in time are linearly related to one another as a function of their time difference._

## Important facts about ACF
1. The ACF(autocorrelation function) of a periodic function has the same periodicity as the original process. 
2. The autocorrelation of the sum of periodic functions is the sum of the autocorrelations of each function separately.
3. All time series have an autocorrelation of 1 at lag 0.
4. The autocorrelation of a sample of white noise will have a value of approximately 0 at all lags other than 0.
5. The ACF is symmetric with respect to negative and positive lags, so only positive lags need to be considered explicitly.
6. A statistical rule for determining a significant nonzero ACF estimate is given by a “critical region” with bounds at +/–1.96 × sqrt(n). This rule relies on a sufficiently large sample size and a finite variance for the process.

## PACF
_The partial autocorrelation of a time series for a given lag is the partial correlation of the time series with itself at that lag given all the information between the two points in time._

_You need to compute a_number of conditional correlations and subtract these out of the total correlation._

_For the case of a sine series, the PACF provides a striking contrast to the ACF. The PACF shows which data points are informative and which are harmonics of shorter time periods._

_The PACF, reveals which correlations are “true” informative correlations for specific lags rather than redundancies. This is invaluable for knowing when we have collected enough information to get a sufficiently long window at a proper temporal scale for our data._

