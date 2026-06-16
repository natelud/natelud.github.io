---
layout: archive
title: "Estimating Unobservable Parameters for Offroad Autonomy"
permalink: /projects/offroad-params/
author_profile: true
---

*16-745 Optimal Control & Reinforcement Learning · Carnegie Mellon University · Spring 2025*

[← Back to all projects]({{ site.baseurl }}/projects/) &nbsp;·&nbsp; [Final paper (PDF)]({{ site.baseurl }}/files/OffroadParams_16-745_final_paper.pdf) &nbsp;·&nbsp; [Poster (PDF)]({{ site.baseurl }}/files/OffroadParams_16-745_poster.pdf) &nbsp;·&nbsp; *with Micah Nye, Nicholas Leone, and Shubhra Aich*

---

## Problem

Off-road autonomous driving is harder than on-road in ways classical control doesn't handle well: uneven terrain, inconsistent traction, payload variation, and mechanical wear all change vehicle dynamics — sometimes within a single rollout. **Model Predictive Path Integral (MPPI)** absorbs *some* model uncertainty through its stochastic sampling, but its rollouts still need a reasonable dynamics model underneath them.

Most off-road MPPI deployments use a **Kinematic Bicycle Model (KBM)** because it's cheap and easy to tune. The trouble is that KBMs ignore lateral slip and load transfer — exactly the effects that dominate off-road. A **Dynamic Bicycle Model (DBM)** captures those effects, but it depends on a long list of parameters (mass distribution, tire cornering stiffness, friction, drag, steering response) that are difficult to measure directly and drift over time.

We built an adaptive framework that pairs **DBM-based MPPI** with **Unscented Kalman Filter (UKF) parameter estimation**, so the dynamics model the controller rolls out against is continuously refined from logged state-control data instead of guessed and frozen.

## Approach

<img src="{{ site.baseurl }}/images/projects/offroad_dbm_fbd.png" alt="Dynamic Bicycle Model free-body diagram" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Single-track, rear-wheel-drive Dynamic Bicycle Model. State: $[p_x, p_y, v_x, v_y, \theta, \dot{\theta}, \delta]$. Captures lateral slip via a linear tire model and accounts for nonlinear steering response.*

### Dynamics: rear-wheel-drive single-track DBM

The DBM has the parameters $\{m, I_z, \ell_f, \ell_r, C_{\alpha f}, C_{\alpha r}, C_m, C_d, C_{rr}, K_s, \mathcal{M}_{so}, C_{so}\}$. We use a **linear tire model** for lateral force, a polynomial form for longitudinal force (engine throttle minus drag minus rolling resistance), and an explicit steering-dynamics term for the vehicle's nonlinear steering response. The full state derivative is integrated as the dynamics function inside both the UKF and the MPPI rollouts.

### Parameter estimation: offline UKF in log space

The UKF is the right tool here: it handles nonlinear dynamics without explicit linearization by propagating sigma points through the dynamics function, computing the unscented mean and covariance, and updating the parameter estimate against measured state evolution.

I augment the state vector with the DBM parameters (in **log space**, so estimates stay positive) and treat them as pseudo-states with random-walk dynamics. The training loop:

1. Run a 30-step DBM rollout from the current state under the recorded control sequence
2. Compute residuals between the rollout and the recorded reference trajectory at matching timestamps
3. UKF update on the parameter pseudo-states
4. Shift the initial state by one step and repeat

20 iterations of this cover the same 50-step horizon MPPI eventually uses for online rollouts, so the trained parameters are exactly what MPPI needs.

<img src="{{ site.baseurl }}/images/projects/offroad_params_over_trials.png" alt="Parameter convergence over training iterations" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Normalized parameter values across UKF iterations — convergence is visible within ~15 iterations and stays stable across diverse driving conditions.*

<img src="{{ site.baseurl }}/images/projects/offroad_dbm_rmse.png" alt="Rollout RMSE over UKF iterations" style="max-width:100%;border-radius:6px;margin:1em 0;">

*UKF rollout RMSE drops monotonically as parameter estimates improve.*

### Control: MPPI with learned costmaps

