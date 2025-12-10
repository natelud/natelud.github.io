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

**Status:** Submission in review for AAMAS 2026 - Collaboration with Honda Research Institute - **Advisor:** Dr. Katia Sycara

---

### Temporal Knowledge Graphs for RL (TKG-RL)

Building on my KG-RL framework, I'm exploring how temporal knowledge graphs can help robots reason about dynamic environments. This addresses the challenge of representing and utilizing time-varying relationships in collaborative robotics.

**Key Contributions:**
- **2.5x improvement** in sample efficiency over standard RL in enviroments with highly difficult temporal dynamics
- Time-stamped edges capturing evolving relationships
- LSTM-based encoding of temporal graph patterns
- Backpropagation Through Time (BPTT) for temporal graph updates

**Status:** Active development

---

## Past Research Projects

### Human Driver Modeling for Autonomous Vehicles
*CMU DRIVE Lab | RISS Fellowship 2023*

Developed computational models of human driver behavior to improve autonomous vehicle prediction and decision-making systems. This work addresses a critical challenge in mixed autonomy: understanding and anticipating human driver actions in complex traffic scenarios.

**Contributions:**
- Hierarchical learned risk-aware planning framework for human driving simulation
- Novel forward-looking Gaussian risk metric incorporating social pooling-based trajectory prediction
- Improved prediction accuracy in complex traffic scenarios
- Framework for integrating human models into AV decision-making
- Enhanced AV safety through better human behavior anticipation

**Follow-on Work:**
After completing the RISS Fellowship, I continued working with the DRIVE Lab to develop Control Barrier Function-inspired Voronoi cell navigation techniques that enable large numbers of agents in multi-agent systems to quickly compute safe controls under motion uncertainty.

**Publication:** ICRA 2024 - **Advisor:** Dr. John Dolan

---

### Dynamic Human-Robot Co-Manipulation
*BYU RAD Lab | 2022-2024*

Investigated control strategies for robots that physically collaborate with humans in manipulation tasks, where the robot and human partner simultaneously grasp and manipulate shared objects. This work focused on developing systems that enable safe force sharing between human and robot partners to accomplish dynamic collaborative goals such as cooperative swinging, throwing, and tossing of objects—applications relevant to assistive technologies and collaborative manufacturing.

**System Development:**
- Designed and implemented an omnidirectional mobile robotic base for physical human-robot co-manipulation tasks, including ROS software architecture and robust control algorithms for safe interaction
- Built a VR experimental platform with force/torque sensing that allowed human participants to see collaborative task goals overlaid on the real world through their VR headset, enabling controlled studies of human-human and human-robot interaction dynamics
- Developed adaptations to impedance control methods that enabled dynamic co-manipulation between humans and robots, including high-acceleration tasks such as jointly throwing objects

**Technical Approach:**
- Implemented compliant control algorithms using impedance control frameworks that allow the robot to behave like a programmable mechanical spring-damper system
- Developed real-time parameter adaptation algorithms that adjust robot stiffness and damping based on detected human partner force inputs
- Integrated 6-axis force-torque sensors operating at 1kHz+ update rates to measure interaction forces during dynamic manipulation
- Conducted stability analysis of coupled human-robot dynamics to ensure safe physical interaction during high-acceleration tasks

**Contributions:**
- Demonstrated successful dynamic co-manipulation (throwing, tossing) through careful tuning of impedance control parameters to balance responsiveness with stability
- Created VR-augmented experimental platform that enables systematic study of human-robot collaboration dynamics and serves as a training tool for operators
- Developed adaptive impedance control framework that accommodates significant variability in human partner forces and movement strategies
- Designed transparent control behaviors that communicate robot intent, improving operator trust and collaborative task performance

**Presentation:** UCUR 2023 Workshop - **Advisor:** Dr. Marc Killpack and Dr. John Salmon

---

### Automated Maize Stalk Stiffness Measurement
*BYU Crop Biomechanics Laboratory | Capstone Project 2023-2024*

Led a capstone research team developing an automated robotic system for measuring maize stalk mechanical properties. The system enables high-throughput measurement for agricultural research, helping plant breeders identify desirable genetic traits, and farmers idnetify fields that need additional help, or earlier harvest times.

**System Design:**
- Novel strain gauge sensor package for non-destructive stiffness testing
- Data processing algorithms to convert strain measurements into stiffness metrics
- Autonomous navigation and data collection control algorithms for robotic platform

**Results:**
- Reduced measurement time by over 500% compared to manual methods
- Improved measurement consistency and repeatability
- System deployed for ongoing agricultural research

**Publication:** MDPI Sensors 2025 - **Advisor:** Dr. Douglas Cook

---

### Industrial Robotic Automation
*Altitude AI | 2022-2023*

Developed robotic automation systems for meat packing and processing applications using industrial robotic arms.

**Key Projects:**

**LLM-Driven Robot Programming:**
- Developed a domain-specific programming language that enables large language models to generate executable code for industrial robots from natural language instructions
- Dramatically accelerated prototyping of robotic manipulation tasks
- Enabled rapid iteration on manipulation strategies without manual programming

**Pork Skinning Robotic System:**
- Full stack development of automated pork belly skin removal system
- Image segmentation algorithms for identifying skin patches on irregular surfaces
- Path planning and control algorithms for precision cutting with industrial robot arms
- Perception, planning, and control pipeline for autonomous operation

**Impact:** Demonstrated practical application of advanced AI and robotics techniques in challenging industrial environments with strict performance and safety requirements.

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

- **Honda Research Institute:** Neuro-symbolic approaches for robotic collaboration and student-teacher methods
- **CMU AART Lab:** Core research on KG-RL and TKG-RL methods
- **CMU DRIVE Lab:** Human-robot interaction in autonomous driving contexts and autonomus navigation
- **BYU RAD Lab:** Physical human-robot collaboration and control
- **BYU Crop Biomechanics Laboratory:** Agricultural robotics and automation
- **Altitude AI:** Industrial robotic automation

---

*I'm actively seeking PhD positions for Fall 2026 in labs working on physical human-robot interaction.*
