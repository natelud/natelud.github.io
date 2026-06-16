---
layout: archive
title: "Histogram-Based Defogging of SPAD LiDAR"
permalink: /projects/spad-defog/
author_profile: true
---

*16-823 Physics-Based Methods in Computer Vision · Carnegie Mellon University · Spring 2025*

[← Back to all projects]({{ site.baseurl }}/projects/) &nbsp;·&nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/SPAD_Defog_16-823_final_paper.pdf)

---

<img src="{{ site.baseurl }}/images/projects/spad_header_result.png" alt="Defogging result banner" style="max-width:100%;border-radius:6px;margin:1em 0;">

## Problem

905 nm SPAD LiDAR dominates the AV market and struggles badly in fog. Existing defogging methods either assume uniform fog (fast, interpretable histogram methods) or require expensive learned inverse maps that can produce confident wrong depth — unacceptable for safety-critical perception. I built a method that keeps the speed and physical grounding of histogram defogging while handling spatially varying fog. Full details in the [paper]({{ site.baseurl }}/files/SPAD_Defog_16-823_final_paper.pdf).

<img src="{{ site.baseurl }}/images/projects/spad_fog_effects.png" alt="Fog causes backscatter and attenuation" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Fog hits SPAD LiDAR two ways: **backscatter** floods the histogram with false-positive returns, and **attenuation** weakens the real return.*

## Approach

<img src="{{ site.baseurl }}/images/projects/spad_method_overview.png" alt="Pipeline overview" style="max-width:100%;border-radius:6px;margin:1em 0;">

Three stages: **spatial binning → per-bin EM fog fit → GLRT depth recovery**.

1. **Spatial binning.** Group pixels into 8×8 bins and average their histograms. Trades resolution for the SNR you need to fit a fog model reliably — the characteristic exponential-decay return becomes recoverable
2. **Per-bin fog fit (EM).** Following Sang et al., model the per-bin fog return as $K_{\text{fog}} \cdot \mu_{\text{ext}} / R^2 \cdot e^{-2\mu_{\text{ext}} R} \cdot u(R - R_1)$. Discontinuities in $R_1$ make gradient methods fragile, so I use Expectation-Maximization to fit $\{K_{\text{fog}}, \mu_{\text{ext}}, R_1\}$ independently per bin — natively handling spatially-varying fog
3. **GLRT depth recovery.** Subtract the estimated fog return, then use a Generalized Likelihood Ratio Test between "fog only" ($H_0$) and "fog + object at range R" ($H_1$). Pick the range that maximizes $\log \Lambda(R)$ subject to a detection threshold and a spatial-consistency check

<img src="{{ site.baseurl }}/images/projects/spad_binning_histogram.png" alt="Single-pixel vs 8x8 bin histogram" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Single pixel (top) vs. 8×8 bin average (bottom). The fog's decay envelope is fittable from the bin; lost in single-pixel noise.*

## Results

Evaluated in **Mitsuba + Mitransient** on a pedestrian-braking scenario: 1.8 m × 0.4 m obstacle at 40 m, sensor in uniform fog, swept across $(\mu_{\text{ext}}, \mu_s) \in \{0.2, 0.4, 0.6, 0.8\}^2$ with Poisson photon sampling and additive Gaussian noise. **Depth RMSE (meters), averaged over 5 trials per cell:**

|                              | $\mu_s = 0.2$ | $\mu_s = 0.4$ | $\mu_s = 0.6$ | $\mu_s = 0.8$ |
|---|---|---|---|---|
| $\mu_{\text{ext}} = 0.2$     | 0.523 m        | 0.671 m        | 1.023 m        | 1.537 m        |
| $\mu_{\text{ext}} = 0.4$     | 0.602 m        | 0.814 m        | 1.205 m        | 1.879 m        |
| $\mu_{\text{ext}} = 0.6$     | 0.636 m        | 0.893 m        | 1.681 m        | 2.366 m        |
| $\mu_{\text{ext}} = 0.8$     | 0.659 m        | 0.968 m        | 2.033 m        | **2.982 m**    |

Even in the **worst tested fog** ($\mu_{\text{ext}} = \mu_s = 0.8$), the method recovers obstacle depth within ~3 m of the true 40 m range — still inside the braking envelope for a 50 mph vehicle.

<img src="{{ site.baseurl }}/images/projects/spad_mu_ext_estimation.png" alt="Extinction coefficient estimation accuracy" style="max-width:100%;border-radius:6px;margin:1em 0;">

*$\mu_{\text{ext}}$ estimation error vs. ground truth — sub-0.1 absolute error under mild fog, degrading smoothly with density.*

<img src="{{ site.baseurl }}/images/projects/spad_sample_return.png" alt="Sample reconstructed depth map" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Reconstructed depth map for the braking scene.*

## Tech Stack

Python · Mitsuba + Mitransient · EM · GLRT · SPAD sensor modeling

## Limitations & Future Work

Evaluation was limited to a single uniformly-fogged scene because of transient-rendering cost. Natural next steps: **spatially-varying fog scenes** (the per-bin formulation is built for this), **partial-occlusion tests**, and **real SPAD hardware deployment**.

---

[← Back to all projects]({{ site.baseurl }}/projects/)
