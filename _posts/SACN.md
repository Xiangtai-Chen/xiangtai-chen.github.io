---
title: '快速配置运行SACN'
date: 2020-11-11
permalink: /posts/2020/11/SACN/
tags:
  - cool posts
  - category1
  - category2
---

# SACN

快速配置运行SACN

# 1. 配置环境

## 1.1. 安装miniconda

参考文章里面的步骤安装[miniconda](https://www.jianshu.com/p/edaa744ea47d)，在Linux命令行界面运行以下代码（Windows参考[这里](https://www.jianshu.com/p/17288627b994)）

```bash
wget -c https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

为了确保您在正确的地方开始，让我们验证您是否已成功安装Anaconda。在终端窗口中，输入以下内容：

```bash
conda --version
```

Conda将回复您已安装的版本号，如：conda 4.9.0

## 1.2. 创建虚拟环境

参考[博客](https://blog.csdn.net/H_O_W_E/article/details/77370456)，下面是创建python=3.6版本的环境，取名叫py36

```bash
conda create -n py36 python=3.6
```

## 1.3. 安装Pytorch

建议直接从[官网](https://pytorch.org/get-started/locally/)安装，实验室Linux已安装 CUDA 10.1。

```bash
conda install pytorch torchvision torchaudio cudatoolkit=10.1 -c pytorch
```

验证Pytorch安装是否成功

```bash
import torch 
torch.cuda.is_available()
```

## 1.4. 复制环境

由于我之前已经安装Pytorch，这里我直接复制我已有的纯净Pytorch环境，创建一个专门跑SACN模型的虚拟环境。

```bash
conda create -n huiling --clone pytorch
```

## 1.5. 激活环境

```bash
conda activate huiling
```

## 1.6. 删除环境

```bash
conda remove --name huiling --all
```

# 2. 运行SACN开源代码

## 2.1. 查阅文档

这时跑神经网络的基础虚拟环境已经搭建完毕，打开SACN开源代码[仓库](https://github.com/JD-AI-Research-Silicon-Valley/SACN)。

## 2.2. Git开源项目

从GitHub上面把项目copy下来，使用Git代码

```bash
git clone https://github.com/JD-AI-Research-Silicon-Valley/SACN.git
```

由于国内网络环境因素，Github访问代码仓库速度缓慢，遇到大型仓库git困难，故可用[镜像网站](https://gitclone.com/)加速访问。

```bash
git clone https://gitclone.com/github.com/JD-AI-Research-Silicon-Valley/SACN.git
```

这时会自动下载项目代码至SACN文件夹下

```bash
cd SACN/
```

## 2.3. 安装SACN所需环境

一般作者会将项目特需的环境自动化部署，我们只需要运行作者的部署代码，例如此模型作者在readme中写清了安装步骤：

Installation This repo supports Linux and Python installation via Anaconda.

Install PyTorch 1.0 using official website or Anaconda.

Install the requirements:

```bash
pip install -r requirements.txt
```

Download the default English model used by spaCy, which is installed in the previous step

```bash
python -m spacy download en
```

## 2.4. 数据预处理

Data Preprocessing

Run the preprocessing script for FB15k-237, WN18RR, FB15k-237-attr and kinship:

```bash
sh preprocess.sh
```

## 2.5. 运行模型

To run a model, you first need to preprocess the data. This can be done by specifying the process parameter.

For ConvTransE model, you can run it using:

```bash
CUDA_VISIBLE_DEVICES=0 python main.py model ConvTransE init_emb_size 100 dropout_rate 0.4 channels 50 lr 0.001 kernel_size 3 dataset FB15k-237 process True
```

For SACN model, you can run it using:

```bash
CUDA_VISIBLE_DEVICES=0 python main.py model SACN dataset FB15k-237 process True
```

You can modify the hyper-parameters from “src.spodernet.spodernet.utils.global_config.py” or specify the hyper-parameters in the command. For different datasets, you need to tune the parameters.
