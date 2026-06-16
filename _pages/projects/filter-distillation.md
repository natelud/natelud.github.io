---
layout: archive
title: "Learning Better Policies Through Safety Filter Distillation"
permalink: /projects/filter-distillation/
author_profile: true
---

*16-886 Embodied AI Safety · Carnegie Mellon University · Spring 2026*

[← Back to all projects]({{ site.baseurl }}/projects/) &nbsp;·&nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/FilterDistillation_16-886_final_paper.pdf) &nbsp;·&nbsp; Codebase forked from [CMU IntentLab/AnySafeReachability](https://github.com/CMU-IntentLab/AnySafeReachability)

---

## Problem

Safety filters — Hamilton-Jacobi reachability shields, Control Barrier Functions — encode something the underlying RL policy never sees: **which states are unsafe, and what minimal deflection from a goal-directed action keeps the system clear of them**. Conventionally these filters are attached as runtime overrides that clip unsafe policy actions, leaving the policy itself untouched. The policy only ever observes a *collision penalty*, not the filter's *reasoning*.

That's a missed opportunity. The filter knows more than "don't crash" — it knows *where* to deflect and *by how much* at every state. We hypothesised that this action-level information is as useful for **training** a policy as it is for filtering one at runtime.

## Approach

<img src="{{ site.baseurl }}/images/projects/anysafe_main.png" alt="Filter distillation pipeline" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Privileged-information distillation framing. **Left:** the teacher (analytic HJ filter) sees the agent, goal, and exact obstacle geometry. **Right:** the student observes only an ego-frame goal vector and 32 body-frame raycast distances — obstacles are present in the world but not in its input.*

I distill the filter **into the policy during training**, instead of bolting it on at deployment. The pipeline:

1. **Analytic HJ teacher.** For a Dubins car with bounded turn rate, I derive a closed-form HJ value $V(s)$ and the corresponding *evasive* and *goal-directed* actions for every state — no learned approximation, no optimisation in the loop
2. **Blended action label.** At each safety-critical state ($V \le 0$), label the action with an $\alpha$-blend: $u_{\text{teacher}} = \alpha \, u_{\text{filter}} + (1 - \alpha) \, u_{\text{goal}}$, where $\alpha = \text{clip}(-V/c, 0, 1)$. Deep inside the unsafe set the label is purely evasive; near the boundary it's mostly goal-directed
3. **Selective BC loss.** Train a SAC student jointly against the task reward *and* a behavioral-cloning loss against the teacher label — but **only at safety-critical states**. The BC term is silent everywhere else, so safety supervision is concentrated exactly where it matters
4. **Two label families.** PROJ (hard projection onto the safe set) and BLEND (the soft $\alpha$-mixture above)

Critically, the resulting policy can be deployed **with or without the filter** — the runtime overhead is negligible either way. The contribution is the *policy* that respects safety as part of optimising the task, not the elimination of the filter.

## Results

Evaluated against seven baselines including SAFE-GIL, CBF-RL, Curriculum Filter Distillation (CFD), and Safety-Augmented Reward Shaping (SARS) on hard random scenes (3–10 obstacles).

<img src="{{ site.baseurl }}/images/projects/anysafe_plot_2.png" alt="Goal rate and safe rate comparison" style="max-width:100%;border-radius:6px;margin:1em 0;">

**Headline numbers (sensor-only, 1000 scenes, 3 seeds):**

| Method | Goal rate | Safe rate |
|---|---|---|
| SAC (no filter) | 0% | 0% |
| SAC + runtime filter | 0% | 100% |
| SAFE-GIL (next-best baseline) | 36.6% | 82.3% |
| **SAC + BC BLEND (ours)** | **95.7%** | **98.1%** |
| SAC + BC PROJ (ours) | 90.3% | 98.2% |

The BLEND variant beat the next-best learning baseline by **59.1 percentage points on goal rate** at a **15.8 pp safety advantage**. Both proposed variants saturated at 100% goal / 100% safe under the privileged-view (with-geometry) setting.

<img src="{{ site.baseurl }}/images/projects/anysafe_plot_0.png" alt="Reward shaping ablation" style="max-width:100%;border-radius:6px;margin:1em 0;">

**Key findings:**

- Pure SAC fails completely (0% goal): the policy never escapes the early collision regime
- Adding a *runtime* filter recovers safety (100%) but task performance stays at 0% — the override prevents collisions but propagates no information back to the policy
- Existing learning baselines occupy an intermediate regime (13–47% goal rate, 42–100% safe). They incorporate the filter at training time, but only as a scalar reward, a state-uniform BC anchor, or a regulariser on shielded transitions — the **action-level structure of the filter is not preserved in the supervision signal**
- The SAC + BC EVASIVE ablation (filter's evasive action as a uniform teacher) hits 23% goal at 100% safe — close to ours on safety but no goal-reaching, isolating that **the action-level filter alone teaches safety but not goal-reaching**
- The combination of action-level supervision *and* a goal-aware label is what closes the gap

## Tech Stack

PyTorch · Stable-Baselines3 / SAC · Analytic Hamilton-Jacobi reachability · Selective behavioral cloning · Dubins-car navigation environment

## What I Learned

- A safety filter is more than a guardrail — it's a *teacher signal* that the policy can learn from if you supervise at the action level rather than via reward
- Concentrating BC supervision on safety-critical states (gating on $V \le 0$) is what keeps the anchor informative without overwhelming the RL objective
- The $\alpha$-blend is easier for a stochastic actor to fit than the bang-bang projection — supports the broader lesson that smoother labels are friendlier to neural policies

---

[← Back to all projects]({{ site.baseurl }}/projects/)
