3D plots are quite popular, in particular in business presentations but also among academics. They are also almost always inappropriately used. It is rare that I see a 3D plot that couldn’t be improved by turning it into a regular 2D figure.

## Avoid Gratuitous 3D

Many visualization tools enable you to spruce up your plots by turning the plots’ graphical elements into three-dimensional objects. Most commonly, we see pie charts turned into disks rotated in space, bar plots turned into columns, and line plots turned into bands. 

Notably, in none of these cases does the third dimension convey any actual data. 3D is used simply to decorate and adorn the plot. This use of 3D is gratuitous. It is unequivocally bad and should be erased from the visual vocabulary of data scientists. 

The problem with gratuitous 3D is that the projection of 3D objects into two dimen‐ sions for printing or display on a monitor distorts the data. The human visual system **tries to correct for this distortion** as it **maps the 2D projection of a 3D image back into a 3D space**. However, this **correction can only ever be partial**. 

### Pie Chart Example
As an example, let’s take a simple pie chart with two slices, one representing 25% of the data and one 75%, and rotate this pie in space . As we change the angle at which we’re look‐ ing at the pie, the size of each slice seems to change as well. In particular, the 25% slice, which is located in the front of the pie, seems to take up much more than 25% of the area when we look at the pie from a flat angle. 
**Nutshell**: Distorted angle.

![[Pasted image 20240505065504.png | 500]]

![[Pasted image 20240505072956.png | 500]]

### Bar Chart example

Below figure shows the breakdown of Titanic passengers by class and gender using 3D bars. Because of the way the bars are arranged relative to the axes, the **bars all look shorter than they actually are**. 

For example, there were 322 passengers total traveling in first class, yet Figure suggests that the number was less than 300. This illusion arises because the columns representing the data are located at a distance from the two back surfaces on which the gray horizontal lines are drawn. 

To see this effect, consider extending any of the bottom edges of one of the columns until it hits the lowest gray line, which represents 0. Then, imagine doing the same to any of the top edges, and you’ll see that all the columns are taller than they appear at first glance.
**Nutshell**: Foreshortening

![[Pasted image 20240505065821.png | 500]]

## Avoid 3D position Scales

It is less clear what to think of visualizations using three genuine position scales ($x$, $y$, and $z$) to represent data. In this case, the use of the third dimension serves an actual purpose. Nevertheless, the resulting plots are frequently difficult to interpret, and they should be avoided if possible.

### 3-D scatterplot example
**Data**: Fuel efficiency vs Displacement vs Power for 32 cars. 
![[Pasted image 20240505070210.png | 500]]
 Even though this 3D visualization is shown from four different perspectives, it is difficult to envision how exactly the points are distributed in space.  I find part (d) particularly confusing. It almost seems to show a different dataset, even though nothing has changed other than the angle from which we look at the dots.

The fundamental problem with such 3D visualizations is that they require two separate, successive data transformations. 
1. Map the data from the data space into the 3D visualization space, in the context of position scales. 
2. Map the data from the 3D visualization space into the 2D space of the final figure. (This second transformation obviously does not occur for visualizations shown in a true 3D environment, such as when shown as physical sculptures or 3D-printed objects. My primary objection here is to 3D visualizations shown on 2D displays.) 

The second transformation is non-invertible, because each point on the 2D display corresponds to a line of points in the 3D visualization space. Therefore, we cannot uniquely determine where in 3D space any particular data point lies.

Our visual system nevertheless attempts to invert the 3D to 2D transformation. However, this process is unreliable, fraught with error, and **highly dependent on appropriate cues in the image** that convey some sense of three-dimensionality. 

When we remove these cues the inversion becomes entirely impossible. This can be seen in the next Figure, which is identical to previous figure, except all depth cues have been removed. 

The result is four random arrangements of points that we cannot interpret at all and that aren’t even easily relatable to each other. Could you tell which points in part (a) correspond to which points in part (b)? 

![[Pasted image 20240505070739.png | 500]]
#### Alternative way to plot

1. If we primarily care about fuel efficiency as the response variable, we can plot it twice, once against displacement and once against power.

![[Pasted image 20240505070931.png | 500]]
2. If we are more interested in how displacement and power relate to each other, with fuel efficiency as a secondary variable of interest, we can plot power versus displacement and map fuel efficiency onto the size of the dots.

![[Pasted image 20240505071039.png | 500]]

### 3-D bars example
**Data**: The mortality rates in 1940 Virginia stratified by age group and by gender and housing location. 
![[Pasted image 20240505071545.png | 500]]

3-D bars indeed help us interpret the plot. It is unlikely that one might mistake a bar in foreground for one in the background, or vice versa. But, it is difficult to judge exactly how tall the individual bars are, and it is also difficult to make direct comparisons. For example, was the mortality rate of urban females in the 65–69 age group higher or lower than that of urban males in the 60–64 age group?
**Nutshell**: Occlusion

In general, it is better to use small multiples plots instead of 3D visualizations. The Virginia mortality dataset requires only four panels when shown as a small multiples plot.

![[Pasted image 20240505071655.png | 500]]
This figure is clear and easy to interpret. It is immediately obvious that mortality rates were higher among men than among women, and also that urban males seem to have had higher mortality rates than rural males, whereas no such trend is apparent for urban and rural females.

## Appropriate Use of 3D

1. The issues described in the preceding section are of lesser concern if the **visualization is interactive and can be rotated by the viewer**, or if it is shown in a VR or augmented reality environment where it can be **inspected from multiple angles**.
2. If the visualization isn’t interactive, **showing it slowly rotating, rather than as a static image from one perspective**, will allow the viewer to discern where in 3D space differ‐ ent graphical elements reside. The human brain is **very good at reconstructing a 3D scene from a series of images taken from different angles**, and the slow rotation of the graphic provides exactly these images.
3. When we want to show actual 3D objects and/or data mapped onto them.

![[Pasted image 20240505072002.png | 500]]
![[Pasted image 20240505072029.png | 500]]

