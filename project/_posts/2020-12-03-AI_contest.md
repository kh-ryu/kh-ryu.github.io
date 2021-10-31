---
layout: archive
title: "Object detection in drone-captured image using YOLO-v3"
tags:
    - Image Processing
    - Machine learning
---
<br/>

<center>
<strong>AI System Design Contest</strong> <br>
SNU Department of System Semiconductor Engineering for AI <br>
Kyunghyun Ryu, Pureun Kim, Keemin Kim <br>
<strong>2nd Prize</strong>
</center>

# Contest Description
In the contest, the goal was developing object detection code with given drone-captured images and labeled data. Participants were restricted to use only given dataset to train the network and used identical server for training. Moreover, they are allowed to use server only for one week. Assessment criteria was mean Average Precision (mAP) of code.

![Dataset](/assets/images/ai_example.jpg)

# Our work

## Detector selection
Due to the limited time, we thought training existing network would be reasonable. We chose YOLO-v3 for object detector. It gives better mAP compared to other real-time detection systems and its pre-trained weight works well with other testsets (more generalized). Our training used YOLOv3-416 pre-trained weight.

## Parameter tuning
We modified several hyper parameters for effective training. 

First, we used learning rate decay during training. Large learning rate in the start of network training could decreases loss very fast. However, as training goes on, large learning rate prevent network from converging to local minimum and make it oscillates. On the contrary, using learning rate decay accelerates learning in initial phase and prevents oscillation in final phase. We used step decaying learning rate regard to training epoch.
>According to [1], we can obtain the same learning curve by increasing the batch size instead of learning rate decay. However, usually batch size is limited to GPU memory.
![learning rate](/assets/images/learning_rate.jpg) [1]


Also, we modified subdivision of YOLO to speed up the training process. Subdivision is how many mini-batches it split its batch in. By reducing subdivision rate, YOLO sends more images to GPU at the same time (Increasing mini-batch size). It not only speed up the training, but also generalize the training more.
[Reference](https://github.com/pjreddie/darknet/issues/224)

## Data Augmentation
In the given dataset, some classes were suffered with the small number of images in those classes. We leveraged data augmentation for the classes with few training images. Data augmentation is increasing the number of training data with minor alterations. We used flip and rotation for several images in such classes.

# Results

![Result](/assets/images/ai_detected.jpg)

[1] Smith, Samuel L., et al. "Don't decay the learning rate, increase the batch size." arXiv preprint arXiv:1711.00489 (2017).