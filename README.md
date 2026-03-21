# Foundation Model for PET-CT

This repository contains the official implementation and pre-trained weights for our 3D Multi-Modal Foundation Models tailored for PET-CT medical image analysis. 

Our framework utilizes an optimized Independent Masking Masked Autoencoder (MAE) pre-training strategy, applying a weighted global Mean Squared Error (MSE) loss ($L = L_{\text{masked}} + 0.2 \cdot L_{\text{unmasked}}$) combined with mean imputation to eliminate blocky artifacts. It supports robust fine-tuning for downstream tasks, such as automated tumor segmentation.

## 🗂️ Repository Structure

Based on our architectural implementations, the repository is organized as follows:
* `preprocessing/`: Scripts for data extraction, robust CT-based bounding box cropping, spatial resampling, and normalization.
* `swinv2base/`: Pre-training and fine-tuning scripts utilizing the **SwinUNETRv2-Base** architecture.
* `swinv2small/`: Pre-training and fine-tuning scripts utilizing the **SwinUNETRv2-Small** architecture.
* `nnunetv2/`: Implementations utilizing the official `PlainConvUNet` from the **nnU-Net v2** underlying backbone library.

## 💾 Pre-Trained Foundation Models

Due to GitHub's file size limitations, our pre-trained model checkpoints (FP32) are hosted externally. Please download the required weights from the link below and place them in your working directory (e.g., `./weights/`).

🔗 **[Download Pre-Trained Weights (Dropbox)](https://www.dropbox.com/scl/fo/2qojv3vn3nxu6smisot8y/ALAYC8EiLp26TbTTy5DGI08?rlkey=x0620mbaggdq5zi5z2au3fvp4&st=c1uscnzh&dl=0)**

| Model Architecture | Parameter Count | File Prefix |
| :--- | :--- | :--- |
| **SwinUNETRv2-Base** (2,2,6,2)-48 | ~74.6M | `swin_mae_best_v2.pth` |
| **SwinUNETRv2-Small** (2,2,2,2)-24 | ~18.3M | `swin_small_mae_best.pth` |
| **nnUNetV2-3d_fullres** | ~31.2M | `nnunet_v2_mim_best.pth` |

## ⚙️ Installation & Requirements

The codebase is built entirely in Python and relies on PyTorch and MONAI. 

**1. Clone the repository:**
```bash
git clone [https://github.com/liu-xiaofeng/Foundation-Model-for-PET-CT.git](https://github.com/liu-xiaofeng/Foundation-Model-for-PET-CT.git)
cd Foundation-Model-for-PET-CT
