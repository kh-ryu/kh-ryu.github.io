---
layout: archive
title: "The review of FMT* sampling-based algorithm"
tags:
    - Path planning
    - MATLAB
---
<br/>

<strong> 2021 Spring SNU AE "Decision making for Autonomous Aerospace Systems" </strong><br><br>
**Course Description**
* Path planning 
  * Potential field
  * Graph search
  * Sampling-based algorithms
* Control
  * Controllability of non-holonomic systems
  * Sliding mode control
  * Adaptive control
  * Back stepping control
  * Model predictive control
* Multi-agent systems
  * Graph network
  * Basic game theory


**Term project subject : The review of FMT\* and its comparison with other sampling-based algorithms.**


-------------

[Fast Marching Tree\*](https://journals.sagepub.com/doi/abs/10.1177/0278364915577958?casa_token=1zSSFeJH5dsAAAAA:P6PLR5nGvrkS05Qg2QNt271rCWCfRDu_147vuxRQMhP1orNMhs-0u5_ePdXhMYvX1OFVK4q5PViFjg, "Fast marching tree: A fast marching sampling-based method for optimal motion planning in many dimensions") (FMT\*) is a  sampling-based method for optimal motion planning proposed by L. Janson . In "Decision making for Autonomous Aerospace Systems" class, I presented a comparison of FMT\* and its variation [DFMT\*](https://ieeexplore.ieee.org/abstract/document/7139514?casa_token=nxDaWBXf7y4AAAAA:h_y7c6deayQKB7nffHkjNCeosmTv1BJ4uGLVrwGZ7Rg8ZtS_ZHqaSaXLwS8f1-Uxrd_9QIqVZzU, "Optimal sampling-based motion planning under differential constraints: The driftless case") with previous sampling-based path planning algorithm, Rapidly exploring Random Tree\* (RRT\*) and Probabilistic Roadmap \* (PRM\*) [link](https://journals.sagepub.com/doi/abs/10.1177/0278364911406761?casa_token=dQ-EM-K-n2AAAAAA:mdjaF0rnS0S69kpLX8dEtTjGwb8kQmBM0eEdInhEIlTp4aOuTSrcmfAIsnBiHb6jk3uVt_GPSkHj1w, "Sampling-based algorithms for optimal motion planning").   

The characteristic of FMT\* is that it chooses sample points like PRM and grows trees like RRT\* . It expands outward, which means it never backtrack over previous node.    
Due to this characteristic, FMT\* outperforms previous sampling-based algorithms in terms of computational time. However, this characteristic could lead to suboptimal connection. 

We can directly find their differece with visualized simulation. Simulation is based on MATLAB environment.
{% capture fig_img %}
![FMT star](/assets/images/FMT_sparse.gif)
{% endcapture %}
<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>FMT* algorithm</figcaption>
</figure>

{% capture fig_img %}
![PRM star](/assets/images/PRM.gif)
{% endcapture %}
<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>PRM* algorithm</figcaption>
</figure>

{% capture fig_img %}
![RRT star](/assets/images/RRT_sparse.gif)
{% endcapture %}
<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>RRT* algorithm</figcaption>
</figure>

[1] Janson, Lucas, et al. "Fast marching tree: A fast marching sampling-based method for optimal motion planning in many dimensions." The International journal of robotics research 34.7 (2015): 883-921.<br>
[2] Schmerling, Edward, Lucas Janson, and Marco Pavone. "Optimal sampling-based motion planning under differential constraints: the driftless case." 2015 IEEE International Conference on Robotics and Automation (ICRA). IEEE, 2015.<br>
[3] Karaman, Sertac, and Emilio Frazzoli. "Sampling-based algorithms for optimal motion planning." The international journal of robotics research 30.7 (2011): 846-894.
