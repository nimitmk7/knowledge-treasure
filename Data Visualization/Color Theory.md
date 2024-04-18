## Color Use cases and Scales

### 1. Label: Distinguish Groups of Data from each other

We frequently use color as a means to **distinguish discrete items or groups that do not have an intrinsic order**.
**Examples**: Different countries on a map, different manufacturers of a certain product. 

In this case, we use a **qualitative** color scale. Such a scale contains a finite set of specific colors and fulfills these conditions: 
1. Colors should look clearly distinct from each other while also being equivalent to each other. 
2. No one color should stand out relative to the others. 
3. The colors should not create the impression of an order, as would be the case with a sequence of colors that get successively lighter. As, Such colors would create an apparent order among the items being colored, which by definition have no order.

> [!WARN] Humans can perceive between 5-10 distinct codes.

 ![[qual_color_scales.png]]
#### Example
![[Pasted image 20240412220931.png]]
### 2. Quantify: Represent data values
Color can also be used to represent quantitative data values, such as income, temperature, or speed. In this case, we use a **sequential** color scale.

Such a scale contains a sequence specific colors which fulfills these conditions:
1. **Discriminability**: Clearly indicate which values are larger or smaller than which other ones. 
2. **Uniformity**: How distant two specific values are from each other(i.e.color scale needs to be perceived to vary uniformly across its entire range.) Difference in values = Perceived difference in color.

![[Pasted image 20240413181156.png]]

Sequential scales can be based on a **single hue** (e.g., from dark blue to light blue) or on multiple hues (e.g., from dark red to light yellow). Multi-hue scales tend to follow color gradients that can be seen in the natural world, such as dark red, green, or blue to light yellow, or dark purple to light green. The reverse (e.g., dark yellow to light blue) looks unnatural and doesn’t make a useful sequential scale.

![[seq_color_scales.png]]
**How to generate single hue sequential scale?**
1. Choose a hue.
2. Keep saturation constant.
3. Map data value to luminance.

**Why use multi-hue over single-hue?**
1. Perceptual sequence preserved.
2. Better aesthetics and discriminability
3. Used for segmentation and labeling.

Representing data values as colors is particularly useful when we want to show how the data values vary across geographic regions. In this case, we can draw a map of the geographic regions and color them by the data values. Such maps are called **chloropleths**. 

Refer to example 1 and [[Spatial Visualization Techniques]].

In some cases, we need to visualize the deviation of data values in one of two directions relative to a neutral midpoint. The appropriate color scale in this situation is a **diverging** color scale. We can think of a diverging scale as two sequential scales stitched together at a common midpoint, which usually is represented by a light color. Diverging scales need to be balanced, so that the progression from light colors in the center to dark colors on the outside is approximately the same in either direction. Otherwise, the perceived magnitude of a data value would depend on whether it fell above or below the midpoint value. Refer to example 2.

**How to generate diverging color scale?**
1. Same saturation on both side.
2. Different hues.
3. Value maps to luminance.
4. Same luminance slope on both side.

→ Generate two sequential color maps and combine them at the center.


![[Pasted image 20240412222021.png]]
#### Examples
1. 
 ![[Pasted image 20240412221658.png]]
2. ![[Pasted image 20240412222052.png]]
### 3. As a tool to highlight 

There may be specific categories or values in the dataset that carry key information about the **story we want to tell**, and we can strengthen the story by emphasizing the relevant figure elements to the reader. <br>
An easy way to achieve this emphasis is to color these figure elements in a color or set of colors that vividly **stand out against the rest of the figure**. 

This effect can be achieved with **accent color scales**, which are color scales that contain both a set of subdued colors and a matching set of stronger, darker, and/or more saturated colors.

![[accent_color_scale.png]]
When working with accent colors, it is **critical that the baseline colors do not compete for attention**. Notice how drab the baseline colors are in 1st example. Yet they work well to support the accent color. 

It is easy to make the mistake of using baseline colors that are too colorful, so that they end up competing for the reader’s attention against the accent colors. There is an easy remedy, however. Just **remove all color from all elements in the figure except the highlighted data categories** or points, just like in example 2 below.

#### Examples
1. 
![[color_highlight_example.png]]
2. ![[Pasted image 20240412220330.png]]

## Color Semantics
Green = OK/Good
Red = Danger/Bad
Gray = No color

**Pro**: Intuitive when used appropriately.
**Con**: May not apply across cultures.

## Misusing Color

1. Avoid Rainbow and Jet Color Maps becuase its ordering of colors is not perceptually intuitive.
2. Avoid mixing multiple attributes using color because human perception can’t decouple them.
3. Avoid encoding too many colors for encoding categories.
4. Use diverging color map when data is bidirectional

## Color Perception

