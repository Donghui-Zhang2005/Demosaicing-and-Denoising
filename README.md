# Demosaicing and Denoising
**Frequency-domain Feature Learning Network for Joint Image Demosaicing and Denoising**

## Introduction
This repository provides the official implementation of **FFNet**, a frequency-domain feature learning framework designed for **joint image demosaicing and denoising**. The codebase supports both the **joint denoising and demosaicing (JDD)** task and the **demosaicing (DM)** task, including model training, inference, and dataset preparation.

The following sections describe the software environment, training and evaluation procedures, and the expected dataset organization.

---

## Contents
- [1. Software Environment](#1-software-environment)
- [2. Training](#2-training)
  - [2.1 Training for Joint Denoising and Demosaicing](#21-training-for-joint-denoising-and-demosaicing)
  - [2.2 Training for Demosaicing](#22-training-for-demosaicing)
- [3. Evaluation](#3-evaluation)
  - [3.1 Evaluation for JDD](#31-evaluation-for-jdd)
  - [3.2 Evaluation for DM](#32-evaluation-for-dm)
- [4. Dataset Preparation](#4-dataset-preparation)

---

## 1. Software Environment

The implementation has been developed and tested under the following software configuration:

```bash
python=3.8
numpy=1.21.2
opencv-python=4.5.5.64
pillow=8.4.0
numba=0.55.1
scikit-image=0.18.3
pytorch=1.10.0
torchvision=0.11.1
cudatoolkit=11.3

```

## 2. Training

### 2.1 Joint Denoising and Demosaicing
To train FFNet for the joint denoising and demosaicing (JDD) task, run:
train JDD:

```shell
python train.py --phase train --task JDnDm --model FFNet --in_type noisy_rgb
```
In this setting, the model takes noisy_rgb as input and is optimized to handle demosaicing and denoising in a unified manner.
train DM:

### 2.2 Demosaicing
To train FFNet-DM-B for the demosaicing (DM) task, use:
```shell
python train.py --phase train --task DM --model FFNet-DM-B --in_type rgb
```
If the repository includes additional architectures, the target network can be changed by replacing the value of the --model argument.

## 3.Test
### 3.1Testing for JDD
To evaluate a pretrained FFNet model on the JDD task, run:
```shell
python jdd_test.py --phase test --model FFNet --test_path dataset/JDnDm/test/ --pretrain logs/JDnDm/DIV2K/model_FFNet-in_type_noisy_rgb-C_64-B_32-Patch_128-Epoch_200/checkpoint/model_FFNet-in_type_noisy_rgb-C_64-B_32-Patch_128-Epoch_200_checkpoint_best.pth
```
This command performs inference on the test images located in dataset/JDnDm/test/ using the specified pretrained checkpoint.

To evaluate FFNet-DM-B on the DM task, run:
```shell
python dm_test.py --phase test --model FFNet-DM-B --test_path dataset/JDnDm/test/ --pretrain logs/JDnDm/DIV2K/model_FFNet-DM-B-in_type_rgb-C_64-B_32-Patch_128-Epoch_200/checkpoint/model_FFNet-DM-B-in_type_rgb-C_64-B_32-Patch_128-Epoch_200_checkpoint_best.pth 
```

The test logs will be saved in ```./logs/.../xxx_test.log``` folder and results will be saved in ```./logs/.../results/...``` folder.

## 4.Dataset 
The training stage uses the DIV2K dataset. Please download the original images from the official source:
 [DIV2K](https://data.vision.ee.ethz.ch/cvl/DIV2K/) train dataset 

The following benchmark datasets are used for evaluation:[Kodak](http://r0k.us/graphics/kodak/), [McMaster](https://www4.comp.polyu.edu.hk/~cslzhang/CDM_Dataset.htm) and [Urban100](https://github.com/jbhuang0604/SelfExSR) test datasets

The data folder should be like the format below:
```
dataset
├─ DIV2K
│ ├─ train     % 32592 images
│ │ ├─ DIV2K_train_HR_sub
│ │ |  ├─ xxxx.png
│ │ |  ├─ ...
│ | |
| | |
│ ├─ valid     % 4152 images
│ │ ├─ DIV2K_valid_HR_sub
│ │ |  ├─ xxxx.png
│ │ |  ├─ ...
|
|
├─ JDnDM
│ ├─ test
| │ ├─ Kodak     
| │ │ ├─ xxxx.png
│ | | ├─ ......
│ │ |
| | |
│ | ├─ McMaster
│ │ | ├─ xxxx.png
│ │ | ├─ ......
| | |
| | |
│ | ├─ Urban100   
│ │ | ├─ xxxx.png
│ │ | ├─ ......
...
```
