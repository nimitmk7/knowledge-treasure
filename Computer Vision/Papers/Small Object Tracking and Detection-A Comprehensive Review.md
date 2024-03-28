Detecting and tracking small objects introduce significant challenges within computer vision due to their subtle appearance and limited distinguishing features, which results in a scarcity of crucial information. 

This deficit complicates the tracking process, often leading to diminished efficiency and accuracy.
## Introduction

**Object detection** involves identifying a target object within a single frame or image, whereas **object tracking** focuses on estimating or predicting the object’s position throughout the video sequence, given its initial location.

### Challenges with Object Tracking
1. Objects move rapidly
2. Objects experience occlusion,
3. Objects become invisible 
4. Presence of noise, 
5. Non-rigid objects,
6. Rotation
7. Scale changes, 
8. Moving cameras
### What is a Small Object ?
A small object is one that appears tiny within the video frame, like a distant
paraglider or soccer ball, often due to being tracked from afar. 

These objects, common in applications such as UAVs, remote sensing, and ball sports, may exhibit poor appearance attributes and are challenging to detect and track due to their size. 

They can often be mistaken for noise due to their minuscule size, negatively impacting tracking accuracy.

Alternatively, as per the MS-COCO metric evaluation, small objects are those with an area of 32 × 32 pixels or less, a threshold generally accepted for datasets involving common objects.

## Taxonomy and Review of Methods

### Unified Track and Detection Methods
#### Filter-based methods


### Track By Detection Methods

