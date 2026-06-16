---
layout: archive
title: "Risk-Aware Weighted Buffered Voronoi Cells — Decentralized Safe Multi-Agent Navigation"
permalink: /experience/wbvc/
author_profile: true
---

*DRIVE Lab follow-on · Carnegie Mellon University · Aug 2023 – Aug 2024*  
*Advisor: [Prof. John M. Dolan](https://www.ri.cmu.edu/ri-faculty/john-m-dolan/)*

[← Back to DRIVE Lab]({{ site.baseurl }}/experience/drive-lab/) &nbsp;·&nbsp; [JAAMAS extended paper (PDF)]({{ site.baseurl }}/files/WBVC_JAAMAS_extended.pdf) &nbsp;·&nbsp; *with Yiwei Lyu, John M. Dolan, and Wenhao Luo*

---

## Problem

Multi-agent systems for search and rescue, environmental sampling, and precision agriculture all need **decentralized, provably-safe collision avoidance** that scales to many agents. Existing approaches make different trade-offs:

- **Reciprocal velocity obstacles** assume symmetric reasoning, which breaks under heterogeneous agents
- **Safety barrier certificates** characterize the joint admissible control space — provably safe but tight to construct at scale
- **Model-predictive methods** are expensive per agent
- **Traditional Voronoi tessellations** partition the joint state space by *position only* — they ignore motion, so a fast-moving aggressive agent gets the same cell as a stationary one

The Voronoi family is appealing because the cells are cheap to compute and give a clean distributed safety guarantee, but the position-only formulation wastes information about what each agent is actually doing.

## Approach

<img src="{{ site.baseurl }}/images/projects/wbvc_comparison_fig.png" alt="Voronoi variant comparison — Traditional, Risk-aware, and Risk-aware Weighted Buffered" style="max-width:100%;border-radius:6px;margin:1em 0;">

*From Lyu, Ludlow, Dolan, Luo (JAAMAS extended). Top row: traditional Voronoi cells — partitioned by position only. Middle: Risk-aware Voronoi — incorporates motion. Bottom: Risk-aware Weighted Buffered Voronoi — adds buffer for safety guarantee and weighting for adaptive partitioning.*

We proposed **Risk-aware Weighted Buffered Voronoi Cells (Risk-aware WBVC)** — a variant of Generalized Voronoi tessellation for decentralized multi-agent collision-free navigation.

Three core ideas:

1. **Voronoi cells determined by motion, not just position.** Unlike traditional Voronoi-based collision avoidance, our partition takes both *positional* and *motion* information into account when drawing the cell boundary between pairwise robots
2. **Control Barrier Functions as a risk measure.** We use CBFs as a measure of risk evaluation — the CBF value captures *to what extent* the safety constraints are satisfied between pairwise robots. The Voronoi cell boundary is then determined by:
   - The varying levels of relative effort between pairwise agents to respond to potential collisions
   - The accumulated risk each agent experiences from surrounding agents
3. **Adaptive constrained-space partition.** Aggressive robots moving with higher speed get *larger* response space; less-threatened robots may yield more room for those exposed to higher risk. The system balances individual effort against overall threat

The result is a **distributed** algorithm: each robot computes its own Voronoi cell locally from CBF-derived risk values and stays inside it. No central optimization, no shared QP — and the cell construction comes with **rigorous formal safety guarantees** (proofs in the paper).

## Demos

The three variants we proposed and compared:

- **RAVA** — Risk-Aware Voronoi (no buffer, no weighting)
- **WRAVA** — Weighted Risk-Aware Voronoi (adds weighting, still no buffer)
- **WBRAVA** — Weighted Buffered Risk-Aware Voronoi (the full method)

<video controls preload="metadata" style="max-width:100%;border-radius:6px;margin:1em 0;">
  <source src="{{ site.baseurl }}/images/projects/wbvc_WBRAVA_linear.mp4" type="video/mp4">
</video>

*WBRAVA Linear — the full method on a 2-agent linear configuration. The buffer + weighting + risk-awareness give clean, asymmetric collision avoidance.*

<video controls preload="metadata" style="max-width:100%;border-radius:6px;margin:1em 0;">
  <source src="{{ site.baseurl }}/images/projects/wbvc_WBRAVA_multi.mp4" type="video/mp4">
</video>

*WBRAVA Multi — the full method scaled up. Demonstrated on up to **25 robots** in the paper.*

<video controls preload="metadata" style="max-width:100%;border-radius:6px;margin:1em 0;">
  <source src="{{ site.baseurl }}/images/projects/wbvc_RAVA_linear.mp4" type="video/mp4">
</video>

*RAVA Linear — the unbuffered, unweighted ablation. Compare to WBRAVA above to see what the buffer + weighting actually contribute.*

## Results

- **Formal safety guarantees** — Rigorous proofs that pairwise robots respecting their Risk-aware WBVC constraints remain collision-free
- **Scale** — Simulations on up to **25 robots** demonstrate the decentralized formulation actually scales (no central QP, no shared state)
- **Compared against** traditional Voronoi tessellation, reciprocal velocity obstacles, and safety barrier certificate methods — and against ablations (RAVA, WRAVA, WBRAVC variants) to isolate the contribution of each design choice

## My Contribution

This work originated as a direct extension of my [undergraduate ME EN 537 final project]({{ site.baseurl }}/projects/voronoi-arms/) at BYU, where I introduced the risk-aware buffered Voronoi formulation for dynamic robotic arms. After joining the DRIVE Lab during and after the RISS Fellowship, I worked with Yiwei Lyu and Wenhao Luo to generalize the formulation, rigorize the CBF-based risk measure, and extend it to many-agent autonomous-vehicle scale.

## Tech Stack

Python · Control Barrier Functions · Voronoi tessellation · Decentralized constrained optimization · Formal safety analysis

## Publication

**Decentralized Safe Navigation for Multi-agent Systems via Risk-aware Weighted Buffered Voronoi Cells.** Yiwei Lyu, Nathan Ludlow, John M. Dolan, Wenhao Luo. JAAMAS extended version. [Paper PDF]({{ site.baseurl }}/files/WBVC_JAAMAS_extended.pdf)

---

[← Back to DRIVE Lab]({{ site.baseurl }}/experience/drive-lab/) · [← Back to Experience]({{ site.baseurl }}/research/)
