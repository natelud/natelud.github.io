---
layout: archive
title: "Research"
permalink: /research/
author_profile: true
---

My research focuses on developing intelligent robots that can effectively collaborate with humans in shared environments. I take a neuro-symbolic approach, combining the strengths of deep learning with structured knowledge representations to create more interpretable, sample-efficient, and robust robotic systems.

---

## Current Research Projects

### Knowledge Graph-Augmented Reinforcement Learning (KG-RL)

I'm developing methods that integrate knowledge graphs with deep reinforcement learning to improve robot learning in collaborative scenarios. By incorporating symbolic knowledge about tasks, objects, and human behaviors, robots can learn more efficiently and make more interpretable decisions.

**Key Contributions:**
- Novel architectures for integrating graph neural networks with RL policies
- Methods for leveraging prior task knowledge to accelerate learning
- **3x improvement** in sample efficiency compared to baseline deep RL
- Applications to collaborative cooking scenarios in Overcooked-AI
- Improved interpretability through graph-based decision explanations

**Technical Approach:**
- Structured representations of task elements, agent relationships, and environmental state using knowledge graphs
- Graph Neural Networks (GNNs) to encode knowledge graph structure
- Integration with actor-critic RL framework using attention mechanisms
- Dynamic graph updates based on observed interactions

**Technologies:** PyTorch, PyTorch Geometric, Stable-Baselines3, Overcooked-AI environment

**Status:** Preparing submission to AAMAS 2026 | Collaboration with Honda Research Institute

---

### Temporal Knowledge Graphs for RL (TKG-RL)

Building on the KG-RL framework, I'm exploring how temporal knowledge graphs can help robots reason about dynamic environments and predict human behaviors over time. This addresses the challenge of representing and utilizing time-varying relationships in collaborative robotics.

**Motivation:** Human collaborators don't just understand current relationships—they anticipate how situations will unfold. A robot working alongside a human should predict when the human will need ingredients, anticipate bottlenecks, and proactively adjust its behavior. TKG-RL enables this forward-looking reasoning.

**Key Innovations:**
- **2.5x improvement** in sample efficiency over standard RL
- Time-stamped edges capturing evolving relationships
- Variational Autoencoder (VAE) for trajectory prediction
- LSTM-based encoding of temporal patterns
- Integration with Model Predictive Path Integral (MPPI) control
- Backpropagation Through Time (BPTT) for temporal graph updates

**Current Focus:**
- Overcooked-AI environment with 100+ recipe configurations
- Real-time strategy inference and adaptation
- Successful prediction of human strategy changes
- Reduced collision rates in shared workspace scenarios

**Technologies:** PyTorch (VAE, LSTM), MPPI controllers, Flask backend for visualization

**Status:** Active development

---

## Past Research Projects

### Human Driver Modeling for Autonomous Vehicles
*CMU DRIVE Lab | RISS Fellowship 2023*

Developed computational models of human driver behavior to improve autonomous vehicle prediction and decision-making systems. This work addresses a critical challenge in mixed autonomy: understanding and anticipating human driver actions in complex traffic scenarios.

**Contributions:**
- Novel approach to characterizing individual driving styles
- Improved prediction accuracy in complex traffic scenarios
- Framework for integrating human models into AV decision-making
- Enhanced AV safety through better human behavior anticipation

**Impact:** Better prediction of human behavior enables reduced collision risk, more comfortable AV maneuvers, and improved handling of edge cases.

**Publication:** ICRA 2024 | **Advisor:** Prof. John Dolan

**Technologies:** Machine learning for driver classification, probabilistic trajectory prediction, CARLA simulation, ROS-based AV stack

---

### Dynamic Human-Robot Co-Manipulation
*BYU RAD Lab | 2020-2023*

Investigated control strategies for robots that physically collaborate with humans in manipulation tasks. Focused on developing systems that can safely share forces with human partners while accomplishing shared goals, with applications to assistive technologies and collaborative manufacturing.

**Technical Approach:**
- Compliant control for safe force sharing using admittance/impedance frameworks
- Real-time adaptation to human partner inputs
- Force-torque sensing integration (6-axis sensors, 1kHz+ update rates)
- Stability analysis for physical interaction

**Applications:**
- Assistive manipulation for mobility-impaired users
- Shared control of heavy or awkward objects
- Collaborative assembly in manufacturing contexts

**Key Insights:**
- Human variability requires adaptive control approaches
- Transparency in robot behavior builds trust
- Force feedback is critical for natural collaboration
- Safety must be designed into fundamental control laws

**Advisor:** Prof. Marc Killpack | **Lab:** Robotic and Autonomous Dynamics (RAD) Lab

---

### Automated Agricultural Phenotyping
*BYU Capstone Project | 2023*

Designed and built an automated robotic system for measuring maize stalk mechanical properties. The system enables high-throughput phenotyping for agricultural research, helping plant breeders identify desirable genetic traits.

**System Design:**
- Custom end-effector for non-destructive stiffness testing
- Computer vision for stalk identification and positioning
- Adaptive gripping for varying stalk sizes
- Real-time data collection and processing pipeline

**Results:**
- Reduced measurement time by 80% compared to manual methods
- Improved measurement consistency and repeatability
- System deployed for ongoing agricultural research

**Publication:** Under review at IEEE Sensors Journal

**Technologies:** Custom manipulator design, ROS, computer vision (OpenCV), force control systems

---

## Research Interests

I'm broadly interested in:

- **Physical Human-Robot Interaction:** Safe and natural interaction between physical embodied systems and humans
- **Multi-agent Systems:** Effective coordination between robots and humans in shared tasks
- **Assistive Robotics:** Robots that help people in everyday life
- **Safety-critical Control:** Guaranteeing safe behavior in uncertain, dynamic environments with humans
- **Learning from Demonstrations:** Using human demonstrations to train robots to behave more optimally and interpretably
- **Neuro-symbolic AI:** Combining learning with structured knowledge for robustness and interpretability

## Collaborations

- **Honda Research Institute:** Neuro-symbolic approaches for robotic collaboration
- **CMU AART Lab:** Core research on KG-RL and TKG-RL methods
- **CMU DRIVE Lab:** Human-robot interaction in autonomous driving contexts
- **BYU RAD Lab:** Physical human-robot collaboration and control

---

*I'm actively seeking PhD positions for Fall 2026 in labs working on physical human-robot interaction. If you're interested in collaboration or have questions about my research, please reach out!*
