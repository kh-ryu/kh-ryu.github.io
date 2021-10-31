---
layout: archive
title: "Object detection in drone-captured image using YOLO-v3"
tags:
    - Image Processing
    - Machine learning
---
<br/>

<center>
**AI System Design Contest** <br>
SNU Department of System Semiconductor Engineering for AI <br>
Kyunghyun Ryu, Pureun Kim, Keemin Kim <br>
**2nd Prize**
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

