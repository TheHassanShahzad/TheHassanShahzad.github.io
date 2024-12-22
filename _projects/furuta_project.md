---
layout: page
title: Furuta Pendulum
description: LQR control of a Furuta Pendulum in Gazebo
img: assets/img/furuta/upright.png
importance: 3
category: work
---

# Furuta Pendulum Project

## Mastering the Dynamics of Underactuated Systems: The Furuta Pendulum Project

### Overview
This project focuses on the mathematical modeling, system integration, and control of a Furuta Pendulum in a **ROS2 + Gazebo** environment. By carefully designing the physical and computational setup—ranging from deriving the system’s equations of motion to implementing an **LQR** controller—this project demonstrates how to stabilize an inherently unstable underactuated system. The main emphasis is on the nuances of the setup, control design, and the underlying theory rather than on the visual data plots alone.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/furuta/upright.png" title="furuta upright" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

---

## Motivation
The Furuta Pendulum (sometimes referred to as the Rotary Inverted Pendulum) is a canonical example of an underactuated robotic system. It provides deep insight into:
- **Lagrangian Mechanics** for describing the nonlinear dynamics.  
- **Feedback Control** techniques such as LQR for balancing an unstable equilibrium.  
- **Practical Implementation** challenges in simulating and tuning underactuated systems.

Beyond being an academic benchmark, the principles learned here scale to more complex systems (e.g., robot arms, humanoids) that exhibit similar underactuated characteristics.

---

## System Design

### Mathematical Foundations

#### Equations of Motion
A typical Furuta Pendulum has two degrees of freedom:
1. The **rotary arm** (actuated, angle \(\theta\)).
2. The **pendulum rod** (unactuated, angle \(\alpha\)).

Using a Lagrangian approach, the total energy \( \mathcal{L} = T - V \) (kinetic minus potential) is computed. For instance:

- Let \( m \) be the pendulum bob mass, \( l \) be the distance from pivot to pendulum bob center of mass, and \( I \) be the rotary arm’s moment of inertia (including motor rotor inertia).
- The angles \(\theta\) (arm) and \(\alpha\) (pendulum) define the configuration.

A simplified (yet representative) set of nonlinear equations can be expressed as:

\[
\begin{aligned}
I_\theta \ddot{\theta} + m l^2 \sin^2(\alpha) \, \ddot{\theta} + \dots = \tau,\\
I_\alpha \ddot{\alpha} + \dots = 0,
\end{aligned}
\]

where \(I_\theta\) and \(I_\alpha\) represent combined inertias about each axis, and \(\tau\) is the control torque applied to the motor. Detailed derivation involves computing partial derivatives of the Lagrangian with respect to \(\theta, \alpha\) and their time derivatives. Friction terms can be added as needed, often requiring empirical tuning.

#### Linearization for LQR
To design an **LQR** controller, the system is linearized about the **upright equilibrium** \((\theta, \alpha) = (0, \pi)\) (or your chosen reference angles). The state vector is typically:

\[
\mathbf{x} = \begin{bmatrix} \theta \\ \alpha \\ \dot{\theta} \\ \dot{\alpha} \end{bmatrix}, 
\quad \mathbf{u} = \tau.
\]

After computing the Jacobians of the nonlinear dynamics at the equilibrium, we obtain:

\[
\dot{\mathbf{x}} = A \mathbf{x} + B \mathbf{u}.
\]

**\(Q\)** and **\(R\)** matrices in LQR are chosen to balance state error (e.g., penalizing large deviations of \(\alpha\) from \(\pi\)) and control effort. The resulting LQR gain matrix \(\mathbf{K}\) yields \(\mathbf{u} = -\mathbf{Kx}\).

---

### Simulation Environment

#### Gazebo + ROS2 Integration
- **Gazebo**: Provides a physics engine that simulates the rotary arm and pendulum rod using specified moments of inertia, mass, length, and friction.  
- **ROS2_control**: Handles the hardware abstraction layer, seamlessly passing torque commands from the LQR node (or other controllers) to the Gazebo joint.

**Key Steps**:
1. **URDF Model**:  
   - Defined links for the rotary arm and pendulum with accurate inertia tensors (mass, inertia about the respective axes).  
   - Specified joints, including the range of motion and friction characteristics.  

2. **Controllers**:  
   - **Joint State Broadcaster**: Publishes the real-time angles \(\theta\) and \(\alpha\).  
   - **Effort Joint Controller**: Takes in torque commands (\(\tau\)) for the rotary joint.  
   - Additional plugins or custom nodes can handle advanced logging, sensor noise injection, or specialized debugging tasks.

3. **Launch Files**:  
   - Combine Gazebo environment, robot description (URDF), and controllers into a single launch script.  
   - Manage simulation parameters (e.g., real-time factor, friction coefficients, damping factors).

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/furuta/down_colour_visual.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/furuta/clear.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/furuta/down_colour_collision.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

---

### Control Objectives

