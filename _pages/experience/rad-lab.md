---
layout: archive
title: "BYU RAD Lab — Robotics And Dynamics Laboratory"
permalink: /experience/rad-lab/
author_profile: true
---

*Undergraduate Research Assistant · February 2022 – June 2023, August 2023 – June 2024*  
*Brigham Young University · Provo, UT*  
*Advisors: [Prof. Marc Killpack](https://www.me.byu.edu/directory/marc-killpack) and Prof. John Salmon*

[← Back to Experience]({{ site.baseurl }}/research/)

---

## Lab Overview

The [BYU RAD Lab](http://radlab.byu.edu/) works on physical human-robot interaction, compliant control, and mobile manipulation. I spent three years building hardware, control software, and experimental infrastructure for **dynamic human-robot co-manipulation** — robots that can safely share physical workspace with humans for tasks like cooperative carrying, swinging, and throwing of large objects.

## Dynamic Human-Robot Co-Manipulation

<video controls preload="metadata" style="max-width:100%;border-radius:6px;margin:1em 0;">
  <source src="{{ site.baseurl }}/images/projects/rad_dynamic_throwing.mp4" type="video/mp4">
</video>

*Dynamic throwing — humans and robots co-manipulate an object through a high-acceleration release. Tasks like this are where impedance-control parameter tuning matters most.*

I worked on the full stack of physical HRI: from low-level impedance control through experimental design and data collection.

**Control development:**

- Implemented compliant control via impedance-control frameworks — robot behaves like a programmable spring-damper system, parameterized by stiffness and damping matrices
- Developed real-time parameter adaptation that adjusts robot stiffness/damping based on detected human partner forces
- Integrated 6-axis force-torque sensors at 1 kHz+ to measure interaction forces during dynamic manipulation
- Conducted stability analysis of the coupled human-robot dynamics for safe high-acceleration tasks
- Demonstrated successful dynamic co-manipulation (throwing, tossing) through careful tuning of impedance parameters to balance responsiveness with stability

**Experimental platform:**

- Built a **VR experimental platform** with force/torque sensing that lets participants see collaborative task goals overlaid on the real world through a VR headset, enabling controlled studies of human-human and human-robot interaction dynamics
- Designed and ran human-subjects studies (IRB-approved) on dynamic throwing tasks — both human-human (baseline) and human-robot conditions, with motion-capture and audio recording

## Mobile Base for Physical HRI

<video controls preload="metadata" style="max-width:100%;border-radius:6px;margin:1em 0;">
  <source src="{{ site.baseurl }}/images/projects/rad_mobile_base_outdoor.mp4" type="video/mp4">
</video>

*Outdoor high-speed mobile-base testing.*

Designed and implemented an **omnidirectional mobile robotic base** for physical HRI in search-and-rescue contexts. The base targets a gap in available hardware: high-payload, low-cost mobile platforms with enough mobility to perform effective co-manipulation alongside a human partner.

**System highlights:**

- Four individually steerable caster wheels for omnidirectional motion (matches human partner's motion in any direction)
- Casters mounted on **differential rocker arms** to traverse small-to-mid-sized obstacles — important for rough-terrain search-and-rescue
- Pneumatically actuated soft continuum arm mounted on a rotating turret for object interaction
- Peak speed: **>3 m/s (6.7 mph)** in any direction
- Payload: **>68 kg (150 lb)**
- Battery runtime: over an hour of continuous operation

**Publication:** [Mobile Base for Physical Human-Robot Interaction and Co-manipulation](https://our.utah.edu/ucur/mobile-base-for-physical-human-robot-interaction-and-co-manipulation/) — UCUR 2023 workshop presentation (first author with Killpack and Salmon).

## Baloo MuJoCo Simulator

<img src="{{ site.baseurl }}/images/projects/baloo_mujoco_sim.png" alt="Baloo humanoid MuJoCo simulation" style="max-width:100%;border-radius:6px;margin:1em 0;">

Contributed to the MuJoCo-based simulator for Baloo, the lab's humanoid platform. Includes kinematic models, joint angle estimators, and motion-profile servo plugins — foundational infrastructure for control research before deploying on hardware.

## Tech Stack

ROS · MuJoCo · Impedance / admittance control · 6-axis F/T sensing · VR experimental platforms (Unity + headset integration) · Motion capture (Vicon/OptiTrack) · Python · C++

## Related Work

- [Risk-Aware Voronoi Arms]({{ site.baseurl }}/projects/voronoi-arms/) (BYU, ME EN 537) and the DRIVE Lab follow-on [Risk-Aware Weighted Buffered Voronoi Cells]({{ site.baseurl }}/experience/wbvc/) (CMU) — multi-agent safe navigation through decentralized Voronoi tessellation

---

[← Back to Experience]({{ site.baseurl }}/research/)
