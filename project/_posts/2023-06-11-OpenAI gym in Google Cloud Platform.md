---
layout: archive
title: "How to use OpenAI gym in Google Cloud Platform(GCP)"
tags:
    - Google Cloud Platform
    - Reinforcement Learning
    - OpenAI gym
    - Mujoco
---
<br/>

_Contents of this post mainly comes from [JEINA DE'VLOG](https://jeinalog.tistory.com/8) and several reference blog posts. Reference links are inserted in the paragraph_

Although running codes in your local machine could be the easiest way, sometimes it might be impossible to do that due to computation limitation like RAM or memory storage, device and version compatibility, or just slow computation. Especially, if your planning on using large deep learning model or reinforcement learning, using cloud service could be necessary because of their huge computation and data. 

Before using GCP, I tried on using Google Colab. However, I felt it is highly limited in installing dependencies like OpenAI gym and they tend to cut off the connection if there are no interaction with the server. Therefore, in this post, I'll go through setting up Google Cloud Platform(GCP) for Reinforcement Learning project, expecially with OpenAI gym environments. 

## Content
### 1. Setting up Virtual Machine in GCP
1) Sign up to Google Cloud Platform. 

2) Create a Project. 

3) Quota request(For GPU). 

4) Creat VM instance. 

5) Install Google Cloud SDK. 

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

### 1) Install basic packages
* Install pip3

```shell
sudo apt-get update
sudo apt-get install python3-pip -y
```

* Install CUDA(If you are using GPU instance, Not yet tested)
[NVIDIA official](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=deb_local)

```shell
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.1.1/local_installers/cuda-repo-ubuntu2004-12-1-local_12.1.1-530.30.02-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-12-1-local_12.1.1-530.30.02-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2004-12-1-local/cuda-*-keyring.gpg/usr/share/keyrings/
sudo apt-get update 
sudo apt-get -y install cuda
```

After the installation, you can check the CUDA version by `nvidia-smi`

* Install cuDNN(If you are using GPU instance, Not yet tested)

* Install Deep Learning packages

```shell
pip3 install tensorflow-gpu torch torchvision keras
```

* Install other packages

```shell
pip3 install jupyter sklearn matplotlib seaborn pandas
```

### 2) Install OpenAI gym
You can install OpenAI gym by just using pip

```shell
pip install gym
```

### 3) Install mujoco-py
If you want to use mujoco environments in OpenAI gym, you should install mujoco-py. You can refer this [instruction](https://gist.github.com/saratrajput/60b1310fe9d9df664f9983b38b50d5da) for installation. However, we'll try to install it without using Anaconda

1. Install git

```shell
sudo apt install git
```

2. Install the Mujoco library(Recommend to start from home directory)

* Download the Mujoco library
```shell
wget https://mujoco.org/download/mujoco210-linux-x86_64.tar.gz
```

* Create a hidden folder
```shell
mkdir /home/username/.mujoco
```

* Extract the library to the .mujoco folder
```shell
tar -xvf mujoco210-linux-x86_64.tar.gz -C ~/.mujoco/
```

* Modify .bashrc file
```shell
# Replace user-name with your username
echo -e 'export LD_LIBRARY_PATH=/home/user-name/.mujoco/mujoco210/bin 
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/nvidia 
export PATH="$LD_LIBRARY_PATH:$PATH" 
export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libGLEW.so' >> ~/.bashrc
```

* Source bashrc
```shell
source ~/.bashrc
```

3. Install mujoco-py

```shell
sudo apt update
sudo apt-get install patchelf
sudo apt-get install python3-dev build-essential libssl-dev libffi-dev libxml2-dev  
sudo apt-get install libxslt1-dev zlib1g-dev libglew1.5 libglew-dev python3-pip

# Clone mujoco-py.
cd ~/.mujoco
git clone https://github.com/openai/mujoco-py
cd mujoco-py
pip install -r requirements.txt
pip install -r requirements.dev.txt
pip3 install -e . --no-cache
```

4. Reboot your machine

```shell
sudo reboot
```

5. After reboot, run these commands to install additional packages

```shell
sudo apt install libosmesa6-dev libgl1-mesa-glx libglfw3
sudo ln -s /usr/lib/x86_64-linux-gnu/libGL.so.1 /usr/lib/x86_64-linux-gnu/libGL.so
# If you get an error like: "ln: failed to create symbolic link '/usr/lib/x86_64-linux-gnu/libGL.so': File exists", it's okay to proceed
pip3 install -U 'mujoco-py<2.2,>=2.1'
```

6. Check if mujoco-py is properly installed

```shell
cd ~/.mujoco/mujoco-py/examples
python3 setting_state.py
```

### Install garage
[garage](https://github.com/rlworkgroup/garage) is a toolkit for developing and evaluating reinforcement learning algorithms. You can install garage using pip.

```shell
pip install --user garage
```

Meanwhile, you might have to use specific version of gym to make it compatible with garage. I have been working with gym version 0.21.0

```shell
pip install gym==0.21.0
```

Thank you for following this post and hope this helped you!
