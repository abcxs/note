# RCNN

## key insights:

- pre-training
- apply high-capacity CNNs to bottom-up region proposals

## process
1. selective search proposal region
2. CNN extract feature
3. SVM classifier

## points
1. fine-tuning for detection imporoves mAP performance by 8 percentage points
2. fully connect layer are not the key
3. use selective search to generate region proposals
4. network AlexNet
5. regradless of the size or aspect ratio of candidate region, we warp all pexels in a tight bounding box around it to the required size(227 * 227)
6. Piror to warping, we dilate the tight bounding box so that at the warped size there are exactly p pexels of warped image context around the original box(p = 16)
7. feature matrix 2000 * 4096, SVM weight matrix 4096 * N, the N is the number of classes.

## train
1. replace the CNN's ImageNet-specific 1000-way classification layer with a randomly initialized 21-way classification layer(20 VOC classes plus background)
2. treat all region proposals with $\geq$  0.5 IOU overlap with a ground-truth box as positives fot that box's class and the rest as negatives
3. start SGD at a learning rate of 0.001 (1/10 of the initial pre-training rate), which allows fine-tuning to make progress without changing the initialization a lot
4. batch: 128(32 positive and 96 background), bias the positive examples becase they are extremely rare compared to background
5. use linear svm classifier, train a binary classifier for each class
6. for svm classifier, the overlay threshold is 0.3, the region overlap with gound truth below it woule be negative example; positive examples are defined simply to be ground-truth bounding boxes for each class.
Notice, the threshold is important. For example, setting 0.5 will decrease mAP by 5 points, and setting 0.3 will decrease mAP by 4 points
7. use standard hard negative mining method, it converges quickly
8. bounding box regression, boosting mAP by 3 to 4 points
9. use a combination of classical tools from computer vision and deep learning