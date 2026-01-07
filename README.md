# Variational Autoencoder for Single-Lead ECG Reconstruction

We implement a **Variational Autoencoder (VAE)** for modeling and reconstructing **single-lead ECG cardiac cycles** using the **Lobachevsky University Electrocardiogram Database (LUDB)**.

The objective is to establish a **clean** and **high-fidelity baseline** for modeling the latent-space of electrophysiological signals, with performance aligned to other academic benchmarks in VAE-based ECG reconstruction and synthesis.

---

## Overview

The pipeline converts raw ECG recordings into a **25-dimensional latent representation** from which the original cardiac morphology is reconstructed.

This implementation shows:
- Stable training
- Physiologically plausible reconstructions
- Statistically faithful latent representations

---

## Technical Workflow

### 1. Preprocessing & Segmentation
- Raw ECG signals from LUDB are segmented into **individual cardiac cycles**
- Each cycle is resampled to **400 samples**
- Only **single-lead ECG** is used in this version

### 2. Normalization
- Each cardiac cycle is normalized to improve optimization stability
- Ensure consistent reconstruction scaling across samples

### 3. Encoder
- Fully connected neural network
- Maps each 400-sample ECG cycle to:
  - Mean vector **μ**
  - Log-variance vector **log σ²**

### 4. Latent Space
- **25-dimensional latent vector**
- Sampling via the **reparameterization trick**:
  
  z = \mu + \sigma \odot \epsilon,    \epsilon \sim \mathcal{N}(0, I)

- Enables backpropagation through stochastic sampling

### 5. Decoder
- Mirrors the encoder architecture
- Reconstructs the original **400-sample ECG cycle** from the latent vector

---

## Training Details

- **Loss Function**
  - Mean Squared Error (MSE) for reconstruction
  - Kullback–Leibler (KL) Divergence for latent regularization

- **Training Duration**
  - 720 epochs

- **Optimization Goal**
  - Balance reconstruction fidelity with a smooth, structured latent space

- **Performance**
  - Achieved **Maximum Mean Discrepancy (MMD)** values consistent with reported results in VAE-based ECG literature ([*Interpretable Feature Generation in ECG*](https://doi.org/10.3389/fgene.2021.638191))

---

## Results

- **Statistical Fidelity**
  - PCA projections of reconstructed ECGs match closely the original LUDB distribution

- **Benchmark Alignment**
  - Reconstruction quality and latent regularization align with academic references

- **Latent Smoothness**
  - Latent interpolations produce morphologically consistent ECG transitions

---

## Limitations

This model is intentionally minimal and has clear constraints:

- **Single-Lead Only**
  - Does not exploit spatial correlations available in 12-lead ECGs

- **Cycle-Level Modeling**
  - Operates on isolated heartbeats
  - No modeling of long-term rhythm, RR variability, or temporal context

- **Limited Interpretability**
  - Latent dimensions are not explicitly mapped to clinical morphology
  - No direct attribution to P-wave, QRS complex, or T-wave variations

---

## Next Steps

This repository serves as a foundation for more advanced architectures:

- **Conditional VAEs (cVAEs)**
  - Conditioning on pathology labels or patient metadata
  - Deeper architectures/ models for more complex ECG patterns

## Acknowledgements

Research-oriented project by **Giorgos Kritopoulos**, developed as part of work in deep generative modeling for biomedical signals, with a focus on variational autoencoders and latent-space analysis of ECG data.

Date: **Summer of 2025**
