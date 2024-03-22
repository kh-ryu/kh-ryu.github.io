---
layout: archive
title: "Saliency Detection using Multi-spectral image"
tags:
    - Image Processing
    - UAV
---
<br/>

**Abstract**: My goal is to detect people in the water with drones with a multi-spectral camera. To bypass the alignment problems in the Micasense camera, I modified the Global Contrast-based saliency detection algorithm for multi-spectral images. My code worked well in detecting saliency objects with the speed of about 8~10 frame/s.

------------------

# Background
For the search and rescue mission in the water, drones detect a human body in the middle of a vast ocean. While the current RGB camera could suffer from recognizing humans, a multi-spectral camera with an infrared spectrum could outperform the RGB camera. Due to the multi-spectral camera's multi-lens system, a drone has to align each capture after taking a photo. However, aligning ocean images would degrade due to a lack of features to use for alignment.

![Rededge-MX](/assets/images/Micasense_Rededge-MX.png){: width="60%" height="60%"}

# Approach
To bypass the problems in aligning water images, I used saliency object detection with unaligned images. Saliency detection is finding the most important, eye-catching part in the image. If the drones find a salient object while flying, they would have to closely investigate the area by lowering the altitude or cropping and magnifying the area.

# Method
For the saliency detection, I modified the Global Contrast based Salient Detection proposed by Cheng et al. [1]. It uses the color distance of a pixel to other pixels in the L\*a\*b color space. The reasons I chose this method are<br/>
(1) It is fast.<br/>
(2) Does not need dataset as training NN.<br/>
(3) Easily extended to a multi-spectral image.<br/>
(4) Does not use gradient, edges, and boundaries (Since we will use unaligned images).

## Pixel Contrast (Histogram Contrast)
The pixel contrast method calculates the saliency value of each pixel. I calculated color distance in a multi-spectral image in 5 channels (Blue, Green, Red, Red Edge, Near Infra-Red).

![Histogram_Contrast](/assets/images/0075 histogram saliency.png){: width="60%" height="60%"}

## Region Contrast
Region Contrast method first segments image to several regions. I used k-means clustering for regional segmentation. Next, the saliency value is calculated for each region using color contrast with other regions.

![Region_Contrast](/assets/images/0075 Saliency.png){: width="60%" height="60%"}

# Thresholding
To find the most salient object, I used thresholding to saliency value. The result of Histogram Contrast and Region Contrast after thresholding is as follows

![Histogram_cut](/assets/images/Histogram Saliency Cut.png){: width="48%" height="40%"}
![Region_cut](/assets/images/Region cut.png){: width="48%" height="40%"}

The result of saliency detection was nearly identical. However, the computational speed of histogram contrast was much faster.

| - | Histogram-Contrast | Region-Contrast |
|---|---|---|
| Computational time(s) | 0.12120 | 0.41966 |

# ROS implementation
I contributed to [micasense_ros](https://github.com/Ehsan67m/micasense_ros.git) package for processing Micasense multispectral image in DJI Manifold-2 (NVIDIA Jetson TX2). SaliencyHC and SaliencyRC receives raw images from micasense camera and calculate saliency map with Histogram Contrast and Region Contrast each. The viewer node is given to check the results. You can find more details in the github link above.

![Saliency_result1](/assets/images/salieny_result1.JPG)
![Saliency_result2](/assets/images/salieny_result2.JPG)

[1] Cheng, Ming-Ming, et al. "Global contrast based salient region detection."IEEE transactions on pattern analysis and machine intelligence 37.3 (2014): 569-582.
[2] Wang, Q., Yan, P., Yuan, Y., & Li, X. (2013). Multi-spectral saliency detection. Pattern Recognition Letters, 34(1), 34-41.
