---
layout: archive
title: "Disentangled Latent World Models for Mobile Robots"
permalink: /projects/world-models/
author_profile: true
---

*16-761 Mobile Robots · Carnegie Mellon University · Spring 2026*

[← Back to all projects]({{ site.baseurl }}/projects/) &nbsp;·&nbsp; [Final presentation (PDF)]({{ site.baseurl }}/files/Disentangled_WorldModels_16-761_final_presentation.pdf)

---

## Problem

Dense world models predict every pixel (or every latent dimension) at every timestep. For a mobile robot moving through a mostly-static environment, that's wasted compute — most of the scene is constant. Can we get the same planning quality from a **disentangled** world model that only predicts the parts of the scene that actually change?

## Approach

<img src="{{ site.baseurl }}/images/projects/disentangled_wm.gif" alt="LeWorldModel rollout" style="max-width:100%;border-radius:6px;margin:1em 0;">

I built a comparison between two world-model formulations:

1. **Dense baseline** — JEPA-style joint-embedding predictive architecture that predicts the full latent state at each step
2. **Disentangled** — Built on **LeWorldModel**: separate factors for static scene structure vs. dynamic objects; only the dynamic stream is rolled out forward in time

The agent then plans against rollouts from each model. Comparison metrics covered planning quality, rollout fidelity, and per-step compute cost.

## Results

The disentangled formulation produced comparable rollout fidelity at meaningfully lower compute per planning step. More interesting was the interpretability: factors learned by the disentangled model had visibly cleaner semantic separation (object vs. background), making downstream tasks like "avoid that obstacle" easier to specify.

## Tech Stack

PyTorch · JEPA architecture · LeWorldModel · Learned latent dynamics

## What I Learned

- Inductive bias matters: telling the model "this part is static, this part is dynamic" pays off in both efficiency and interpretability
- World-model evaluation is genuinely hard — rollout fidelity ≠ planning quality, and you have to measure both
- Mobile-robot planning benchmarks would benefit from disentanglement as a default, especially for warehouse / indoor settings where the scene is mostly static

---

[← Back to all projects]({{ site.baseurl }}/projects/)
