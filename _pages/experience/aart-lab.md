---
layout: archive
title: "AART Lab — Advanced Agent-Robotics Technology"
permalink: /experience/aart-lab/
author_profile: true
---

*Graduate Research Assistant · September 2024 – Present*  
*Carnegie Mellon University Robotics Institute · Pittsburgh, PA*  
*Advisor: [Prof. Katia Sycara](https://www.ri.cmu.edu/ri-faculty/katia-sycara/)*

[← Back to Experience]({{ site.baseurl }}/research/) &nbsp;·&nbsp; [TMLR paper (PDF)]({{ site.baseurl }}/files/TMLR_KGRL_paper.pdf)

---

## Lab Overview

The [Advanced Agent-Robotics Technology (AART) Lab](https://www.ri.cmu.edu/robotics-groups/advanced-agent-robotics-technology-lab/) works on multi-agent systems, human-robot interaction, and learning methods that combine deep learning with structured knowledge. My thesis work is on **neuro-symbolic reinforcement learning** for collaborative robotics, in collaboration with the **Honda Research Institute**.

---

## A Plug-in Knowledge-Graph Adapter for Reinforcement Learning

*First-author submission to TMLR (under review). With V. Tadiparthi, H. N. Mahjoub, Y. Xie, W. Kim, K. Sycara, S. Stepputtis. Honda Research Institute collaboration.*

<img src="{{ site.baseurl }}/images/projects/kgrl_high_level.png" alt="Plug-in KG adapter overview" style="max-width:100%;border-radius:6px;margin:1em 0;">

*The plug-in KG adapter. A static task knowledge graph is fused each step with a dynamic scene graph; a GCRN processes the merged graph; a recommender pools per-node features into a single `kg_feat` vector that's concatenated with whatever observation features the policy backbone produces. The backbone — CNN-MLP, SoftMoE-LSTM, GTrXL, PoliFormer, anything else — is left unmodified.*

### The Question

RL agents have to explore an enormous state space before any structure emerges in the reward signal. Humans cheat: when a chef learns a new recipe, the recipe specifies the ingredients and order — not the motor sequence or the distance to the cutting board. The chef *grounds* abstract transferable knowledge in a concrete environment.

Prior knowledge-graph-augmented RL methods take exactly the right shape of prior — symbolic, sparse, easy to author — but they tie that prior to one specific policy architecture (a relational attention module, a graph-attention navigation head, etc.). The same prior can't be reused as policy backbones evolve.

**My contribution is a backbone-agnostic *adapter*.** Same external interface across every policy architecture; backbone is treated as a black box; nothing about the backbone is modified.

### How the Adapter Works

<img src="{{ site.baseurl }}/images/projects/kgrl_adapter_generic.png" alt="Adapter detail" style="max-width:100%;border-radius:6px;margin:1em 0;">

Four-stage pipeline inside the adapter:

1. **Graph union.** Static task KG and per-step scene graph share the same node vocabulary by construction. Merge by element-wise max of their adjacency matrices — preserves all structural edges, adds situational edges this step
2. **GCRN.** A Graph Convolutional Recurrent Network maintains a per-node hidden state across timesteps. Lets the per-node memory survive even when an entity is momentarily off-screen or forgotten by the backbone's own recurrent state
3. **Recommender pooling.** A small MLP scores each node; a dense softmax (not top-K) attends across all nodes; `kg_feat` is the attention-weighted sum. Dense softmax keeps gradient flowing to every node every step, which stabilizes training versus sparse top-K selection
4. **Splice into backbone.** Concatenate `kg_feat` with the backbone's observation features just before the actor/critic heads. That's the entire interface

### Plugging Into Any Backbone

<img src="{{ site.baseurl }}/images/projects/kgrl_cnn_mlp.png" alt="Adapter + CNN-MLP" style="max-width:48%;border-radius:6px;margin:1em 0.5em 1em 0;display:inline-block;">
<img src="{{ site.baseurl }}/images/projects/kgrl_gtrxl.png" alt="Adapter + GTrXL" style="max-width:48%;border-radius:6px;margin:1em 0;display:inline-block;">

The same adapter (green branch) spliced into a CNN-MLP trunk (left, for Overcooked / MiniGrid) and a GTrXL trunk (right, for Craftax). In all four backbones tested — CNN-MLP, SoftMoE-LSTM, GTrXL, PoliFormer (frozen DINOv2-ViT) — splicing in is one concatenation plus a one-line re-dimensioning of the head's first layer.

### Knowledge & Scene Graph Construction

A second practical contribution: in every environment tested, the KG is **algorithmically generated** from a short specification (a list of named entities plus four to five relation templates such as `drops on collect`, `crafts to`, `contains target`). No hand-authoring.

- **Symbolic worlds** (Overcooked, MiniGrid, Craftax) — KG templates evaluated against simulator-exposed tables; SG read directly from simulator state via four or five geometric probes ("is the agent facing this object?", "is object A within distance r of object B?")
- **Open-world simulators** (AI-Habitat ObjectNav) — KG templates evaluated against statistics mined from a one-time pre-pass of the same visual perception pipeline used at training; SG built per step by running an **open-vocabulary detector** → **promptable segmentation** over the agent's RGB frame, then applying the same geometric probes

<img src="{{ site.baseurl }}/images/projects/kgrl_tomato_onion_kg.png" alt="Example KG for the Overcooked Tomato-Onion recipe" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Example: the knowledge graph for the Overcooked Tomato-Onion recipe. Entities: ingredients, tools, plates, goal. Relations: `pickup_from`, `place_in`, `serves_to`. Generated from the recipe spec, not authored by hand.*

### Results

Evaluated across **four environments × four backbones = 16 cells**. The adapter helps in every cell. Two different forms of improvement appear:

**Form 1: Same final policy, fewer steps to get there.** Single-agent Overcooked and MiniGrid — both adapted and un-adapted backbones eventually reach the same asymptotic reward; the adapter just gets there much faster.

<img src="{{ site.baseurl }}/images/projects/kgrl_overcooked_scaling.png" alt="Overcooked scaling — adapted vs un-adapted SoftMoE-LSTM" style="max-width:100%;border-radius:6px;margin:1em 0;">

*SoftMoE-LSTM with and without the adapter on single-agent Overcooked, swept across ingredient count. The gap between adapted and un-adapted widens monotonically with task complexity.*

Steps-to-threshold reductions on the hardest recipes:

- CNN-MLP: **~3× fewer steps**
- SoftMoE-LSTM: **~2.5× fewer steps**
- GTrXL: **~2× fewer steps**

<img src="{{ site.baseurl }}/images/projects/kgrl_simple_tasks.png" alt="Overcooked standard recipes" style="max-width:32%;border-radius:6px;margin:1em 0;display:inline-block;">
<img src="{{ site.baseurl }}/images/projects/kgrl_complex_tasks.png" alt="Overcooked challenge recipes" style="max-width:32%;border-radius:6px;margin:1em 0;display:inline-block;">
<img src="{{ site.baseurl }}/images/projects/kgrl_minigrid.png" alt="MiniGrid eight environments" style="max-width:32%;border-radius:6px;margin:1em 0;display:inline-block;">

*Cross-backbone gains. Left: Overcooked standard recipes. Middle: challenge recipes. Right: eight MiniGrid environments. Un-adapted baselines scale super-linearly with task complexity; adapted versions scale approximately linearly.*

**Form 2: Higher final policy under a fixed budget.** Two-agent Overcooked and Craftax — the un-adapted backbones don't reach the optimum within the available training budget; the adapter raises the final return rather than just the convergence speed.

<img src="{{ site.baseurl }}/images/projects/kgrl_craftax.png" alt="Craftax KG-RL, TKG-RL, GTrXL, GTrXL+KG" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Craftax over a 1B-environment-step budget. The un-adapted GTrXL backbone lags the published PPO and PPO-RNN baselines of Matthews et al. (2024). **GTrXL + adapter overtakes both and ends as the highest-reward method** on the benchmark.*

### Robustness to KG Corruption

Authoring KGs is fast *and* imperfect — the templates won't always produce a perfect graph. I corrupted the training-time KG in four ways (node removal, edge removal, spurious node insertion, spurious edge insertion) and measured steps-to-threshold on the 8-ingredient Overcooked recipes.

Two findings:

- Even at **50% node removal**, the adapter still beats the no-adapter baseline. The surviving structural prior is useful even when half the relevant entities are missing
- The adapter is **more robust to insertions than deletions** — the recommender learns to ignore irrelevant extra nodes, but can't recover information that isn't in the graph. Practical authoring rule: **over-include rather than under-include**

### Ablations

I ablated the two main internal pieces of the adapter:

- **No-SG** (only the static KG, no per-step scene graph): hurts most on Overcooked, where spatial layout of ingredients and tools is critical
- **Static GCN** (replace the recurrent GCRN with a static GCN): hurts most on MiniGrid memory and lock-room tasks, where per-node memory across steps is essential

Both pieces matter, for different reasons.

---

## Temporal Knowledge Graphs for RL (TKG-RL)

Extending the adapter to **temporal knowledge graphs** with **Time2Vec** and **Time-LSTM** encoders so the agent can reason over dynamic state evolution and history-dependent constraints. Time-stamped edges encode evolving partner roles, transient resource availability, and order-of-operation constraints. Already present as a separate trace in the Craftax results above; more to come.

---

## Tech Stack

JAX · JaxMARL · PyTorch · IPPO multi-agent PPO · Graph Convolutional Recurrent Networks · SoftMoE / GTrXL / PoliFormer (DINOv2-ViT) · Open-vocabulary detection (OWLv2) · Promptable segmentation (SAM2)

## Related Projects

- [Latent Strategies for Ad-Hoc Human-Robot Teaming]({{ site.baseurl }}/projects/ad-hoc-teaming/) — coordination on the same Overcooked-AI benchmark, accepted at the [CHAI Annual Workshop 2026]({{ site.baseurl }}/publications/)
- [Filter Distillation]({{ site.baseurl }}/projects/filter-distillation/) — safety-supervised policy training, complementary to KG-augmented training

---

[← Back to Experience]({{ site.baseurl }}/research/)
