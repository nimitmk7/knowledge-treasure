
## Human Eye Anatomy

![[Pasted image 20240403223202.png]]
Ref: https://www.thoughtco.com/how-the-human-eye-works-4155646

We can only see a small region with high resolution(Fovea). Very little is stored in our memory.

## Strategies
### Saccadic Eye Movement
- Traverse through the image very fast.

We don’t see images with our eyes, but we see them with our brains.
### Attention
- What we attend to and retain is highli

### Inattentional Blindness

## Visual Queries
- The activity of attention-driven eye movements to find information we need to complete a task.

## Visual Channels

### List of channels
![[Pasted image 20240404093445.png]]

### Expressiveness
- What can be expressed with the visual channel

> [!TIP] Among the common visual channels used, *Shape* has the least expressiveness. It can only represent discrete or categorical data. 
> 
## Effectiveness
- How well can this information be expressed with this visual channel

## Accuracy
- Perceived Value vs Actual value different for different channels
![[Pasted image 20240405214232.png | 500]]
- **Channel Accuracy Ranking**: Position > Length and Angle > Area
- But sometimes, we do have to tradeoff accuracy with scalability.

## Discriminability
- It is about being able to visually distinguish values.
- How many distinct values within this channel can viewers perceive ?
- It depends on:
	1. Channel Properties
	2. Spatial arrangement
	3. Size/Resolution
	4. Cardinality

### Spatial Arrangement
Size, if kept ordered is easier to distinguish, like on the left side.
![[Pasted image 20240405215803.png]]
Same for colour. On the left, we are able to clearly see both shades of all colours used.
![[Pasted image 20240405215840.png]]

Left better than right, in both cases. 

### Size
![[Pasted image 20240405215959.png]]
If the element is very small in size, we lose the ability to distinguish between colours. Bigger size allows the colour to be seen more clearly.

![[Pasted image 20240405220038.png]]

## Cardinality
- The more categories we have, harder it is to distinguish.

![[Pasted image 20240405220250.png]]

### Implications for Design
1. Do not overestimate the number of values a viewer can distinguish.
2. Categorical data with high cardinality is one of the hardest types of data to visualize, because encoding them with color, shape etc. decreases our ability to distinguish categories apart. Thus the coping strategies are:
	1. **Grouping**
	2. **Filtering** 
	3. **Faceting**: Show a subplot for each category.
## Saliency
- The ability to stand out
### Pre-attentive Processing
- Task performed in less than 200 to 250 milliseconds.
- Faster than eye movement initiation
- Processed in parallel by low level visual system.

### Guide to pre-attentive features

![[Pasted image 20240405221451.png | 500]]

<iframe width="560" height="315" src="https://www.youtube.com/embed/Dec1x4c9VKY?si=pF2Y07i9D6k60kNc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Non-attentive features

![[Pasted image 20240405221741.png]]

- Task that require multiple channels are non-attentive! 
- **Conjunctive search**: requires searching for two features at the same time.
- Non-attentive features require visual search

### Ranking of features for pre-attention

- Color > Size ![[Pasted image 20240405222113.png]]

- Also, features are not symmetric for pre-attention.
![[Pasted image 20240405222146.png]]
### Implications for Design
1. Think carefully about how to direct viewer’s attention
2. The more channels you have, the harder it is to tune into something specific.
3. Use color sparingly, and gray color is your friend.

## Separability
- The amount of interference among multiple channels.

### Examples:
1. Position + Color: Separable
2. Shape + Color: Not separable(Color overpowers shape)
3. Width + Height: Not separable

### Implications for design
1. Use integral dimensions when holistic reading is required.
2. Use separable channels when viewers should be able to tune on one variable at a time.

## Grouping
- Pattern formation: Visualization is all about pattern identification. What makes a pattern emerge ?
- Gestalt laws of grouping:
	- Principles on how individual visual objects are grouped to form a pattern.

### Gestalt’s Laws
1. Proximity
2. Similarity
3. Connection
4. Enclosure

![[Pasted image 20240405223520.png|400]]
**Relative order**: Enclosure > Connection > Proximity > Similarity

### Other techniques

#### Closure
Gaps in open structures are perceived as closed if it makes sense.

![[Pasted image 20240405224243.png]]

#### Continuity
Objects aligned along a shape are perceived as a group even when interrupted by gaps or other objects.

![[Pasted image 20240405224426.png]]
#### Alignment
Objects that are aligned are perceived as belonging to the same group.

![[Pasted image 20240405224535.png]]

## Putting it all together

![[Pasted image 20240405224724.png]]
- **Accuracy**: Position > Length and Angle > Area
- **Discriminability**: Use Grouping, filtering and faceting for data with high cardinality.
- **Saliency**: The ability to pop out, by using pre-attentive processing step in the visualization pipeline.
- **Separability**: Avoid using non-separable channels together.
- **Grouping**: Patterns emerge when data points are perceived together. Use Proximity, Similarity, Connection and Enclosure.


