# ReliMat: Reliability-Aware Refinement for High-Quality Video Matting

<img style="max-width: 100%;" src="https://github.com/rizwanchouhan/relimat/blob/main/resources/wax.png" alt="Title Overview">

## Overview

We propose ReliMat, a reliability-aware video matting framework that predicts pixel-wise matte reliability to guide refinement and preserve fine details. A reference-frame learning strategy captures long-range temporal dependencies, improving robustness to motion and appearance variations. Extensive experiments demonstrate state-of-the-art performance on challenging real-world datasets.

# 👁️💬 Architecture

## 🧾 Framework Overview

The proposed ReliMat framework combines a matte reliability predictor, reliability-aware supervision, and reference-frame learning. These components estimate prediction confidence, guide adaptive refinement, and capture long-range temporal dependencies for accurate and temporally consistent video mattes.

<img style="max-width: 100%;" src="https://github.com/rizwanchouhan/relimat/blob/main/resources/method.jpg" alt="ReliMat Overview">


## 🛠️ Installation

### 1. Clone the Repository

```bash
git clone YOUR_REPOSITORY_URL
cd relimat
```

### 2. Create the Conda Environment

We recommend using Python 3.10.

```bash
conda create -n relimat python=3.10 -y
conda activate relimat
```

### 3. Install PyTorch

Install the PyTorch version compatible with your CUDA environment.

For example:

```bash
pip install torch torchvision
```

> For reproducible experiments, please use the PyTorch and CUDA versions specified in the release notes or environment file provided with the repository.

### 4. Install Dependencies

```bash
pip install -r requirements.txt
```

If the repository provides a local package configuration, install it with:

```bash
pip install -e .
```

---

## 📦 Pretrained Models

Download the released ReliMat checkpoints and place them under:

```text
pretrained_models/
├── relimat.pth
└── ...
```

If multiple checkpoints are provided, use the checkpoint corresponding to the dataset/model configuration you want to evaluate.

Example:

```text
pretrained_models
└── relimat.pth
```

> Replace the checkpoint URL and filename with the official release path once the pretrained models are publicly available.

---

# 🔥 Inference

## Input Format

ReliMat takes a video together with an initial foreground/target mask.

The input directory can be organized as:

```text
inputs/
├── videos/
│   ├── sample1.mp4
│   └── sample2.mp4
│
└── masks/
    ├── sample1.png
    └── sample2.png
```

The mask should indicate the target foreground object(s) in the reference/first frame according to the input protocol used by the released model.

---

## Quick Test

After downloading the pretrained model, run:

```bash
python inference.py \
    --input inputs/videos/sample1.mp4 \
    --mask inputs/masks/sample1.png \
    --checkpoint pretrained_models/relimat.pth
```

If your implementation uses a different inference entry point, replace `inference.py` and the corresponding arguments with the provided script.

For example, a folder containing video frames can be processed as:

```bash
python inference.py \
    --input inputs/videos/sample1 \
    --mask inputs/masks/sample1.png \
    --checkpoint pretrained_models/relimat.pth
```

---

## Output

The generated results are saved to the specified output directory.

A typical output structure is:

```text
results/
└── sample1/
    ├── alpha/
    │   ├── 000000.png
    │   ├── 000001.png
    │   └── ...
    │
    ├── alpha.mp4
    └── foreground.mp4
```

The alpha output contains the predicted continuous-valued matte, where:

- `0` represents background,
- `1` represents foreground,
- intermediate values represent partially transparent regions.

---

## ⚡ Command-Line Interface

If a CLI entry point is provided, inference can also be launched using:

```bash
relimat --input inputs/videos/sample1.mp4 \
        --mask inputs/masks/sample1.png \
        --checkpoint pretrained_models/relimat.pth
```

Run:

```bash
relimat --help
```

to view all available options.

---

# 📊 Evaluation

ReliMat is evaluated on three real-world video matting benchmarks:

- **VideoMatte**
- **YoutubeMatte**
- **CRGNN**

The primary evaluation metrics include:

- **MAD** ↓ — Mean Absolute Difference
- **MSE** ↓ — Mean Squared Error
- **Grad** ↓ — Gradient error
- **Conn** ↓ — Connectivity error

Please refer to the evaluation scripts/configurations included in the repository for the exact benchmark protocol.

### Evaluate VideoMatte

```bash
python eval_hr.py \
    --dataset videomatte \
    --data_root YOUR_VIDEOMATTE_ROOT \
    --checkpoint pretrained_models/relimat.pth
```

### Evaluate YoutubeMatte

```bash
python eval_lr.py \
    --dataset youtubematte \
    --data_root YOUR_YOUTUBEMATTE_ROOT \
    --checkpoint pretrained_models/relimat.pth
```

### Evaluate CRGNN

```bash
python eval_crgnn.py \
    --dataset crgnn \
    --data_root YOUR_CRGNN_ROOT \
    --checkpoint pretrained_models/relimat.pth
```

> Replace the command-line arguments with the exact evaluation interface implemented in the released repository.

---

# 📚 Dataset Preparation

## VideoMatte

Prepare the VideoMatte dataset according to its official organization and place it under:

```text
data/
└── VideoMatte/
    ├── train/
    ├── test/
    └── ...
```

## YoutubeMatte

Place the YoutubeMatte videos and corresponding alpha annotations under:

```text
data/
└── YoutubeMatte/
    ├── videos/
    ├── alpha/
    └── ...
```

## CRGNN

Place the CRGNN dataset under:

```text
data/
└── CRGNN/
    ├── videos/
    ├── alpha/
    └── ...
```

The exact directory layout should follow the dataset preparation scripts included with this repository.

---