The MPPI cost function combines a euclidean distance-to-goal term, a control-effort regularizer, and **learned ego-centric costmaps** for traversability and speed. The cost maps come from MaxEntIRL on BEV feature maps built from a visual foundation model (RADIO / DINOv2), following Triest et al.'s prior work.

## Results

We compared three models on logged hardware data:

- **KBM** — the standard baseline previously used on the vehicle
- **Untrained DBM** — same dynamics structure, nominal pre-measured parameters
- **UKF-trained DBM (ours)** — same DBM, parameters learned from logged trajectories

### Rollout fidelity

<img src="{{ site.baseurl }}/images/projects/offroad_rollout1.png" alt="Trajectory rollout example 1" style="max-width:48%;border-radius:6px;margin:1em 0.5em 1em 0;display:inline-block;">
<img src="{{ site.baseurl }}/images/projects/offroad_rollout2.png" alt="Trajectory rollout example 2" style="max-width:48%;border-radius:6px;margin:1em 0;display:inline-block;">

*Two rollouts from a slalom maneuver on uneven terrain (sine-wave steering input → lateral slip + uncertain footing). KBM captures the gross trend; the untrained DBM improves through the turns; the UKF-trained DBM tracks the recorded trajectory most faithfully.*

**State RMSE on hardware rollouts** (lower is better):

| State        | KBM     | Untrained DBM | **UKF-trained DBM** |
|---|---|---|---|
| Full state   | 0.5575  | 0.4967         | **0.4139** |
| XY only      | 0.7870  | 0.7221         | **0.5960** |

### Control reconstruction

We then asked the inverse question: given the same goal and observed states, do the three MPPI controllers reproduce the actual control commands the vehicle issued?

<img src="{{ site.baseurl }}/images/projects/offroad_mppi_command.png" alt="MPPI commanded vs ground-truth steering and throttle" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Commanded steering and throttle from MPPI under each dynamics model, vs. the recorded ground-truth controls. All three have MPPI's characteristic sampling noise; the UKF-trained DBM tracks the underlying sine-wave steering pattern most closely.*

**Control RMSE:**

| Control   | KBM     | Untrained DBM | UKF-trained DBM |
|---|---|---|---|
| Steering  | 0.1883  | 0.1597         | **0.1372** |
| Throttle  | **0.1713** | 0.2384      | 0.2229     |
| Full      | **0.1800** | 0.2029      | 0.1851     |

The UKF-trained DBM is the **best steering match** by a clear margin. KBM still wins on throttle (and therefore squeaks ahead on the full control RMSE), which is consistent with the throttle dynamics being the part of the model where the DBM adds the most parameters relative to the KBM — and therefore where it's most sensitive to estimation quality. The lateral dynamics, which are what matter most for off-road safety, are where the trained DBM wins decisively.

### Online autonomy

<img src="{{ site.baseurl }}/images/projects/offroad_autonomy.jpg" alt="Full autonomy stack with learned costmaps and trained DBM" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Full autonomy stack running online: learned BEV traversability and speed costmaps generated from a visual foundation model, with MPPI rolling out the UKF-trained DBM to track a goal pose.*

## Tech Stack

Julia · MPPI (sampling-based MPC) · Unscented Kalman Filter · Dynamic Bicycle Model · MaxEntIRL · BEV feature maps · DINOv2 / RADIO foundation features

## My Contribution

This was a team project with Micah Nye, Nicholas Leone, and Shubhra Aich — all four of us contributed equally. I focused on the DBM derivation and the UKF parameter-estimation loop, including the log-space augmentation and the iterative rollout-residual update.

## Limitations & Future Work

Hardware deployment of the trained-DBM MPPI controller wasn't possible inside the project window, and there's no good public off-road simulator to evaluate the loop end-to-end in. The natural next steps are:

- **Real-time UKF on the vehicle** — same algorithm, but running online so the parameter estimate tracks payload changes, tire wear, and surface transitions as they happen
- **Closed-loop hardware deployment** of the offline-trained DBM inside MPPI to validate the simulation-fidelity gains carry over to real driving
- **Nonlinear tire model** (e.g. Pacejka) for the lateral force term — we used a linear tire model for simplicity, and that's likely the first thing to upgrade once parameter estimation is reliable

---

[← Back to all projects]({{ site.baseurl }}/projects/)
