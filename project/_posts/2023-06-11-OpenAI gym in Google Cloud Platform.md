---
layout: archive
title: "How to use OpenAI gym in Google Cloud Platform"
tags:
    - Google Cloud Platform
    - Reinforcement Learning
    - OpenAI gym
    - Mujoco
---
<br/>

# How to use OpenAI gym in Google Cloud Platform(GCP)

_Contents of this post mainly comes from [JEINA DE'VLOG](https://jeinalog.tistory.com/8) and several reference blog posts. References are summarized in the end of this post_

Although running codes in your local machine could be the easiest way, sometimes it might be impossible to do that due to computation limitation like RAM or memory storage, device and version compatibility, or just slow computation. Especially, if your planning on using large deep learning model or reinforcement learning, using cloud service could be necessary because of their huge computation and data. 

Before using GCP, I tried on using Google Colab. However, I felt it is highly limited in installing dependencies like OpenAI gym and they tend to cut off the connection if there are no interaction with the server. Therefore, in this post, I'll go through setting up Google Cloud Platform(GCP) for Reinforcement Learning project, expecially with OpenAI gym environments. 

## Content
### 1. Setting up Virtual Machine in GCP
1) Sign up to Google Cloud Platform
2) Create a Project
3) Quota request(For GPU)
4) Creat VM instance
5) Install Google Cloud SDK

### 2. Install OpenAI gym and mujoco-py
1) Install basic packages
2) Install OpenAI gym
3) Install mujoco-py(For users using mujoco environments in OpenAI gym)
4) Install garage

## 1. Setting up Virtual Machine in GCP
### 1) Sign up to Google Cloud Platform
When you sign up to [GCP](https://cloud.google.com) in first time, you'll get $300 free credit. Also, they won't charge you before you upgrade to paid account. So you don't have to worry about getting charged while your tring bunch of stuffs in the first time!

You can check how much computing you can use with $300 free credit in [Pricing Calculator](https://cloud.google.com/products/calculator/#id=89b96158-21b0-4a74-b219-fb5acbfa0e5f). For example, if you want to use General purpose cpu E2 with 8 vCPU and 32GB RAM, $195 will be charged for one month usage(if you are running 24 hours per day, 7 days per week). 
<img width="421" alt="E2 monthly" src="https://github.com/kh-ryu/kh-ryu.github.io/assets/45212225/a6393b6f-a9a8-4947-a25e-57d4b4855256">


You can start your free trial by creating an account or signing in to (Google Cloud Platform)[https://cloud.google.com] and select _Start Free_. When signing in, __I recommend to use personal account rather than university or buisness account__. If you don't use personal account, you'll lose some freedom to manage your project by organization manager. You'll have to enter your billing information but it won't be charged untill you use all the free credit and decide to upgrade to the full account. 

After logging in, the main console shows the remaining credit and the option to upgrade to full account. 
<img width="1243" alt="main console" src="https://github.com/kh-ryu/kh-ryu.github.io/assets/45212225/e753ad6e-e24c-433e-bc5b-5b5ebc6a6ffd">

### 2) Create a project
__My First Project__ is generated as you sign in to GCP. We'll start from here but you can create new project if you want. 
<img width="1256" alt="new project" src="https://github.com/kh-ryu/kh-ryu.github.io/assets/45212225/0aeb7401-8a87-4255-b9bd-353eef2d29d0">


### 3) Quota request
If you want to use GPU for your virtual machine, you should request for quota and get a permission. If cpu is fine for your job for now, you can skip this part.
__To use GPU, you should upgrade your account to Full account. This can charge for the GPU usage through your billing information after you use all the free credit__

I haven't go through this process yet. However, it is well summarized in (here)[https://stackoverflow.com/questions/45227064/how-to-request-gpu-quota-increase-in-google-cloud].

### 4) Create VM instance
After creating the project, you can see the project overview in the dashboard. 
<img width="1455" alt="dashboard" src="https://github.com/kh-ryu/kh-ryu.github.io/assets/45212225/ddb1e991-0fad-4614-b7a2-482acdf62580">


To creat a VM instance, go through Compute Engine => VM instances
<img width="470" alt="VM instance" src="https://github.com/kh-ryu/kh-ryu.github.io/assets/45212225/fca3558f-b2de-40e9-b5b5-304aa944a254">

__Create an instance__ helps you creating a new VM instance. You can set up an instance name, region, and machine configuration in here. 
__Pricing depends on your region selection and there are some low CO2 options. Also, there can be some unavailable machine options depend on your region selection
__

For machine configuration, I chose general purpose, e2-standard-8. However, it should depend on your project purpose and computation requirements. If you need GPUs, people usually uses K80, P100, and V100. They are cheaper in this order. You can find their specs in [K80 vs P100](https://www.xcelerit.com/computing-benchmarks/insights/nvidia-p100-vs-k80-gpu/) and [P100 vs V100](https://www.xcelerit.com/computing-benchmarks/insights/benchmarks-deep-learning-nvidia-p100-vs-v100-gpu/)

We can set up operating system in boot disk. We'll choose ubuntu 20.04 LTS and set up proper disk size. Keep in mind that some of disk size will be used for operating system.
<img width="763" alt="boot disk" src="https://github.com/kh-ryu/kh-ryu.github.io/assets/45212225/d07dcf93-da05-4777-a91e-95554decd169">

Now you can create VM instance in GCP!

### 5) Install Google Cloud SDK
There are many different ways to connect to your VM instance.
<img width="1056" alt="connect" src="https://github.com/kh-ryu/kh-ryu.github.io/assets/45212225/66439194-0d38-4008-b7e4-ea7b65cf03d4">

If you choose _open in browser window_, there will be a pop-up window terminal which connects to VM instance through ssh.
<img width="902" alt="open in browser" src="https://github.com/kh-ryu/kh-ryu.github.io/assets/45212225/7ece4e86-a461-4976-8194-0bf46fe8fc2d">

If you don't want to opening browser, logging in to GCP, and going through opening terminal in web browser, you can set up gclound and access through your local terminal. To do this, you should install Google Clound SDK by following (SDK Documentation[https://cloud.google.com/sdk/docs/install-sdk]. 

After the install, you can go into _view gcloud command_ and copy and paste that command into your local terminal.

Finally, your all set with setting up your VM machine!

## 2. Install OpenAI gym and mujoco-py
Before you start your project, you should install several packages.

