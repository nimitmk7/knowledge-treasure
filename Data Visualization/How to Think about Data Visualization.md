**Question to ask**:
~~What visualization tool should I use ?~~
How should I think about Data Visualization?

Data is **encoded** into a visual representation that yields intuition.
Data is represent by a *Mark*.
*Scales* map encodings to values.

## Building blocks of Visualization:

1. Data
2. Transformation
3. Marks
4. Encoding - mapping from fields to mark properties
5. Scale - Functions that map data to visual scales
6. Guides - Visualization of scales

## Important Questions:
1. What visual encoding is the most effective for my data?
2. What mark is most effective for my data?
3. What scale is most effective for my data ?

Most suited to ordered data*: 2D position, Size, Color Value
Most suited to unordered data*: Texture, Color Hue, Angle, Shape

*In decreasing order of effectiveness

![[Pasted image 20240206134437.png]]

## Practical Takeaways:
1. Not encodings are created equal.
	1. Prefer Position Encodings When possible.
	![[Pasted image 20240206134606.png]]
    ![[Pasted image 20240206134704.png]]
    2. Use color scales effectively: Best color scale depends on data type.
	    Before:
	    ![[Pasted image 20240206135018.png]]
	     After:
	     ![[Pasted image 20240206135104.png]]
	     

		   ![[Pasted image 20240206135243.png]]
2. Use Visualization API structured around best-practices.
3. Use a grammar-based charting package. (ggplot2, plotnine, Vega-Lite, Altair)
	1. Visualization concepts should map directly to visualization implementation.
	2. Implementation drives conceptualization.

## Reference:
https://youtu.be/vTingdk_pVM?si=c_cYz9h1gRiK9z4G 



