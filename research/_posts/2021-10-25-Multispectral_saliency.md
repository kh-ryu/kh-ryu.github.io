---
layout: archive
title: "Saliency Detection using Multi-spectral image"
tags:
    - Image Processing
    - UAV
---
<br/>

<center>
Abstract: My goal is detecting people in the water with drones with multi-spectral camera. To bypass the alignment problems in Micasense camera, I modified Global Contrast based saliency detection algorithm for mulit-spectral images. My code well detected saliency object with speed about 8~10 frame/s
</center>

# Background
For the search and rescue mission in the water, drones should have to detect human body in the middle of vast ocean. While current RGB camera could suffer from recognizing human, multi-spectral camera with infrared spectrum could outperform RGB camera. Due to multi-lens system of multispectral camera, image of each band should be aligned after taking a photo. However, aligning ocean image would degrade due to lack of features to use for alginment.

![Rededge-MX](/assets/images/Micasense_Rededge-MX.png){: width="60%" height="60%"}

# Approach
To bypass the problems in aligning water images, I used saliency object detection with unaligned images. Saliency detection is finding the most important, eye-catching part in the image. If the drones find salient object while they are flying, they would have to closely investigate the area by lowering the altitude or cropping and magnifying the area.

# Method
For the saliency detection, I modified Global Contrast based Salient Detection proposed by Cheng et al [1]. It uses color distance of a pixel to other pixels in L*a*b color space. The reasons I chose this method is<br/>
(1) It is fast.<br/>
(2) Does not need dataset as training NN.<br/>
(3) Easily extended to multi-spectral image.<br/>
(4) Does not use gradient, edges, and boundaries (Since we will use unaligned images).

## Pixel Contrast (Histogram Contrast)
Pixel contrast method calculates saliency value of each pixel. In multi-spectral image, I calculated color distance in 5 channels (Blue, Green, Red, Red edge, Near Infra Red).

![Histogram_Contrast](/assets/images/0075 histogram saliency.png){: width="60%" height="60%"}

## Region Contrast
Region Contrast method first segments image to several regions. I used k-means clustering for regional segmentation. Next, saliency value is calculated for each region using color contrast with other regions.

![Region_Contrast](/assets/images/0075 saliency.png){: width="60%" height="60%"}

# Thresholding
To find most salient object, I used thresholding to saliency value. The result of Histogram Contrast and Region Contrast after thresholding is as follows

![Histogram_cut](/assets/images/Histogram Saliency Cut.png){: width="48%" height="40%"}
![Region_cut](/assets/images/Region cut.png){: width="48%" height="40%"}

The result of saliency detection was nearly identical. However, computational speed of histogram contrast was much faster.

| - | Histogram-Contrast | Region-Contrast |
|---|---|---|
| Computational time(s) | 0.12120 | 0.41966 |

[1] Cheng, Ming-Ming, et al. "Global contrast based salient region detection." IEEE transactions on pattern analysis and machine intelligence 37.3 (2014): 569-582.
