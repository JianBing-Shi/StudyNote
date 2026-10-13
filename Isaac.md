# Isaac 
## Setup
本指南提供在 Ubuntu 20.04 上安装 *IsaacSim 4.5.0 + IsaacLab 2.1.0* 的详细步骤，包括下载、安装、配置和验证过程。

[指南](https://juejin.cn/post/7498970712473747497#heading-29)


1. 硬件要求
    * CUDA 12.1以上，ROS Noetic
    * GPU: NVIDIA RTX系列显卡
    * 驱动版本: 推荐 535.129.03 或更高版本
    * 内存: 至少 32GB RAM
    * 显存: 至少 16GB VRAM
    * 存储: 至少 200GB可用空间

2. 配置虚拟环境
    ```bash
    # 创建并激活 conda 环境
    conda create -n isaaclab_4_5_0 python=3.10 pip setuptools wheel -y
    conda activate isaaclab_4_5_0
    # 验证pip是否正确安装 pip --version
    # 安装必要的基础包
    conda install pip setuptools wheel  

    # 安装 PyTorch 2.5.1
    conda install pytorch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1  pytorch-cuda=12.1 -c pytorch -c nvidia
    ```