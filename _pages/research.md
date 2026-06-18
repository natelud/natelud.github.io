---
layout: archive
title: "Experience"
permalink: /research/
author_profile: true
---

I am a robotics software engineer who started in mechanical design and grew into full-stack autonomy. Over two years in industry I built perception, planning, and control software for industrial robot arms, including a domain-specific language that let LLMs program manipulators from natural language. Across four years of research I have published in reinforcement learning, autonomous driving, and physical human-robot collaboration, and I now want to bring that range to a team shipping real-world robotics.

---

## Industry

### Altitude AI
*Robotics Software Engineer · May 2022 – September 2023 · Salt Lake City, UT*

Worked end-to-end across the robotics stack — perception, path planning, control, and real-time execution — on industrial-arm automation for the **meat-packing and processing** industry. Sensor integration, motion generation, on-site deployment; bridging research methods and deployable factory software.

**Highlights:**

- **Automated meat processing — pork-belly skin removal (team lead).** Helped lead the team building the full perception → planning → control pipeline for robotic pork-belly skin removal, coordinating industrial arms with electric cutting tools to operate on variable, deformable workpieces in a production setting. Owned key parts of the perception stack (image segmentation for skin patches on irregular surfaces) and contributed to the cutting-path planner and the real-time control loop. Also ran the **on-site deployment tests** that validated the system under real production conditions.
- **Natural-language robot programming (LLM-to-Robot DSL).** Designed a domain-specific language that let large language models generate executable code for industrial arms from natural-language instructions. The DSL enabled customers to rapidly create and modify custom tasks on our hardware platforms without writing robot code themselves, and dramatically accelerated internal manipulation-task prototyping.
- **Pick-and-place deployment at a customer facility.** Contributed to a pick-and-place robotic system that we deployed at a customer's production site, integrating perception (object detection and pose estimation), motion planning, and gripper control into a working factory cell. The deployment work covered everything from on-site commissioning to dialing in cycle-time targets against the customer's existing line.

**Stack:** Python · C++ · ROS · OpenCV · Industrial robotic arms (Yaskawa, UR, xArm) · LLM tooling · On-site production deployment

*See [CV]({{ site.baseurl }}/cv/) for additional context.*

---

## Research

### [AART Lab — Advanced Agent-Robotics Technology]({{ site.baseurl }}/experience/aart-lab/)
*Graduate Research Assistant · September 2024 – Present · Carnegie Mellon University · Advisor: Prof. Katia Sycara*

<img src="{{ site.baseurl }}/images/projects/kgrl_high_level.png" alt="Plug-in KG adapter overview" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

Designed a **backbone-agnostic plug-in knowledge-graph adapter** for reinforcement learning. The adapter merges a static task KG with a dynamic scene graph each step, processes the result with a GCRN, and emits a fixed-width feature vector that splices into *any* policy backbone (CNN-MLP, SoftMoE-LSTM, GTrXL, PoliFormer) with one line of code. Validated across **4 environments × 4 backbones**: up to **3× fewer steps** to optimum on Overcooked / MiniGrid, and a higher final policy than published baselines on Craftax. Robust to 50% KG corruption. Collaboration with **Honda Research Institute**.

**Stack:** JAX / JaxMARL · PyTorch · GCRN · IPPO · OWLv2 / SAM2 (open-world scene graphs)
&nbsp; · &nbsp; *First-author submission under review at TMLR*
&nbsp; · &nbsp; [**Read full write-up →**]({{ site.baseurl }}/experience/aart-lab/) &nbsp;·&nbsp; [Paper (PDF)]({{ site.baseurl }}/files/TMLR_KGRL_paper.pdf)

---

### [DRIVE Lab — Driverless Intelligent Vehicles]({{ site.baseurl }}/experience/drive-lab/)
*Research Assistant (Aug 2023 – Aug 2024) · RISS Fellow (Summer 2023) · Carnegie Mellon University · Advisor: Prof. John M. Dolan*

<img src="{{ site.baseurl }}/images/projects/riss_graphical_abstract.jpg" alt="Hierarchical risk-aware planning" style="max-width:100%;border-radius:6px;margin:0.5em 0;">

