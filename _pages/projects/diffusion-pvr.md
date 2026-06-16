---
layout: archive
title: "Evaluating Pre-trained Visual Representations with Diffusion Policies"
permalink: /projects/diffusion-pvr/
author_profile: true
---

*16-831 Introduction to Robot Learning · Carnegie Mellon University · Fall 2024*

[← Back to all projects]({{ site.baseurl }}/projects/) &nbsp;·&nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/RobotLearning_16-831_final_report.pdf) &nbsp;·&nbsp; *Team project with Giri Anantharaman, Micah Nye, and Pablo Ortega-Kral*

---

## Problem

Diffusion-based visuomotor policies (Chi et al.) have shown strong results on robotic manipulation, but they rely on **end-to-end trained ResNet-18 visual encoders** — which means significant data and compute requirements for every new task.

Meanwhile, the vision community has produced powerful **pre-trained visual representations (PVRs)** — DINOv2, R3M, ImageNet — that capture general visual features from massive datasets. The natural question: can frozen PVRs replace the trained encoders in diffusion policies, lowering training cost and improving generalization to novel scenes?

## Approach

<img src="{{ site.baseurl }}/images/projects/diffusion_policy_overview.png" alt="Diffusion policy with PVR encoder" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Diffusion policy architecture with the ResNet-18 encoder replaced by a frozen PVR. Transformer-based diffusion network ε_θ predicts noise to denoise an action sequence conditioned on the visual observation.*

We took the Diffusion Visuomotor Policy framework and **systematically replaced the encoder** with four different PVR backbones, keeping the rest of the policy fixed:

1. **DINOv2** (large and base variants) — self-supervised vision transformer, ImageNet-scale
2. **R3M** — Reusable Representations for Robotic Manipulation, trained on the Ego4D dataset (3500 hours of egocentric clips)
3. **ImageNet1K_V1** — standard supervised pre-training baseline (ResNet-18)
4. **End-to-end ResNet-18** — original diffusion policy baseline, trained from scratch

PVRs were **frozen during policy training** to test whether existing encodings could perform well without task-specific fine-tuning — mirroring how PVRs have been used successfully in classical RL and imitation learning.

Two research questions:

1. How does the use of PVRs compare to training an end-to-end encoder in diffusion policies?
2. How do different PVRs compare to each other in this setting?

Evaluation: simulated **Push-T** manipulation environment, training for 3000 epochs for a fair comparison against the end-to-end baseline.

## Results

**Frozen PVRs consistently underperformed end-to-end trained encoders.** This was a *negative result*, and an interesting one: PVRs work well as drop-in replacements in many imitation-learning and RL settings, but **diffusion policies appear to depend on encoder features that the standard PVRs don't capture** — likely the action-specific structure that the end-to-end ResNet learns through the denoising objective.

The implications for practitioners: frozen-PVR diffusion policies are not a free lunch. Fine-tuning the PVR (or returning to end-to-end training) is required for competitive performance.

## Tech Stack

PyTorch · Denoising Diffusion Probabilistic Models (DDPM) · Diffusion Visuomotor Policy · Transformer architectures · DINOv2 / R3M / ImageNet pretraining

## What I Learned

- Pre-trained features that work for *recognition* don't automatically work for *control* — diffusion policies expose this in a way that simple BC doesn't
- Negative results are publication-worthy when they're systematic — a methodology that *should* work but doesn't tells the field where the actual problem is
- The honest answer to "should I use a frozen PVR?" is "fine-tune it or train end-to-end" — full stop

---

[← Back to all projects]({{ site.baseurl }}/projects/)
