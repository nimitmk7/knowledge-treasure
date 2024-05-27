This metric measures the ratio between the number of relevant items in the output list and the total number of relevant items available in the entire dataset. The formula is:

$$\text{recall@k} = \frac{\text{no. of relevant items among top k items ion the output list}}{\text{total relevant items}}$$

## Caveats
1. In some systems, such as search engines, the no. of total relevant items can be very high. This negatively affects the recall, as denominator is very large. 
2. It does not measure the ranking quality.