1. **Swing-Up Control**  
   - Increase the pendulum’s total mechanical energy to reach the upright position.  
   - An energy-based method (e.g., implementing \(\dot{E}\)-shaping) was coded as a separate node that modulates torque to drive the pendulum near \(\alpha \approx \pi\).

2. **Stabilization**  
   - Once near upright, switch to a **Linear Quadratic Regulator (LQR)** designed around \(\alpha = \pi\).  
   - Minimizes a cost function \(J = \int_0^{\infty} (\mathbf{x}^T Q \mathbf{x} + u^T R u) \, dt\).  
   - Achieves small angle deviation and robust performance.

#### Angle Normalization
For computational convenience, angles are mapped as follows:
- **Pendulum “down”** \(\alpha = 0 \equiv 2\pi\).  
- **Pendulum “up”** \(\alpha = \pi\).  

In code, modular arithmetic ensures states remain continuous and do not introduce abrupt jumps (e.g., going from \(2\pi\) back to 0). This is vital for consistent state feedback and smooth handoff between swing-up and balance controllers.

---

### Implementation Highlights

1. **Custom URDF Modeling**  
   - Accurately defined inertial frames to reflect the real mass distribution of the pendulum and arm.  
   - Verified center of mass placement through both manual calculation and Gazebo’s visuals.

2. **ROS2 Node Architecture**  
   - **`furuta_controller_node`** subscribes to `/joint_states` for \(\theta\) and \(\alpha\).  
   - Implements a real-time loop (e.g., 1 kHz) to compute the required torque using either energy-based or LQR logic, then publishes to `/arm_torque_controller/commands`.

3. **Code Organization**  
   - **`furuta_description`**: URDF, meshes, and plugin configs.  
   - **`furuta_bringup`**: Launch files to start Gazebo, controllers, and any auxiliary nodes.  
   - **`furuta_control`**: Implementation of LQR, energy-based control, and angle normalization utilities.

4. **Tuning Strategy**  
   - **LQR Weights**: Experimented with different diagonal entries in \(Q\) to penalize large \(\alpha\) errors and high angular velocities while keeping \(R\) moderate to prevent actuator saturation.  
   - **Energy Shaping Gains**: Balanced the energy injection rate so as not to exceed torque limits or induce large overshoot.

5. **Simulation Fidelity**  
   - Used smaller real-time factor for debugging to “slow down” the simulation and observe transient behaviors.  
   - Introduced low-level friction and damping parameters in URDF to mimic real hardware.  
   - Verified correct gravity compensation so the pendulum naturally hangs down when no torque is applied.

---

### Challenges and Insights

1. **Angle Offsets & Controller Handoff**  
   - Ensuring the angles used in the LQR matched the physical reference frames.  
   - Smoothly blending swing-up commands with LQR commands near the upright position without torque discontinuities.

2. **Dynamic Tuning Loop**  
   - Because the Furuta Pendulum is highly sensitive, small changes in inertia or friction significantly affected stability margins.  
   - Iteratively tuned both the URDF parameters and the LQR gains together to obtain robust performance.

3. **Runtime Logging and Debugging**  
   - Real-time data logging in ROS2 to track \(\theta\), \(\alpha\), torque, and velocities.  
   - Detailed debugging messages indicated when the pendulum was approaching upright, triggering the LQR.

---

### Results

- **Successful Swing-Up**: Repeatedly achieved smooth transitions from the lowest position to near-upright angles.  
- **Stabilization at \(\alpha = \pi\)**: The LQR controller kept the pendulum balanced with minimal oscillation, showcasing rapid settling times under varied initial disturbances.  
- **Robustness**: Small model parameter errors (like friction changes) did not break stability, thanks to conservative choice of \(Q\) and \(R\).

Though secondary, a selection of plots (angle vs. time, control input vs. time, energy breakdown) validated the final performance quantitatively.  

---

### Lessons and Extensions

1. **Hardware Realization**  
   - Current simulation groundwork is well-suited for porting to an actual hardware Furuta Pendulum. ROS2 and hardware drivers can mirror the same control pipeline, ensuring minimal code changes.

2. **Advanced Control Algorithms**  
   - Potential to explore **Model Predictive Control (MPC)** or **Adaptive Control** to handle parameter uncertainty more systematically.  
   - Could incorporate state observers (e.g., Kalman filters) for noisy sensor data.

3. **Multi-DoF Underactuated Robotics**  
   - Techniques here generalize to multi-link underactuated arms, allowing deeper exploration of energy-based swing-up methods and complex LQR scheduling.

---

### Acknowledgments
A heartfelt thanks to the open-source ROS2, Gazebo, and control-theory communities for providing powerful tools, detailed documentation, and conceptual inspiration.

---

## Media & Supplementary Notes

Below are some additional references and images illustrating the system design and partially the results. They serve as a visual complement rather than the central focus:

- **Wireframe CAD**: Showcasing URDF-defined shapes and collision geometry.  
- **Energy & Angle Plots**: Demonstrating how the mechanical energy is shaped during swing-up and how angles converge at \(\alpha = \pi\).  
- **Controller Debug Output**: Terminal snapshots indicating state transitions between swing-up and LQR balancing.  

