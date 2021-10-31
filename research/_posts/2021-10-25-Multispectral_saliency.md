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

{% capture fig_img %}
![Rededge-MX](/assets/images/Micasense_Rededge-MX.png){: width="60%" height="60%"}
{% endcapture %}
<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Micasense Rededge-MX camera</figcaption>
</figure>

# Approach
To bypass the problems in aligning water images, I used saliency object detection with unaligned images. Saliency detection is finding the most important, eye-catching part in the image. If the drones find salient object while they are flying, they would have to closely investigate the area by lowering the altitude or cropping and magnifying the area.

# Method
