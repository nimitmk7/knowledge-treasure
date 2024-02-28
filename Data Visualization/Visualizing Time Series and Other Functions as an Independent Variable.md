## Introduction

When one of the two variables can be thought of as time, time imposes additional structure on the data. Now the data points have an inherent order; we can arrange the points in order of increasing time and define a predecessor and successor for each data point.

## Individual Time Series

### Scatterplot
-  Represents the actual data as its raw form
- Not very visually aesthetic, especially to see patterns over time.
Example:
![[Pasted image 20240214082153.png]] 
### Line Graph
- Used for measurements-based temporal data
- Lines in a sense correspond to made-up data. So they may not represent observed data accurately, especially when there are only a few observations that are spaced far apart.
- Lines help with perception when the points are spaced far apart or are unevenly spaced.
- Lines places more emphasis on the overall trend in data and less on individual observations. 
- Denser the time series, the less important it is to show individual observations with dots.

![[Pasted image 20240214091429.png]]
- However if we have too many categories, plotting every single category using line chart and placing them all together is very noisy : **Overplotting**
- Solving Overplotting:
	- **Grouping**: Combine data so there are fewer categories
	- **Filtering/Focusing**: Only show a few categories of interest.
	- **Highlighting**: Gray out un-interesting categories and highlight the categories of interest with bright color and larger line width.
	- **Faceting**
### Area graph
- Emphasizes the overarching trend in the data, because it visually separates the area above the curve from area below.
- **Visualization only valid if y-axis starts at zero**.

![[Pasted image 20240214091623.png]]
#### Stacked Area Chart
- Useful to depict proportion over time.
- But only width is meaningful, temporal trend is hard to read.

![[Pasted image 20240215193131.png]]

#### Percent Area Chart


![[Pasted image 20240215221848.png]]


## Multiple Time Series and Dose-Response Curves

- We often have multiple time courses that we want to show at once. In this case, we have to be more careful in how we plot the data, because the figure can become con‐ fusing or difficult to read.
- Scatterplot might be a bad idea, as the individual time courses run into each other, and it is difficult to read. Connecting the dots with lines alleviates the issue.

![[Pasted image 20240214091832.png]]
![[Pasted image 20240214091854.png]]
- The separate legend still creates unnecessary cognitive load, which we can reduce by labelling the lines directly.

![[Pasted image 20240214091954.png]]
> [!INFO] Line graphs are not limited to time series. They are appropriate whenever the data points have a natural order that is reflected in the variable shown along the x axis, so that neighboring points can be connected with a line. 

## Time Series of Two or more Response Variables

- With the tools from the preceding sections, we can visualize such data as two separate line graphs stacked on top of each other. This plot directly shows the two variables of interest, and it is straightforward to interpret.
- However, because the two variables are shown as separate line graphs, drawing comparisons between them can be cumbersome.
- **If we want to identify temporal regions when both variables move in the same or in opposite directions, we need to switch back and forth between the two graphs and compare the relative slopes of the two curves**.

![[Pasted image 20240214092406.png]] 
### Connected Scatterplot
- As an alternative to showing two separate line graphs, we can plot the two variables against each other, drawing a path that leads from the earliest time point to the latest 
- Such a visualization is called a connected scatterplot, because we are technically making a scatterplot of the two variables against each other and then are connecting neighboring points.

![[Pasted image 20240214092533.png]]
- When drawing a connected scatterplot, it is important that we indicate both the direction and the temporal scale of the data. Without such hints, the plot can turn into a meaningless scribble. Here, we used a gradual darkening of the color to indicate direction; alternatively, one could draw arrows along the path.

- Even though connected scatterplots can show only two variables at a time, we can also use them to visualize higher-dimensional datasets. The trick is to apply dimension reduction first. e.g. PCA

### Connected scatterplot vs Separate line graph

- Separate line graphs tend to be easier to read, but once people are used to connected scatterplots, they may be able to extract certain patterns (such as cyclical behavior with some irregularity) that can be difficult to spot in line graphs.
- Research reports that readers are more likely to confuse order and direction in a connected scatterplot than in line graphs, and less likely to report correlation. 
- On the flip side, connected scatterplots seem to result in higher engagement, and thus such plots may be effective tools to draw readers into a story.

### HeatMap
- A heat map is ==a 2-dimensional data visualization technique that uses color to represent the magnitude of individual values in a dataset==. The variation in color may be by hue or intensity
- It is a great tool when we have many categories of data.
![[Pasted image 20240215223628.png]]

- Great tool to visualize trends.
- Not accurate to read accurate measurements, as colour is not quantitatively perceptive.
- Can also size to visualize quantities instead of color.

![[Pasted image 20240215223750.png]]

### Bubble Plot/Punch Card
- Great tool to visualize event data.
![[Pasted image 20240216110751.png]]
- Transparency used to show the overlapping circles properly.
- Encodes data/time of the event in one spatial axis, and some other category in the other spatial attribute.
- Color and Size are used to encode event attributes.

### Gantt Chart
- Great tool to visualize event data with duration. Event has a start-time and end-time. E.g. Taxi Trips

![[Pasted image 20240216111712.png]]
- Very popular in project management for management of tasks, especially when there are tasks where parallelizable/seriable.
#### Ranged Dot Plot Lollipop Chart
- Instead of bars, just use a horizontal line. 
![[Pasted image 20240216111930.png]]

## Periodic Data
E.g. Temperature, GDP Data
- Common Usages:
	- Visualize change within a single period
	- Compare across different periods.
- Can use Calendar layout for heatmap
- Can use Radial Layout(avoids temporal discontuity) for line chart.


## Visual Scalability
-  Use **Horizon Chart**
	- Convert line chart to area chart
	- Different shading for different sections of area based on measurement value, and different colour for negative and positive values
	- Flip the negative part onto the positive part(Reduces height by 2), color helps us to not lose information.
	- Stack the shades on top of each other.
	![[Pasted image 20240216135953.png]]
- Use **Sparklines**
	- Sparklines are small, intense, wordsized graphics with typographicresolution. Sparklines are can be placed anywhere that words or numbers or graphics can be placed: in sentences, maps, graphics, tables.
	![[Pasted image 20240216140235.png]]

## Other Charts
### Candlestick chart

![[Pasted image 20240216140413.png]]
### Connected Scatter Plot

![[Pasted image 20240216140559.png]]

### Time Curves

![[Pasted image 20240216140802.png]]


## Other remarks

1. Aspect ratio matters in line charts(example shows same graph with different aspect ratios)
	![[Pasted image 20240215222239.png]]
	- **Banking to 45 degrees rule**: If we take a line chart, and compute average slope of lines, the best aspect ratio is where the average absolute slopes of the lines are around 45 degrees.
2. 