Designed a **hierarchical risk-aware behavioral model** for simulating human drivers in AV testing, introducing a forward-looking Gaussian risk metric with social-pooling trajectory prediction. **First-author at ICRA 2024.** Follow-on work: [Risk-aware Voronoi cell navigation]({{ site.baseurl }}/experience/wbvc/) inspired by Control Barrier Functions for decentralized, many-agent safe navigation around dynamic obstacles under motion uncertainty.

**Stack:** PyTorch · Convolutional Social Pooling · NGSIM · Risk-aware planning · CBF / Voronoi
&nbsp; · &nbsp; [ICRA 2024 paper](https://doi.org/10.1109/ICRA57147.2024.10610354)
&nbsp; · &nbsp; [**Read full write-up →**]({{ site.baseurl }}/experience/drive-lab/)

---

### [RAD Lab — Robotics And Dynamics Laboratory]({{ site.baseurl }}/experience/rad-lab/)
*Undergraduate Research Assistant · Feb 2022 – Jun 2024 · Brigham Young University · Advisors: Prof. Marc Killpack and Prof. John Salmon*

Developed dynamic control methods for **physical human-robot co-manipulation** of large objects (cooperative swinging, throwing, tossing). Built the full hardware/software architecture for an omnidirectional mobile base (>3 m/s, >68 kg payload) and a VR experimental platform with force sensing for analyzing human-human interaction dynamics during cooperative manipulation.

**Stack:** ROS · MuJoCo · Impedance control · 6-axis F/T sensing · VR experimental platforms · Mocap
&nbsp; · &nbsp; UCUR 2023 workshop presentation
&nbsp; · &nbsp; [**Read full write-up →**]({{ site.baseurl }}/experience/rad-lab/)

---

### [Crop Biomechanics Laboratory]({{ site.baseurl }}/experience/crop-lab/)
*Capstone Team Lead · September 2023 – April 2024 · Brigham Young University · Advisor: Prof. Douglas Cook*

Led a 6-person senior capstone team building an autonomous robotic system to measure **maize-stalk stiffness** in the field. Designed a novel strain-gauge sensor package, data-processing algorithms, and autonomous navigation control. Reduced measurement time by over 500% vs. manual methods; calibration R² > 0.99 across three prototypes. Co-author on a related sensor-design paper.

**Stack:** Python · Kivy · Raspberry Pi · Strain-gauge instrumentation · Farm-ng Amiga
&nbsp; · &nbsp; [MDPI Sensors 2025](https://doi.org/10.3390/s25216561)
&nbsp; · &nbsp; [**Read full write-up →**]({{ site.baseurl }}/experience/crop-lab/)

---

## Research Themes

- **Physical Human-Robot Interaction** — Safe, natural interaction between embodied systems and people (RAD Lab, AART Lab)
- **Multi-Agent Systems** — Coordination between robots and humans in shared tasks (AART Lab, DRIVE Lab, undergrad Voronoi work)
- **Safety-Critical Control** — Guaranteeing safe behavior under uncertainty (DRIVE Lab Voronoi, course work on CBF/HJR, filter distillation)
- **Learning from Demonstrations** — Using human demos to bias policies toward human-compatible behavior (AART Lab, Ad-Hoc Teaming)
- **Neuro-Symbolic AI** — Combining learning with structured knowledge for robustness and interpretability (AART Lab thesis work)

## Collaborations

- **Honda Research Institute** — Neuro-symbolic methods for collaborative robotics
- **CMU AART Lab** — KG-RL / TKG-RL methods
- **CMU DRIVE Lab** — Human driver modeling and autonomous navigation
- **BYU RAD Lab** — Physical human-robot collaboration and control
- **BYU Crop Biomechanics Lab** — Agricultural robotics and sensor design
- **Altitude AI** — Industrial robotic automation

---

*Open to robotics engineering roles starting Summer 2026 — reach out via [email](mailto:nludlow@andrew.cmu.edu) or [LinkedIn](https://www.linkedin.com/in/nathan-ludlow).*
