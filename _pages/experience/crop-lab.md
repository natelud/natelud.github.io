---
layout: archive
title: "BYU Crop Biomechanics Laboratory"
permalink: /experience/crop-lab/
author_profile: true
---

*Capstone Research Team Lead · September 2023 – April 2024*  
*Brigham Young University · Provo, UT*  
*Advisor: Prof. Douglas Cook*

[← Back to Experience]({{ site.baseurl }}/research/) &nbsp;·&nbsp; [MDPI Sensors 2025 paper](https://doi.org/10.3390/s25216561)

---

## Lab Overview

The BYU Crop Biomechanics Laboratory studies the mechanical properties of crop plants — relevant to plant breeding, harvest timing, and farm-scale yield. I led the senior capstone team that designed and built an automated robotic system for measuring **maize stalk stiffness** in the field, and co-authored a sensor-design paper out of the same lab that establishes a new force-and-position sensor concept.

---

## Force-Position Sensor Paper (Co-author, MDPI Sensors 2025)

<img src="{{ site.baseurl }}/images/projects/sensor_graphical_abstract.png" alt="Graphical abstract — cantilever-beam force-position sensor" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Graphical abstract from* [Noh, Smith, Shamo, Porter, Steele, **Ludlow**, Hall, Holst, Williams, Cook. "Measurement of Force and Position Using a Cantilever Beam and Multiple Strain Gauges," MDPI Sensors 2025](https://doi.org/10.3390/s25216561). [Paper PDF]({{ site.baseurl }}/files/MDPI_Sensors_2025_paper.pdf).

### The Concept

Force and position sensing are usually treated as two separate problems. Conventional load cells assume the force is applied at a known location — apply it somewhere else and your reading is wrong. Tactile arrays can give you both, but they're delicate, low-range, and discrete (the position comes from "which cell lit up," not a continuous coordinate).

We characterize a sensor concept that gives **simultaneous, continuous measurements of force magnitude and force application position** along a beam, using nothing more than two (or more) strain gauges bonded to a cantilever.

<img src="{{ site.baseurl }}/images/projects/sensor_diagram.png" alt="Generic sensor architecture" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Generic sensor architecture: a cantilever beam with two strain gauges bonded at distinct axial locations near the base. The strain at each gauge depends on both the magnitude of the applied force $F$ and its position $x$ along the beam, so two readings let you solve for both quantities.*

### Why Two Gauges Near the Base Works

Each strain-gauge reading on a cantilever under a point load is a linear function of $F$ and $F \cdot x$ (force times its moment arm). With two gauges at distinct axial positions $d_1$ and $d_2$, the readings form a 2×2 system you can invert algebraically for $F$ and $x$.

Placing both gauges **near the base** of the beam has two practical consequences:

- All the wiring and electronics live in one compact region, leaving the rest of the cantilever as bare sensing surface — durable, easy to seal, and easy to drop into compact applications like robotic grippers
- The same sensor measures **forces in two axes** (most existing single-axis-only force-position sensors give up an axis to get position)

The paper derives the governing equations, characterizes the design trade-off between force range and position range (increasing one shrinks the other), and gives a complete recipe for designing a sensor for a target measurement window.

### Prototypes and Validation

We designed, built, and characterized **three prototypes** spanning a range of beam geometries and gauge spacings:

<img src="{{ site.baseurl }}/images/projects/sensor_prototypes_cad.jpg" alt="Prototype CAD models" style="max-width:100%;border-radius:6px;margin:1em 0;">

<img src="{{ site.baseurl }}/images/projects/sensor_prototype_c.jpg" alt="Prototype C hardware" style="max-width:60%;border-radius:6px;margin:1em 0;">

*Top: CAD models for the three prototypes (different beam length, cross-section, and gauge-spacing combinations). Bottom: Prototype C hardware.*

<img src="{{ site.baseurl }}/images/projects/sensor_data_collection.jpg" alt="Data collection rig — Prototype B" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Calibration rig: a controlled load is applied at known positions along Prototype B; ground-truth force and position are logged for comparison against the sensor's predictions.*

### Results

<img src="{{ site.baseurl }}/images/projects/sensor_prediction_results.jpg" alt="Predicted vs measured force and position" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Predicted vs. measured force and position across the calibrated range. Highly linear; close to the unit-slope reference line across all three prototypes.*

The headline numbers from the paper:

- **Average percent error: ~1.71%**
- **Highly linear** response across the calibrated range, for both force and position
- **Force accuracy is dependent on applied load but insensitive to where the load is applied** — a major usability win over conventional load cells
- **Position accuracy depends on applied load and only weakly on position** — i.e., the sensor is most accurate at moderate loads and degrades gracefully at the extremes

<img src="{{ site.baseurl }}/images/projects/sensor_error_load.png" alt="Error as a function of applied load" style="max-width:48%;border-radius:6px;margin:1em 0.5em 1em 0;display:inline-block;">
<img src="{{ site.baseurl }}/images/projects/sensor_error_position.png" alt="Error as a function of applied position" style="max-width:48%;border-radius:6px;margin:1em 0;display:inline-block;">

*Empirical error characterization. Left: error as a function of applied load. Right: error as a function of applied position. The two together let designers pick a working window appropriate for their application.*

### What This Enables

The compact electronics and bare sensing surface make the design well-suited for:

- **Robotic end-effectors** that need to know both *what* force they're applying and *where* on the finger contact occurred
- **Field-deployable instrumentation** where a delicate tactile array would be destroyed by abrasion (the application that motivated the work in our lab)
- **Distributed load sensing** where adding more gauges along the beam extends the spatial range without adding more electronics

### My Contribution

Within a 10-author team, I contributed to prototype design and characterization, data-acquisition software for the calibration rig, and the analysis pipeline that converts raw strain readings into the reported force and position estimates. NSF-supported.

---

## R.E.A.P.E.R / Rodney — Automated Maize Stalk Stiffness Measurement (Capstone)

I led a 6-person senior capstone team building **Rodney**, a sensor for rapidly measuring maize stalk stiffness and strength **non-destructively, in the field, at production scale**. The traditional measurement method is manual, slow, and inconsistent. Rodney mounts on an autonomous agricultural robot (Farm-ng's Amiga) and measures stalks continuously as the robot drives between rows.

**System design:**

- **Mechanical** — Cylindrical sensing rod with two sets of strain gauges (closely related to the cantilever concept above), mechanical guarding, mounting hardware for the agricultural robot
- **Electrical** — Wheatstone bridges amplify strain signals; ruggedized for outdoor conditions; integrated with the robot's power system
- **Software (FIELDAQ)** — Python + Kivy GUI on Raspberry Pi for in-field data acquisition. Multi-screen touchscreen interface for plot configuration, live signal monitoring, calibration, and trial logging

<img src="{{ site.baseurl }}/images/projects/fieldaq_software.png" alt="FIELDAQ field data acquisition GUI" style="max-width:100%;border-radius:6px;margin:1em 0;">

*FIELDAQ — the Python/Kivy GUI for in-field data acquisition.*

**My specific contributions:**

- Designed the strain-gauge sensor package and the data-processing algorithms that convert strain measurements into stiffness metrics
- Implemented autonomous navigation and data-collection control algorithms for the robotic platform
- Drove integration testing and field calibration across three prototype iterations

**Results:**

<img src="{{ site.baseurl }}/images/projects/reaper_stalk_pass.jpg" alt="Force vs time trace from a sensor pass" style="max-width:100%;border-radius:6px;margin:1em 0;">

*Normal force, displacement, and stalk-deflection traces logged during a single sensor pass — the two force peaks correspond to two stalks contacted in sequence as the robot drives down the row.*

- **Reduced measurement time by over 500%** compared to manual methods
- Improved consistency and repeatability across operators and conditions
- System deployed for ongoing agricultural research at BYU

## Tech Stack

Python · Kivy GUI · Raspberry Pi · Strain-gauge sensor design · Wheatstone-bridge instrumentation · Field robotics integration · Farm-ng Amiga platform · LaTeX (paper)

---

[← Back to Experience]({{ site.baseurl }}/research/)