- **Rod Cells** - Responsible for Light and Dark perception, are evenly distributed throughout the retina
- **Cone Cells** - Responsible for Color perception, they are more in the centre region of the retina.

![[photoreceptor_cell.png]]
![[rods_and_cones.png]]
### Trichromacy Theory
The trichromatic theory of color vision is ==a theory that explains how the human eye perceives color==. The theory states that the human eye can only perceive three colors of light: **red**, **blue**, and **green**. The wavelengths of these three colors can be combined to create every color on the visible light spectrum.

So we have 3 types of cone cells for each of these 3 colors. 
1. Short-wavelength
2. Medium-wavelength
3. Long-wavelength

![[3_cones.png]]

The visual cortex combines information from these 3 types of cone cells to generate color perception. So we do not perceieve color in the amount of green, red and blue, but as a single combined color.

### Opponent Process Theory
The ability to perceive color is controlled by three receptor complexes with opposing actions. These three receptor complexes are the red-green complex, the blue-yellow complex, and the black-white complex.

![[op_proc_theory.png]]

#### Negative After Image
![[negative_after_image.png]]
### Color Specification

#### Specify by name
1. Not scalable
2. Our perception of color is often wrong.

![[Pasted image 20240413172412.png]]

Color Perception Illusion Debate: https://en.wikipedia.org/wiki/The_dress

#### Specify by wavelength
- Not user friendly

##### Color Metamerism
Metamerism is a perceived matching of color with non-matching spectral distrubution.

![[Pasted image 20240413172631.png]]
##### Color Space

![[Pasted image 20240413172747.png]]
#### RGB/CMYK
![[Pasted image 20240413172843.png]]


![[Pasted image 20240413173209.png | 500]]

- RGB is popular for display devices
- CMYK is popular for printed medium.
- Both can be specified using hex values. E.g. # FF0000(R=255, G=0, B=0)
- It is difficult to decompose a color into RGB/CMYK channels with human perception system.

#### HSV/HSL

![[Pasted image 20240413173548.png]]
- It is the polar coordinate representation of the RBG Color space.
- Easier for humans to understand and edit.
- Reducing saturation convert the color into some shades of gray.(Effective in reducing visual distractions)
- Reducing value/lightness leads to black.

#### CIE Lab/CIE Luv
L = Lightness
a = green-magenta/Red
b = yellow-blue

- Equal distance in the $L^*a^*b^*$ space are perceived as equal. So it is a perceptually *uniform* color space.
- It intentionally applies non-linear operations to make math align to what humans expect to see.

![[Pasted image 20240413174230.png|500]]
- LAB is best used for desaturating an image. It captures contrast and colors better than linear RGB.
![[Pasted image 20240413174357.png]]

- It is hard to specify color with “a” and “b” color channel.

## Color Blindness

It is caused by missing or defective photoreceptors. 
![[Pasted image 20240413121056.png]]

### Types
#### 1. Deuteranopia
Deuteranopia is a type of red-green color blindness that **causes certain shades of green to appear more red**. It's the most common type of color vision deficiency, affecting about 8% of men and 0.5% of women. 

Deuteranopia is usually **genetic** and causes people to have **missing M cones**, which prevents them from perceiving green light. As a result, people with deuteranopia mostly see blues and golds, and may **confuse some shades of red with some shades of green**, or **yellows with bright shades of green**.

#### 2. Protanopia
Protanopia is a type of color blindness that occurs when the **L cones in the retina are missing**, **preventing the perception of red light**. People with protanopia **see colors as shades of blue or gold**, and may confuse different shades of **red with black**.

#### 3. Tritanopia
Tritanopia is a rare type of color blindness that makes it difficult to distinguish between colors that contain blue or yellow, such as blue and green, purple and red, and yellow and pink. It also makes colors appear less bright. People with tritanopia have normal red and green vision.

#### 4. Monochromacy
Monochromacy, also known as achromatopsia, is ==a rare condition that causes people to have complete color blindness and can only see in shades of gray==. People with monochromacy can't perceive colors, but instead see the world in shades of gray ranging from black to white.

> [!INFO]  10% male and 1% female have some color deficiencies. Most common form is red-green color blindness.

### Recommendation for visualization
1. Use single hue color map if possible.
2. Blue/Red, Blue/Orange are often safe.
3. Use IBM, Wong, Tol color maps.
4. If green/red can’t be avoided, use on-the-line labeling and/or fine tune the color intensity.
5. Test design with color blindness simulators.

Refer to: [The Economist Visual Styleguide](https://design-system.economist.com/documents/CHARTstyleguide_20170505.pdf)



## References
1. https://clauswilke.com/dataviz/color-basics.html
2. https://sourcegraph.com/blog/strange-loop/strange-loop-2019-rgb-to-xyz-the-science-and-history-of-color
3. 