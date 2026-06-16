---
layout: archive
title: "Latent Strategies for Ad-Hoc Human-Robot Teaming"
permalink: /projects/ad-hoc-teaming/
author_profile: true
---

*16-867 Human-Robot Interaction · Carnegie Mellon University · Fall 2025*

[← Back to all projects]({{ site.baseurl }}/projects/) &nbsp;·&nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/AdHocTeaming_16-867_final_paper.pdf) &nbsp;·&nbsp; [CHAI poster (PDF)]({{ site.baseurl }}/files/AdHocTeaming_CHAI2026_poster.pdf) &nbsp;·&nbsp; *with Mehul Menon, Adrian Krieger, and Juee Chandrachud*

> **This is the same project that was accepted at the [CHAI Annual Workshop 2026]({{ site.baseurl }}/publications/) (UC Berkeley) as *"Strategy Search for Human-Robot Teaming."*** The class report and the CHAI submission describe the same underlying system — VAE strategy encoder, LSTM belief tracker, MPPI scorer — under two different titles.

---

## Problem

Robots that work alongside people have to **infer what their partner is doing without being told**. In domains like collaborative cooking, search-and-rescue, or factory teaming there's no language channel — intent has to come from observed behavior. Reactive RL policies fail this test: they specialize to a single coordination pattern during self-play and break down when the partner plays differently.

We tested this hypothesis in **Overcooked-AI**, a gridworld cooperative-cooking benchmark that captures rich, ambiguous coordination with multiple ingredients, plating, and delivery routes.

## Approach

<img src="{{ site.baseurl }}/images/projects/teaming_architecture.png" alt="Joint strategy architecture" style="max-width:100%;border-radius:6px;margin:1em 0;">

*System architecture: (a) belief tracker training, (b) VAE strategy-belief training, (c) full inference pipeline coupling belief tracker, decoder, and MPPI scorer.*

We formulated ad-hoc teaming as a **POMDP** where the human's policy is an unobservable latent variable. Solving the full POMDP is intractable, so we reduced it to a **Latent-MDP** by augmenting the environment state with a learned low-dimensional strategy embedding $z$. The agent's policy then conditions on its current belief over $z$ instead of the infinite policy space.

Three components:

1. **Strategy Belief Encoding (VAE).** Two LSTM encoders map 50-step human and robot trajectories to per-agent strategy embeddings $z^H$ and $z^R$. A shared decoder reconstructs joint trajectories from the concatenated $z_{\text{joint}} = [z^H \mid z^R]$ and the initial environment state, so it implicitly learns a latent dynamics model. Trained on **480 human-human Overcooked trajectories** the team collected.

2. **Belief Tracker (LSTM).** Trained on *partial* trajectories (1–50 steps, randomized windows) to predict the human and robot strategy embeddings online from a partial history. This is what makes the system **ad-hoc**: it updates its inference as the partner reveals their behavior, rather than requiring a full trajectory upfront.

3. **Inference Pipeline (MPPI).** At runtime, the belief tracker emits $\hat{z}^H_t$ and $\hat{z}^R_t$. We sample 10 candidate joint embeddings (injecting noise into the robot component), decode them into 10 candidate trajectory pairs, and score each pair with an MPPI controller using environment reward + an MLP strategy-compatibility term. The robot executes the action from the highest-scoring trajectory.

## Results

Evaluated against two baselines: **PPO with Fictitious Co-Play** (the reactive-RL frontier) and **Behavioral Cloning** trained on the same human-human dataset. Two regimes: non-adaptive human replay (fixed pre-recorded trajectories) and adaptive BC-proxy partners (responsive but human-like). 200 episodes per condition.

<img src="{{ site.baseurl }}/images/projects/teaming_strategies.png" alt="Asymmetric Advantages strategies" style="max-width:100%;border-radius:6px;margin:1em 0;">

*The three core human strategies in the asymmetric-advantages layout: onion-only, plate-only, and plate+onion — combined with left/right positions for six distinct strategy-position cases.*

### Non-adaptive human-replay results

The RL baseline collapses to a single rigid pattern despite training on six partners: it peaks in two scenarios that match its converged pattern (onion-only/right and plate-only/left) and **drops to zero in the complementary configurations**. Our method achieves the **best performance across every other strategy-position combination** and stays at ~80% of RL's reward in the two scenarios where RL wins. Against BC across the board: **1.2–2.2× higher reward**.

### Adaptive BC-proxy results

Against an adaptive BC partner (the most realistic simulated human):

- BC-MPPI (ours): **90.6 / 86.3** (right / left position)
- BC-RL: 61.3 / 70.0
- BC-BC: 52.4 / 52.4

Beating BC-BC — two agents trained on identical demonstrations — by **+72.5% / +64.6%** means online MPPI optimization is discovering coordination patterns *not present* in the average human demonstration.

The takeaway: **adaptation vs. specialization is a fundamental trade-off in HRI**. RL specializes — great when the partner matches, catastrophic when they don't. BC stays modest but consistent. Our method *balances* the two through learned representations plus online optimization.

### Human-behavior preservation

<img src="{{ site.baseurl }}/images/projects/teaming_bc_heatmap.png" alt="BC agent spatial occupancy heatmaps" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Spatial-occupancy heatmaps for the BC agent under different partners. Top: BC-BC baseline. Bottom: BC paired with RL or our MPPI method. Lower MSE relative to baseline means the partner forced less behavioral change.*

A teaming agent that wins by forcing the human to change strategy isn't actually winning. We measured how much each method made the BC-proxy partner deviate from its natural occupancy pattern. Our method had **lower MSE on the right side** (0.0026 vs. RL's 0.0064) — RL forces more compensation because of its training-collapse asymmetry.

### Latent space structure

<img src="{{ site.baseurl }}/images/projects/teaming_latent_space.png" alt="Latent strategy space PCA" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Left: latent space colored by ground-truth strategy label. Right: K-means clusters. NMI 0.5880 — about 60% agreement with semantic labels.*

The latent space encodes **functional similarity** rather than the dataset's labels. A "plate+onion" trajectory that pragmatically uses plates only ends up next to "plate-only" trajectories — the VAE merges strategies that are *behaviorally equivalent* despite different labels. The clusters also show **smooth transitions** between adjacent strategies, which is exactly what MPPI needs: small perturbations of $z_{\text{joint}}$ produce small variations in candidate trajectories rather than discontinuous jumps.

## Tech Stack

PyTorch · Variational Autoencoders · LSTM belief tracking · MPPI planning · Overcooked-AI · POMDP / Latent-MDP formulation

## Connection to My Research

The belief tracker over latent partner strategies is directly relevant to my [KG-RL thesis work in the AART Lab]({{ site.baseurl }}/experience/aart-lab/). Both ask the same question: how does *prior structure* — here a learned latent strategy space; there an explicit knowledge graph — replace the failed reliance on reactive policies in collaborative settings.

## What I Learned

- POMDP belief tracking over latent partner strategies is genuinely tractable when the latent space is well-structured. A VAE plus an LSTM is enough
- MPPI as a *scoring layer* is a clean way to bridge high-level discrete strategy reasoning with low-level continuous control
- "Ad-hoc teaming" is a useful frame for HRI more generally: assume you'll meet a partner you've never seen, and design the system to update online rather than memorize

---

[← Back to all projects]({{ site.baseurl }}/projects/)
