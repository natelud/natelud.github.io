---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---

A selection of course and independent projects that complement my [experience]({{ site.baseurl }}/research/). They span optimal control, reinforcement learning, computer vision, safety-critical autonomy, multi-agent systems, and mechanical design — together they form the technical foundation for my current work on neuro-symbolic robotics for human-robot collaboration.

**Click any project title for a full write-up with the final paper / presentation, results, and demos.**

---

## Graduate Coursework — Carnegie Mellon Robotics Institute

### [Learning Better Policies Through Safety Filter Distillation]({{ site.baseurl }}/projects/filter-distillation/)
*16-886 Embodied AI Safety · Spring 2026*

<img src="{{ site.baseurl }}/images/projects/anysafe_main.png" alt="Safety filter distillation framing" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

Rather than bolting a Hamilton-Jacobi safety filter onto a policy at runtime, I **distill the filter into the policy during training** via a selective behavioral-cloning loss gated on safety-critical states. The result: a policy that reaches the goal **95.7%** of the time at **98.1%** safe on hard Dubins-car scenes — beating the next-best learning baseline by 59 percentage points on goal rate.

**Stack:** PyTorch · SAC · Analytic HJ reachability · Selective BC
&nbsp; · &nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/FilterDistillation_16-886_final_paper.pdf)

---

### [Disentangled Latent World Models for Mobile Robots]({{ site.baseurl }}/projects/world-models/)
*16-761 Mobile Robots · Spring 2026*

<img src="{{ site.baseurl }}/images/projects/disentangled_wm.gif" alt="Disentangled world model rollout" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

Investigated whether **disentangled** world-model representations reduce the cost of model-based planning by focusing predictions on the parts of the scene that actually change. Compared dense JEPA-style prediction against a disentangled formulation built on **LeWorldModel**.

**Stack:** PyTorch · JEPA / LeWorldModel · Learned latent dynamics
&nbsp; · &nbsp; [Final presentation (PDF)]({{ site.baseurl }}/files/Disentangled_WorldModels_16-761_final_presentation.pdf)

---

### [Latent Strategies for Ad-Hoc Human-Robot Teaming]({{ site.baseurl }}/projects/ad-hoc-teaming/)
*16-867 Human-Robot Interaction · Fall 2025*

<img src="{{ site.baseurl }}/images/projects/teaming_architecture.png" alt="Joint strategy architecture" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

A POMDP formulation of human-robot teaming where the partner's policy is an **unobservable latent variable**. A VAE learns a joint latent strategy space from 480 human-human Overcooked trajectories; an LSTM belief tracker infers strategy online from partial behavior; an MPPI scorer selects the best robot action over sampled trajectory pairs. Beats PPO-FCP and BC baselines by **1.2–2.2×** under non-stationary partners and outperforms BC-BC pairings by **+72.5% / +64.6%** against adaptive proxies. **Accepted at the [CHAI Annual Workshop 2026]({{ site.baseurl }}/publications/)** as "Strategy Search for Human-Robot Teaming."

**Stack:** PyTorch · VAE · LSTM belief tracking · MPPI · Overcooked-AI
&nbsp; · &nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/AdHocTeaming_16-867_final_paper.pdf) &nbsp; · &nbsp; [CHAI poster (PDF)]({{ site.baseurl }}/files/AdHocTeaming_CHAI2026_poster.pdf)

---

### [Histogram-Based Defogging of SPAD LiDAR]({{ site.baseurl }}/projects/spad-defog/)
*16-823 Physics-Based Methods in Computer Vision · Spring 2025*

<img src="{{ site.baseurl }}/images/projects/spad_header_result.png" alt="SPAD defogging result" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

Physics-informed defogging for **905 nm SPAD LiDAR** under spatially-varying fog. Pixels are grouped into 8×8 spatial bins; an **Expectation-Maximization** procedure fits a per-bin gamma-style fog model; a **Generalized Likelihood Ratio Test** recovers object depth from the cleaned histogram. Validated in **Mitsuba** transient rendering on a 40 m pedestrian-braking scenario — recovers depth within ~3 m even in dense fog ($\mu_{\text{ext}}=\mu_s=0.8$). No learned priors, real-time-compatible.

**Stack:** Python · Mitsuba + Mitransient · EM · GLRT · SPAD sensor modeling
&nbsp; · &nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/SPAD_Defog_16-823_final_paper.pdf)

---

### [Estimating Unobservable Parameters for Offroad Autonomy]({{ site.baseurl }}/projects/offroad-params/)
*16-745 Optimal Control & Reinforcement Learning · Spring 2025*

<img src="{{ site.baseurl }}/images/projects/offroad_autonomy.jpg" alt="Full off-road autonomy stack — 8 MPPI trajectories on learned BEV costmaps" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

Coupled **MPPI control** with **Unscented Kalman Filter** parameter estimation for off-road autonomous driving. The UKF refines the **Dynamic Bicycle Model** parameters (mass, tire cornering stiffness, friction, steering dynamics) from logged state-control data; MPPI then rolls out against the trained DBM with a **learned BEV costmap** from MaxEntIRL on DINOv2 / RADIO features. Beat both KBM and untrained-DBM baselines on hardware rollouts — **state RMSE 0.41 vs. 0.56 (KBM)** and steering RMSE 0.137 vs. 0.188.

