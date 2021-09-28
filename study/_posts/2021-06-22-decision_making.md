---
layout: archive
title: "Decision making for Autonomous Aerospace Systems"
tags:
    - Path planning
    - MATLAB
---
<br/>
<br/>
This is term project form "Decision making for Autonomous Aerospace Systems".   
The class covered various topics for unmanned vehicle platforms.   
For path planning, potential field, graph search, and sampling-based algorithms are introduced.   
In control part, we learned controllability of non-holonomic systems and various control methods, such as sliding mode control, adaptive control, back stepping contral and model predictive control.   
Finally, graph network of multi-agent systems and basic game theory like nash equilibrium are given.

[FMT\*](https://journals.sagepub.com/doi/abs/10.1177/0278364915577958?casa_token=1zSSFeJH5dsAAAAA:P6PLR5nGvrkS05Qg2QNt271rCWCfRDu_147vuxRQMhP1orNMhs-0u5_ePdXhMYvX1OFVK4q5PViFjg, "Fast marching tree: A fast marching sampling-based method for optimal motion planning in many dimensions") is a fast marching sampling-based method for optimal motion planning proposed by L. Janson . In "Decision making for Autonomous Aerospace Systems" class, I presented a comparison of FMT\* and its variation [DFMT\*](https://ieeexplore.ieee.org/abstract/document/7139514?casa_token=nxDaWBXf7y4AAAAA:h_y7c6deayQKB7nffHkjNCeosmTv1BJ4uGLVrwGZ7Rg8ZtS_ZHqaSaXLwS8f1-Uxrd_9QIqVZzU, "Optimal sampling-based motion planning under differential constraints: The driftless case") with previous sampling-based path planning algorithm, RRT\* and PRM\* [link](https://journals.sagepub.com/doi/abs/10.1177/0278364911406761?casa_token=dQ-EM-K-n2AAAAAA:mdjaF0rnS0S69kpLX8dEtTjGwb8kQmBM0eEdInhEIlTp4aOuTSrcmfAIsnBiHb6jk3uVt_GPSkHj1w, "Sampling-based algorithms for optimal motion planning").   

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


