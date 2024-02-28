Aesthetics describe every aspect of a given graphical element. 

## Aspects of a graphic element
Graphic element =  Position + Shape + Size + Color (Basic attributes)

Other attributes(optional): Line width, line dash-dot pattern, Font family, font size, transparency

![[Pasted image 20240205194216.png]]

All aesthetics fall into two groups: 
1. Those that can represent continuous data
2. Those that can only represent discrete data

## Aesthetics and Types of Data
**Factors**: Variables holding qualitative data
**Levels**: Different categories of a factor(may have an intrinsic order)

![[Pasted image 20240205194703.png]]
## Scales Map Data Values onto Aesthetics

**Scale**: Data Values <--> Aesthetics values mapping

A scale defines a unique mapping between data and aesthetics. A scale must be **==one-to-one==**, such that for each specific data value, there is exactly one aesthetics value, and vice versa.

![[Pasted image 20240205215801.png]]
Example: Daily temperature data 
**Option 1**: Temperature on Y-axis, Day of the year on x axis, Location on Color

![[Pasted image 20240205220234.png]]
**Option 2**: Location on Y-axis, Day of the year on X-axis, Temperature on Color

![[Pasted image 20240205220339.png]]
We can use more than 3 scales as well. Example: 
2 position scales, 1 color scale, 1 size scale, 1 shape scale

![[Pasted image 20240205220647.png]]



