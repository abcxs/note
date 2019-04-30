# Fast RCNN

## Object detection
1. numerous candidate object locations must be processed
2. these candidates provide only rough localization that must be refined

## RCNN
1. training is a multi-stage pipeline
2. training is expensive in space and time
3. object detection is slow

RCNN is slow because it performs a ConvNet forward pass each object proposal, without sharing coumputation.

## SPPnet
1. Spatial Pyamid Pooling
2. not need warp region proposal

The root cause is that back-propagation through the SPP layer is highly inefﬁcient when each training sample (i.e. RoI) comes from a different image, which is exactly how R-CNN and SPPnet networks are trained.

each ROI have a very large receptive field, often spanning the entire input image. Since the forward pass must process the entire receptive field, the training inputs are large.

I guess:
Because a large number of images need to be calculated, the efficiency in time and space is very low, which leads to slow update and low efficiency.

## Contributions
1. Higher detection quality than RCNN, SPPnet
2. Training is single-stage, using a multi-task loss
3. training can update all network layers
4. No disk storage is required for feature caching


## train
1. N=2, R=128. Sample 128 ROI from 2 images
2. Fast R-CNN uses a streamlined training process with one ﬁne-tuning stage that jointly optimizes a softmax classiﬁer and bounding-box regressors, rather than training a softmax classiﬁer, SVMs, and regressors in three separate stage