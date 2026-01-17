# Autonomous Vehicle Control & Simulation Using ROS

**Authors:** Inon Reany & Adi Cheifetz

---

## 1. Overview
This project bridges the gap between high-level autonomous algorithms and physical vehicle actuation. By utilizing **ROS (Robot Operating System)** as middleware, the system integrates sensor perception, path planning, and low-level control protocols to navigate a simulated environment.

---

## 2. System Architecture
The architecture is divided into three primary functional layers:

| Layer | Component | Function |
| :--- | :--- | :--- |
| **Simulation** | Gazebo / RViz | Physics engine (gravity/friction) and data visualization. |
| **Perception** | LiDAR / Camera | 3D point clouds and visual recognition of lanes and signs. |
| **Localization** | GPS / IMU | Global positioning, orientation, and acceleration data. |
| **Execution** | Drive-by-Wire | Translation of software commands into mechanical actions. |

---

## 3. Core Theoretical Foundations

### 3.1 Ackermann Steering Geometry
Unlike differential drive robots, this system follows Ackermann geometry to prevent tire slippage during turns.
* **The Principle:** During a turn, the front wheels are angled differently because the inner wheel follows a tighter curve than the outer wheel.
* **The Calculation:** All wheels must rotate around a common **Instantaneous Center of Rotation (ICR)**.
* **Project Integration:** ROS nodes calculate specific left/right wheel angles based on the wheelbase and track width.

### 3.2 CAN Bus Protocol
Data communication within the vehicle is based on the **Controller Area Network (CAN)** protocol, the automotive industry standard.
* **Frame Structure:** Consists of a unique Identifier and an 8-byte data field.
* **Robustness:** Highly resistant to electrical noise and allows efficient communication between multiple Electronic Control Units (ECUs).
* **Implementation:** The project simulates CAN messages for commands like steering and braking, while reading messages for wheel speeds.

---

## 4. The ROS Control Loop
ROS acts as the central nervous system, managing a continuous feedback loop:

1. **Data Acquisition (Perception):** Nodes subscribe to raw sensor data on dedicated topics such as `/scan` (LiDAR) and `/camera/image_raw`.
2. **Path Planning & Navigation:** The algorithm processes waypoints and calculates optimal paths while avoiding obstacles.
3. **Actuation (Control):** Velocity commands are translated into physical control values:
   * **Throttle:** Percentage (0.0 - 1.0).
   * **Brake:** Torque or percentage.
   * **Steering:** Angle in radians.

---

## 5. Navigation Logic & Safety
* **Navigation Process:** The system reads GPS coordinates, computes errors in distance and angle to the target waypoint, and sends corrective commands.
* **Obstacle Avoidance:** Continuous monitoring of LiDAR data ensures that if an object is detected within three meters, the vehicle stops automatically.

---

## 6. Critical Analysis: Simulation vs. Reality
While simulation provides a powerful environment, the system has inherent limitations:
* **The Reality Gap:** Gazebo physics cannot perfectly mimic real-world tire friction, weather conditions, or sensor noise.
* **Real-Time Performance:** ROS 1 is not a hard real-time system; CPU load can cause latency in message delivery, which is critical at high speeds.

---

## 7. Future Directions
* **ROS 2 Migration:** Transitioning to ROS 2 (DDS-based) for better real-time support and security.
* **End-to-End Deep Learning:** Implementing Behavioral Cloning to allow neural networks to drive directly from camera images.
* **Predictive Control:** Moving to **Model Predictive Control (MPC)** to predict vehicle behavior several steps ahead.
