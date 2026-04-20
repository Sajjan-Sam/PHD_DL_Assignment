# Naturalness-Aware Speech Deepfake Detection

<p align="center">
  <img src="https://img.shields.io/badge/Task-Speech%20Deepfake%20Detection-0A66C2?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Backbone-XLS--R%20%2B%20Conformer-1F8B4C?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Focus-Naturalness--Aware%20Training-7A3E9D?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Extension-HNDL-B22222?style=for-the-badge" />
</p>

This repository explores **speech deepfake detection** through **naturalness-aware training**.  
It is built around the paper **“Naturalness-Aware Curriculum Learning with Dynamic Temperature for Speech Deepfake Detection”** and extends it with a local discrepancy-based framework called **HNDL**.

The main idea is simple: as synthetic speech becomes cleaner and more realistic, artifact-only detection becomes less reliable. Instead of looking only for obvious distortions, this project studies whether **speech naturalness** itself can guide training and improve robustness.

---

## Overview

The original method introduces two key ideas:

- **Curriculum Learning (CL)** based on speech naturalness  
- **Dynamic Temperature Scaling (DT)** to control model confidence based on sample difficulty  

Building on that, this repository also explores:

- **HNDL — Hierarchical Naturalness Discrepancy Learning**
  - local sub-utterance discrepancy modelling
  - hierarchical difficulty fusion
  - discrepancy-guided local-global attention
  - auxiliary supervision for stronger training signals

---

## Why this project matters

Modern TTS and voice conversion systems can generate speech that sounds highly realistic.  
That makes speech deepfake detection harder, especially when obvious synthesis artifacts are no longer present.

This project focuses on a more useful question:

> Can a model learn from **how natural** speech sounds, not just from what is visibly wrong in the signal?

That is the motivation behind the naturalness-aware training pipeline reproduced and extended here.

---

## What is implemented

### Reproduced pipeline
- **DF_BASE** — XLS-R Conformer baseline  
- **DF_CL** — baseline + curriculum learning  
- **DF_CL_DT** — baseline + curriculum learning + dynamic temperature  

### Proposed extension
- **DF_HNDL_EXT** — an extended framework that adds local naturalness discrepancy modelling and auxiliary learning objectives

---

## Core idea behind HNDL

The original framework uses a single **utterance-level MOS score** to measure difficulty.  
This is effective, but it may miss **local changes inside a sample**.

HNDL was designed to address that limitation by asking:

- does one part of the utterance sound less natural than another?
- can those local inconsistencies help the detector?
- can auxiliary losses improve learning when data is limited?

In short, HNDL tries to move from **global naturalness** to **hierarchical naturalness**.

---

## Main observations

- Naturalness-aware curriculum learning is a strong idea, especially when deepfake artifacts are subtle.
- The reproduced CL-based models were highly sensitive to **small-data settings**.
- On a reduced subset, the plain baseline remained stable, while CL and CL+DT degraded noticeably.
- The proposed **HNDL** extension showed much stronger behaviour in that limited-data setup and converged quickly.

---

## Repository structure

```bash
.
├── 01_data.ipynb        # data preparation, checks, subset creation
├── 02_model.ipynb       # model setup, reproduction, extension experiments
├── report.tex           # full technical write-up
├── report.pdf           # compiled report
└── README.md
