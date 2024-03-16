## Intuition
The metric is based on the alignment/proximity of the predicted bounding box with the actual bounding box.
## Formulation

$$
IoU = \frac {Area(B_{Pred}\space \cap B_{GT})}{Area(B_{Pred}\space \cup B_{GT})}
$$

where $B_{Pred}$ is the predicted Bounding Box, and $B_{GT}$ is the ground truth/actual bounding box. 

The union acts as a normalizing factor, and thus the IoU metric always lies between 0 and 1.
## Visualization

![[Pasted image 20240313222950.png]]
## Use
- IoU value is compared with a threshold $t$, whose value typically is chosen to be between 0.5 and 0.75.
IoU > $t$ : True Positive
IoU < $t$ : False Positive


