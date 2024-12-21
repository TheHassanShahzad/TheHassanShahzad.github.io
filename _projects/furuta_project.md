---
layout: page
title: Furuta Pendulum
description: LQR control of a Furuta Pendulum in Gazebo
img: assets/img/furuta/best.jpg
importance: 3
category: work
---

# Furuta Pendulum Project

## Mastering the Dynamics of Underactuated Systems: The Furuta Pendulum Project

### Overview
This project explores the control and dynamics of a Furuta pendulum, a classical problem in control theory and robotics. Simulated in **Gazebo** and controlled via **ROS2_control**, this project demonstrates my ability to design, implement, and fine-tune control systems for underactuated mechanisms. Key objectives included stabilizing the pendulum in the upright position and managing transitions using advanced feedback control techniques.

> **Insert Image**: Screenshot of the Furuta pendulum in Gazebo simulation showing the pendulum and rotary arm.

---

### Motivation
Underactuated systems like the Furuta pendulum are widely used in robotics to study control strategies for dynamic systems with fewer actuators than degrees of freedom. This project:
- Provided hands-on experience with modeling, simulation, and control.
- Bridged theoretical knowledge with practical implementation.
- Strengthened my understanding of ROS2 and its application in robotics control.

---

### System Design

#### Simulation Environment
- **Gazebo** was used for realistic simulation of the pendulum’s dynamics, enabling precise modeling of:
  - Rotary arm and pendulum properties (mass, length, inertia).
  - Friction and damping.
- Integrated with **ROS2_control** for seamless actuator and controller management.

> **Insert Image**: Diagram of the system architecture showing the interaction between Gazebo, ROS2 nodes, and the control framework.

#### Control Objectives
1. **Swing-Up Control**:
   - Transition the pendulum from rest to the upright position using energy-based control.
2. **Stabilization**:
   - Maintain the pendulum at its unstable equilibrium (π) using:
     - **Linear Quadratic Regulator (LQR)** for optimal control.
     - Alternative controllers (e.g., PID, MPC) for comparison.

#### Angle Normalization
- Adjusted angle references to simplify control logic:
  - **0** and **2π** represent the lowest position.
  - **π** denotes the upright position.
- Ensured seamless transition between states without singularity or wrap-around errors.

---

### Implementation

#### Key Steps
1. **Gazebo Model Development**:
   - Designed a custom URDF model for the pendulum and arm.
   - Tuned parameters for realistic physical behavior.

> **Insert Image**: Code snippet of the URDF model or a rendered 3D model of the pendulum.

2. **ROS2 Integration**:
   - Developed ROS2 nodes for control and monitoring.
   - Used ROS2 topics for real-time data flow between controllers and the simulation.

3. **Control Design**:
   - Linearized the system around its unstable equilibrium for feedback control.
   - Implemented swing-up strategies based on energy shaping.
   - Evaluated performance metrics like overshoot, settling time, and stability.

> **Insert Image**: Plot of pendulum angle over time showing swing-up and stabilization phases.

---

### Challenges
1. **Angle Reference Adjustments**:
   - Initial configuration placed the lowest point at π, which conflicted with control logic.
   - Reconfigured the system to ensure consistency between physical intuition and implementation.

2. **Controller Tuning**:
   - Balancing sensitivity and robustness to handle simulated noise.
   - Avoiding instability due to actuator saturation.

3. **Simulation Fidelity**:
   - Achieved a fine balance between computational efficiency and accurate dynamics.

> **Insert Image**: Comparison plot showing the effect of different controller gains on system stability.

---

### Results
- **Swing-Up Control**:
  - Achieved smooth transitions from rest to the upright position.
- **Stabilization**:
  - Demonstrated precise balancing at π, even under simulated disturbances.
- **Metrics**:
  - Response times and stability margins met predefined benchmarks.

> **Insert Image**: Additional plot showing controller stability margins or actuator effort over time.

---

### Takeaways
1. **Technical Expertise**:
   - Gained proficiency in ROS2_control and Gazebo simulation.
   - Strengthened skills in feedback control design and analysis.

2. **Problem-Solving**:
   - Overcame real-world-inspired challenges like noise, delays, and reference inconsistencies.

3. **Future Applications**:
   - Insights from this project can extend to real-world robotic systems and advanced underactuated mechanisms.

---

### Extensions
- **Hardware Implementation**: Build a physical Furuta pendulum and test control strategies on real hardware.
- **Comparison of Control Strategies**: Extend to nonlinear controllers or adaptive control for better disturbance rejection.
- **Multi-Agent Systems**: Integrate this project into a collaborative robotics setup for swarm control studies.

> **Insert Image**: Concept diagram or photo of a potential hardware implementation.

---

### Acknowledgments
Special thanks to open-source communities and research contributions in control theory that provided invaluable guidance during the development of this project.

---

### Media Gallery
- **Controller Performance**: Graphs comparing LQR and PID/MPC performance.
- **System Diagram**: Visualizing the interaction between Gazebo, ROS2 nodes, and control frameworks.
