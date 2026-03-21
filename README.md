# Foundation Model for PET-CT

This repository contains the official implementation and pre-trained weights for our 3D multimodal foundation models for PET-CT medical image analysis.

Our framework uses an optimized independent masking masked autoencoder (MAE) pre-training strategy with a weighted global mean squared error (MSE) loss, combined with mean imputation to reduce blocky artifacts. The pre-trained models support robust fine-tuning for downstream tasks such as automated tumor segmentation.

## Repository Structure

The repository is organized as follows:

- `preprocessing/`  
  Scripts for data extraction, CT-based bounding box cropping, spatial resampling, and normalization.

- `swinv2base/`  
  Pre-training and fine-tuning scripts based on the **SwinUNETRv2-Base** architecture.

- `swinv2small/`  
  Pre-training and fine-tuning scripts based on the **SwinUNETRv2-Small** architecture.

- `nnunetv2/`  
  Implementations based on the official `PlainConvUNet` from the **nnU-Net v2** backbone library.

## Pre-Trained Foundation Models

Due to GitHub file size limitations, the pre-trained model checkpoints are hosted externally. Please download the required weights from the link below and place them in your working directory, for example `./weights/`.

**Download link:**  
[Pre-Trained Weights (Dropbox)](https://www.dropbox.com/scl/fo/2qojv3vn3nxu6smisot8y/ALAYC8EiLp26TbTTy5DGI08?rlkey=x0620mbaggdq5zi5z2au3fvp4&st=c1uscnzh&dl=0)

| Model Architecture | Parameter Count | File Prefix |
| --- | ---: | --- |
| **SwinUNETRv2-Base** `(2,2,6,2)-48` | ~74.6M | `swin_mae_best_v2.pth` |
| **SwinUNETRv2-Small** `(2,2,2,2)-24` | ~18.3M | `swin_small_mae_best.pth` |
| **nnUNetV2-3d_fullres** | ~31.2M | `nnunet_v2_mim_best.pth` |

## Installation

The codebase is implemented in Python and primarily depends on PyTorch and MONAI.

### 1. Clone the repository

```bash
git clone https://github.com/liu-xiaofeng/Foundation-Model-for-PET-CT.git
cd Foundation-Model-for-PET-CT
```

### 2. Create a virtual environment

```bash
conda create -n petct_fm python=3.10
conda activate petct_fm
```

### 3. Install dependencies

```bash
# Core deep learning and medical imaging libraries
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
pip install monai SimpleITK scipy numpy tqdm

# Required for nnU-Net v2 backbone integration
pip install dynamic-network-architectures
```

## Usage Guide

### 1. Data Preprocessing

The preprocessing pipeline is designed to handle varying voxel spacings and baseline shifts across datasets such as AutoPET, DeepPSMA, SPADE, and ViMedPET. The scripts dynamically crop empty background regions using CT thresholding (`CT > -500`), standardize voxel spacing, and independently normalize PET and CT using z-score normalization.

```bash
# Example: AutoPET preprocessing
python preprocessing/process_autopet.py
```

> **Note**  
> Please update the `source_base` and `target_base` paths inside the preprocessing scripts to match your local dataset directories.

### 2. Pre-Training (Masked Autoencoder)

To train a foundation model from scratch using the independent masking strategy, use the provided MAE notebooks or scripts. The network masks 50% of the PET and CT inputs independently and reconstructs the original volumes.

```bash
# Example: run SwinUNETR MAE pre-training
jupyter nbconvert --to script MAEv2.ipynb
python MAEv2.py
```

### 3. Downstream Fine-Tuning (Segmentation)

To fine-tune the pre-trained foundation models for downstream segmentation, run the corresponding training scripts. The code supports checkpoint resumption and cosine annealing learning rate scheduling.

```python
# Example inside a fine-tuning script (e.g., 100%training.ipynb)
train_and_evaluate(
    train_fraction=1.0,
    use_foundation=True,  # Set to False to train from scratch
    total_epochs=100
)
```

Make sure that `foundation_ckpt_path` in the script points to the downloaded `.pth` checkpoint file.

## Citation

If you find this repository or the pre-trained foundation models useful in your research, please consider citing our work.

```bibtex
% Citation details will be added upon publication
```

## Contact

For questions regarding the code, data preprocessing, or model weights, please open an issue in this repository.
"""

