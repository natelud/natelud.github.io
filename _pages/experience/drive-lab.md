---
layout: archive
title: "DRIVE Lab — Driverless Intelligent Vehicles Lab"
permalink: /experience/drive-lab/
author_profile: true
---

*Research Assistant · August 2023 – August 2024*  
*RISS Research Fellow · June 2023 – August 2023*  
*Carnegie Mellon University Robotics Institute · Pittsburgh, PA*  
*Advisor: [Prof. John M. Dolan](https://www.ri.cmu.edu/ri-faculty/john-m-dolan/)*

[← Back to Experience]({{ site.baseurl }}/research/)

---

## Lab Overview

The Driverless Intelligent Vehicles (DRIVE) Lab works on autonomous-vehicle perception, prediction, and planning. I spent **summer 2023 as a [Robotics Institute Summer Scholar (RISS) Fellow](https://riss.ri.cmu.edu/student/nathan-ludlow/)** in the lab, then continued part-time through August 2024 on follow-on multi-agent safe-navigation work.

## Hierarchical Learned Risk-Aware Planning for Human Driver Modeling

<img src="{{ site.baseurl }}/images/projects/riss_graphical_abstract.jpg" alt="Hierarchical risk-aware planning framework graphical abstract" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Graphical abstract: the system predicts surrounding vehicle trajectories, computes a forward-looking Gaussian risk field along the ego path, and plans against a learned risk-aware cost.*

Developed a computational model of human driver behavior for evaluating autonomous-vehicle systems in simulation. The motivation: AV safety testing needs a realistic model of how human drivers actually behave — not just how a rule-based driver would. Real-world testing is expensive, dangerous, and rare; high-fidelity human-behavior simulation lets you stress AV stacks against the long tail of human variability.

**System design:**

1. **Trajectory prediction (LSTM social pooling)** — Predict the next 5 seconds of motion for surrounding vehicles using a Convolutional Social Pooling network, producing 6 candidate futures per agent with associated probabilities
2. **Forward-looking risk field** — For each predicted future above a 10% probability threshold, generate a Gaussian risk field along the path that weights risk along the full vehicle width
3. **Risk-aware planner** — Plan an ego trajectory 5 seconds ahead under the aggregated risk field, with a learnable risk-tolerance parameter
4. **Learned style parameters** — Train the risk parameters on NGSIM real-driving data so the resulting model reproduces actual human driving behavior, not just rule-conformant behavior

**Demos:**

<video controls preload="metadata" style="max-width:100%;border-radius:6px;margin:1em 0;">
  <source src="{{ site.baseurl }}/images/projects/riss_traffic_weave.mp4" type="video/mp4">
</video>

*Traffic weave — ego vehicle (orange) executing a multi-lane maneuver under forward-looking risk evaluation.*

<video controls preload="metadata" style="max-width:100%;border-radius:6px;margin:1em 0;">
  <source src="{{ site.baseurl }}/images/projects/riss_move_out_of_way.mp4" type="video/mp4">
</video>

*Move-out-of-way scenario — driver style emerges from learned risk parameters.*

**Results:**

- Plans 5-second-horizon trajectories in **under 100 ms**, fast enough for closed-loop AV simulation
- Reproduces high-level human behaviors (overtaking, evasive lane changes) that single-second-horizon models miss
- Risk-tolerance parameter cleanly modulates driver style, enabling stress-testing across the population distribution

**Publication:** [Hierarchical Learned Risk-Aware Planning Framework for Human Driving Modeling](https://doi.org/10.1109/ICRA57147.2024.10610354), ICRA 2024 (first author) · [Paper PDF]({{ site.baseurl }}/files/RISS_ICRA2024_paper.pdf) · [Poster PDF]({{ site.baseurl }}/files/RISS_ICRA2024_poster.pdf)

## [Follow-On: Risk-Aware Weighted Buffered Voronoi Cells]({{ site.baseurl }}/experience/wbvc/)

<img src="{{ site.baseurl }}/images/projects/wbvc_comparison_fig.png" alt="Voronoi variant comparison" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

After the RISS Fellowship I continued with the DRIVE Lab on **Risk-aware Weighted Buffered Voronoi Cells (WBVC)** — a CBF-based decentralized safe-navigation method for multi-agent systems. The cell partition uses Control Barrier Functions as a measure of pairwise risk, and the resulting Voronoi cells are weighted and buffered to give adaptive space partitioning with **rigorous formal safety guarantees**. Scaled to up to 25-agent simulations.

This work originated as my [undergraduate Risk-Aware Buffered Voronoi]({{ site.baseurl }}/projects/voronoi-arms/) project at BYU (ME EN 537) and was generalized + extended with Yiwei Lyu, Wenhao Luo (UNC Charlotte), and Prof. Dolan.

**[Read the full write-up →]({{ site.baseurl }}/experience/wbvc/)**

## Tech Stack

PyTorch · Convolutional Social Pooling · NGSIM dataset · Risk-aware planning · Control Barrier Functions · Voronoi tessellation · Decentralized constrained optimization · Multi-agent safe navigation

## Travel & Recognition

Travel to ICRA 2024 in Yokohama, Japan was supported by:

- IEEE-RAS Travel Grant
- BYU Weidman Center for Global Engagement
- Safety21 Travel Grant
- BYU WCSL Grant

---

[← Back to Experience]({{ site.baseurl }}/research/)
