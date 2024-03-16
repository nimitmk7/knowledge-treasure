## Motivation
Non-maximum suppression (NMS) is a post-processing technique that is commonly used in [object detection](https://builtin.com/artificial-intelligence/image-recognition) to eliminate duplicate detections and select the most relevant [bounding boxes](https://builtin.com/machine-learning/data-labeling) that correspond to the detected objects. It’s a critical step in many [computer vision](https://builtin.com/machine-learning/computer-vision) applications, such as face detection, pedestrian detection and object recognition.

## Steps
1. Sort the bounding boxes based on their confidence scores.
2. Select the bounding box with the highest confidence score and save it as a detection.
3. Remove all the bounding boxes that have a significant overlap with the selected bounding box(Using IoU metric). The amount of overlap is typically determined by a predefined threshold value. **Example**: If IoU over 0.3 with the highest confidence bbox, then remove it.
4. Repeat steps 2 and 3 until no more bounding boxes remain.

## Visual Example

![[Pasted image 20240314090206.png]]
## Downside

NMS is very **expensive computationally**: If there are _n_ boxes then the algorithm performs **_O(n²)_** comparisons and quickly becomes a serious bottleneck, especially if the input image is large and the processor is weak.

### Mitigation
Several ways have been invented to mitigate this NMS bottleneck. 
1. Sift through the boxes and **remove the ones with very low confidence level even before the NMS**. This helps get rid of about 90% of the boxes on VOC-Pascal dataset, for example. While this method is useful, it **still requires O(n) operations and the remaining high confidence boxes still require the NMS**.
2. Use a prediction architecture that inherently generates less proposals. **_CenterNet_** is such an architecture.

## References
1. [A Deep Dive Into Non-Maximum Suppression (NMS) ](https://builtin.com/machine-learning/non-maximum-suppression)
2. 