**Stack:** Julia · MPPI · UKF · Dynamic Bicycle Model · MaxEntIRL · BEV foundation features
&nbsp; · &nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/OffroadParams_16-745_final_paper.pdf) &nbsp; · &nbsp; [Poster (PDF)]({{ site.baseurl }}/files/OffroadParams_16-745_poster.pdf)

---

### [Evaluating Pre-trained Visual Representations with Diffusion Policies]({{ site.baseurl }}/projects/diffusion-pvr/)
*16-831 Introduction to Robot Learning · Fall 2024*

<img src="{{ site.baseurl }}/images/projects/diffusion_policy_overview.png" alt="Diffusion policy with PVR encoder" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

Systematic evaluation of frozen **pre-trained visual representations** (DINOv2, R3M, ImageNet) as drop-in encoders for **diffusion-based visuomotor policies** on the Push-T manipulation benchmark. Found that frozen PVRs **consistently underperform** end-to-end trained ResNet encoders — a clean negative result showing that diffusion policies depend on encoder features standard PVRs don't capture.

**Stack:** PyTorch · DDPM · Diffusion Policy · DINOv2 · R3M · Transformers
&nbsp; · &nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/RobotLearning_16-831_final_report.pdf)

---

## Undergraduate Projects — Brigham Young University

### [Risk-Aware Buffered Voronoi Multi-Agent Navigation]({{ site.baseurl }}/projects/voronoi-arms/)
*ME EN 537 Robotics · Fall 2023*

<img src="{{ site.baseurl }}/images/projects/voronoi_arms_RAVC.png" alt="Risk-aware Voronoi cells" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

**Risk-aware Weighted Buffered Voronoi Tessellation** for decentralized multi-agent collision-free navigation of dynamic robotic arms. Computed-torque control, IK, RNE dynamics, multi-trial validation. This formulation seeded my [DRIVE Lab follow-on]({{ site.baseurl }}/experience/wbvc/) at CMU — a CBF-based generalization with formal safety guarantees, demonstrated on up to 25 agents.

**Stack:** Python · Robot dynamics · Voronoi partitioning · Decentralized control
&nbsp; · &nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/Voronoi_Arms_ME537_final_paper.pdf)

---

## Coursework Topics

Beyond the projects above, my graduate (CMU MSR) and undergraduate (BYU ME + CS minor) coursework has covered the following technical areas in depth:

**Optimal Control**
&nbsp; LQR · iLQR · DDP · Model Predictive Control · **MPPI** (sampling-based MPC) · trajectory optimization · trajectory optimization through contact / hybrid dynamics · iterative learning control · convex optimization · nonlinear programming

**Safety-Critical Control**
&nbsp; **Control Barrier Functions (CBF)** · **Hamilton-Jacobi reachability (HJR)** · reachability shields · safety filters · conformal prediction · Lyapunov stability analysis

**Planning**
&nbsp; A\* · **RRT / RRT\*** · PRM · sampling-based motion planning · BFS / Dijkstra on grid maps · informed search · belief-space planning · POMDP solvers

**Reinforcement & Imitation Learning**
&nbsp; **PPO** · **SAC** · DQN · REINFORCE · A2C · **Behavioral Cloning** · **DAgger** · model-based RL · **world models** (Dreamer, JEPA, LeWM) · multi-agent RL · **IPPO** · fictitious co-play · ad-hoc teaming · knowledge-graph-augmented RL

**State Estimation**
&nbsp; Kalman filtering · Extended Kalman Filter (EKF) · **Unscented Kalman Filter (UKF)** · particle filters · sensor fusion · belief tracking · POMDPs

**Robotics Fundamentals**
&nbsp; Forward / inverse kinematics · **recursive Newton-Euler dynamics** · Lagrangian mechanics · **impedance / admittance control** · computed-torque control · PID · screw theory · continuum manipulator kinematics

**Perception & Computer Vision**
&nbsp; Hough transform · semantic segmentation · image stitching · QR code detection · CNNs for classification · **SPAD LiDAR** modeling · physics-based rendering (Mitsuba3) · scene reconstruction (Blender) · **pre-trained visual representations** (DINOv2, R3M)

**Deep Learning**
&nbsp; CNNs · RNNs / LSTMs · Transformers · VAEs · **diffusion models / DDPM** · Graph Neural Networks (GCRN, GAT) · self-supervised learning · representation learning

**Multi-Agent & HRI**
&nbsp; **Voronoi tessellation** · buffered Voronoi cells · risk-aware planning · intent inference · latent-strategy modeling · learning from demonstration · partner-aware policies

**Math & Foundations**
&nbsp; Linear algebra · numerical methods · stochastic processes · information theory · optimization theory · differential geometry (for control)

**Mechanical / Embedded**
&nbsp; Compliant mechanisms · mechanism design · strain-gauge instrumentation · 6-axis force-torque sensing · real-time data acquisition · Raspberry Pi / microcontroller embedded systems · sensor calibration

**Software Engineering**
&nbsp; **ROS / ROS 2** · MuJoCo · PyBullet · Hydra · WandB · Docker · Git · Linux systems

---

*See the [Experience]({{ site.baseurl }}/research/) page for my industry and lab work at AART, DRIVE, RAD, CROP Biomechanics, and Altitude AI.*
