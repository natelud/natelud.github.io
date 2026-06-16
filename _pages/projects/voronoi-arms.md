---
layout: archive
title: "Risk-Aware Buffered Voronoi Multi-Agent Navigation"
permalink: /projects/voronoi-arms/
author_profile: true
---

*ME EN 537 Robotics · Brigham Young University · Fall 2023*

[← Back to all projects]({{ site.baseurl }}/projects/) &nbsp;·&nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/Voronoi_Arms_ME537_final_paper.pdf) &nbsp;·&nbsp; [Abstract (PDF)]({{ site.baseurl }}/files/Voronoi_Arms_ME537_abstract.pdf)

---

## Problem

Multi-agent collision avoidance for dynamic robotic arms is hard in two ways at once: the arms' link geometry creates complex collision shapes, and the arms move fast enough that reactive avoidance has to account for **uncertainty in the other arm's future trajectory**. Buffered Voronoi Cells (BVC) give each agent a private safe region, but they're not risk-aware — they treat all neighbors as equally certain.

## Approach

<img src="{{ site.baseurl }}/images/projects/voronoi_arms_RAVC.png" alt="Risk-aware Voronoi cells" style="max-width:100%;border-radius:6px;margin:1em 0;">

I extended Buffered Voronoi Cells into a **Risk-Aware Weighted Buffered Voronoi Tessellation**:

1. **Weight cell partitioning by motion uncertainty.** Each agent's Voronoi cell shrinks faster against neighbors whose future trajectory is uncertain
2. **Buffer for arm geometry.** Classical BVC assumes point agents; I extended the buffer to account for link envelopes
3. **Decentralized control.** Each arm solves its own QP locally — no central coordinator, no shared optimization
4. **Underlying dynamics.** Computed-torque control with inverse kinematics; recursive Newton-Euler dynamics for the full nonlinear arm model

<video controls preload="metadata" style="max-width:100%;border-radius:6px;margin:1em 0;">
  <source src="{{ site.baseurl }}/images/projects/voronoi_arms_safe.mp4" type="video/mp4">
</video>

*Three dynamic arms operating in a shared workspace under the risk-aware Voronoi tessellation — safe across multiple trial conditions.*

## Results

Tested against a vanilla BVC baseline in multi-trial simulations with randomized initial configurations and trajectory uncertainty. The risk-aware variant maintained collision-free operation across all trials where the baseline failed under high-uncertainty conditions, with only modest reduction in task throughput.

## Tech Stack

Python · Robot dynamics (recursive Newton-Euler) · Inverse kinematics · QP-based decentralized control · Voronoi partitioning

## Follow-On at CMU

This work was the conceptual seed for my **[Risk-aware Weighted Buffered Voronoi Cells (WBVC) project]({{ site.baseurl }}/experience/wbvc/)** with the [DRIVE Lab]({{ site.baseurl }}/experience/drive-lab/) at CMU. The DRIVE Lab follow-on generalized the formulation, added rigorous safety proofs via Control Barrier Functions, and scaled the system to **25-agent simulations** for autonomous-vehicle settings.

## What I Learned

- Decentralized safety guarantees are only as strong as your model of what the *other* agent might do — uncertainty has to be a first-class citizen in the formulation
- Voronoi-based methods are a sweet spot for multi-agent navigation: cheap to compute, formally tractable, and they degrade gracefully when assumptions break
- The right benchmark for collision avoidance is *worst-case across trial conditions*, not average performance

---

[← Back to all projects]({{ site.baseurl }}/projects/)
