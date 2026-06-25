# 🎧 AudioGuard — Adversarial Robustness Evaluation for Audio Classification

This repository evaluates the robustness of audio classification models under adversarial attack conditions and measures whether adversarial training improves resistance to gradient-based attacks. The project uses a CNN trained on synthetic UrbanSound8K-style audio data and evaluates performance under FGSM and PGD adversarial attacks.

---

# Overview

This project demonstrates how to build a research-style adversarial machine learning workflow with:

* Audio classification using convolutional neural networks
* Mel-spectrogram feature extraction
* Fast Gradient Sign Method (FGSM) attacks
* Projected Gradient Descent (PGD) attacks
* Adversarial training and model hardening
* Robustness benchmarking and evaluation
* Reproducible experimentation on consumer hardware

---

# Problem Context: Adversarial Machine Learning

Machine learning models are often evaluated using clean test datasets. However, real-world systems may encounter malicious inputs specifically designed to cause misclassification.

This is particularly relevant for audio systems such as:

* Speech recognition
* Voice assistants
* Audio moderation systems
* Speaker identification systems
* Voice-cloning detection systems

Small perturbations added to an audio signal can significantly reduce model performance while remaining difficult for humans to detect.

This project evaluates how robust an audio classifier remains under attack conditions and whether adversarial training can improve resistance to these attacks.

---

# Methodological Challenge

Early experiments reported nearly perfect robustness under both FGSM and PGD attacks.

Further investigation revealed that this result was caused by:

* Extremely low training loss
* Vanishing gradients
* An overly separable synthetic dataset

The dataset and training procedure were revised to create a more realistic robustness benchmark using:

* Frequency jitter
* Increased noise
* Deliberate class overlap
* Label smoothing

A built-in sanity check verifies that gradients remain meaningful before adversarial attacks are evaluated.

This highlights an important lesson in adversarial machine learning:

> A model that appears robust may simply be difficult to attack because gradients have collapsed rather than because genuine robustness has been learned.

---

# Dataset

The project uses a synthetic UrbanSound8K-style audio dataset consisting of:

* 10 audio classes
* Variable frequency ranges
* Background noise augmentation
* Frequency jitter
* Mel-spectrogram feature extraction

The dataset is generated programmatically within the notebook and does not require external downloads.

---

# Key Features

## CNN Audio Classifier

A convolutional neural network is trained on mel-spectrogram representations of synthetic audio samples.

Model architecture includes:

* 4 convolutional layers
* Batch normalization
* Dropout regularization
* Cross-entropy loss with label smoothing

## FGSM Evaluation

The Fast Gradient Sign Method generates single-step adversarial perturbations using:

* ε = 0.03

This provides a fast estimate of model robustness against gradient-based attacks.

## PGD Evaluation

Projected Gradient Descent performs a stronger iterative attack using:

* ε = 0.03
* 10 attack steps

PGD is generally considered a more challenging robustness benchmark than FGSM.

## Adversarial Training

The project trains a hardened model using adversarial examples and compares performance against the baseline model.

This allows direct measurement of how much robustness can be recovered through adversarial training.

---

# Results Summary

| Model    | Clean | FGSM  | PGD   |
| -------- | ----- | ----- | ----- |
| Baseline | 81.9% | 68.8% | 64.4% |
| Hardened | 81.9% | 75.6% | 75.0% |

## Key Findings

* Adversarial training improved FGSM robustness by **6.8 percentage points**
* Adversarial training improved PGD robustness by **10.6 percentage points**
* Clean accuracy remained unchanged
* The largest gains occurred under the stronger PGD attack

These results suggest that adversarial training improved genuine robustness rather than merely defending against a single attack method.

---

# Tech Stack

**Language:** Python

**Deep Learning:** PyTorch

**Audio Processing:** Torchaudio

**Numerical Computing:** NumPy

**Visualization:** Matplotlib

**Platform:** Google Colab

**Research Area:** Adversarial Machine Learning, Robustness Evaluation, Audio Security

---

# Repository Structure

```text
audioguard/
│
├── notebook/
│   └── AudioGuard.ipynb
│
├── paper/
│   └── audioguard.pdf *coming soon! 
│
├── figures/
│   ├── robustness_comparison.png
│   ├── sample_spectrograms.png
│   └── perturbation_visual.png
│
├── results/
│   ├── results.json
│   └── report.md
│
├── requirements.txt
├── LICENSE
└── README.md
```

---

# Setup & Usage

Install dependencies:

```bash
pip install -r requirements.txt
```

Run the notebook:

```text
notebook/AudioGuard.ipynb
```

A CUDA-enabled GPU is recommended for faster training and evaluation.

---

# Evaluation Pipeline

The workflow consists of four stages:

## 1) Dataset Generation

Synthetic audio samples are generated with class-specific frequency distributions and noise augmentation.

## 2) Model Training

The CNN is trained on mel-spectrogram features using label smoothing to prevent loss saturation.

## 3) Adversarial Evaluation

FGSM and PGD attacks are applied to measure baseline robustness.

## 4) Adversarial Hardening

A second model is trained using adversarial examples and re-evaluated under attack conditions.

---

# Outputs

The project generates:

* Clean accuracy metrics
* FGSM robustness metrics
* PGD robustness metrics
* Robustness comparison figures
* Experiment reports
* Reproducible result files

---

# Future Work

* Evaluate on real-world audio datasets
* Test transfer attacks
* Compare additional adversarial training methods
* Evaluate voice-cloning detection systems
* Add certified robustness approaches
