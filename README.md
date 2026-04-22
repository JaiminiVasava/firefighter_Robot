# DOF2 Arm – Robotic Arm Simulation Project

A 2-Degree-of-Freedom (2-DOF) planar robotic arm modelled and simulated in **MATLAB/Simulink R2025b** using **Simscape Multibody** (SimMechanics) and **Simscape Thermal** libraries.

---

## Project Overview

This project implements a rigid-body, physics-based simulation of a 2-DOF revolute-joint robotic arm. The model captures the full kinematic chain from a fixed world frame through two rotational joints and two rigid links to an end-effector, and includes an additional thermal sensing subsystem attached to the arm structure.

---

## Repository Contents

| File | Description |
|---|---|
| `DOF2_Arm.slx` | Main Simulink model (latest version, R2025b) |
| `DOF2_Arm.slx.original` | Original / backup version of the Simulink model |
| `untitled.slx` | Auxiliary / working Simulink model |
| `untitled.slxc` | Simulink model cache file (auto-generated) |
| `DOF2_Arm_Load.m` | MATLAB script to load the robot model into the workspace |

---

## Requirements

- **MATLAB** R2025b (or compatible)
- **Simulink**
- **Simscape** (base package)
- **Simscape Multibody** (formerly SimMechanics) — for rigid body dynamics
- **Simscape** Thermal library (`fl_lib`) — for the thermal sensor subsystem

---

## Getting Started

1. Open MATLAB and set the project folder as your working directory.
2. Run the loader script to import the robot model into the workspace:
   ```matlab
   run('DOF2_Arm_Load.m')
   ```
   This sets the sample time `Ts = 0.001` s and imports the robot object using `importrobot('DOF2_Arm')`.
3. Open the main Simulink model:
   ```matlab
   open('DOF2_Arm.slx')
   ```
4. Click **Run** to simulate. The default stop time is **10 seconds**.

---

## Model Architecture

The top-level Simulink model is structured as a serial kinematic chain:

```
World Frame → [Weld Joint] → Base → Joint1 (Revolute) → Link1 → Joint2 (Revolute) → Link2 → End-Effector
                                                                                            ↓
                                                                                    Thermal Sensor
```

### Subsystems

| Subsystem | Type | Description |
|---|---|---|
| **Base** | Cylindrical Solid | Fixed base of the arm; connected to the World Frame via a Weld (fixed) joint |
| **Joint1** | Revolute Joint | First rotational joint (base ↔ Link1); motion actuation mode: Computed Motion |
| **Link1** | Brick Solid | First arm link — dimensions: 0.25 m × 0.03 m × 0.03 m; density: 1000 kg/m³ |
| **Joint2** | Revolute Joint | Second rotational joint (Link1 ↔ Link2); motion actuation mode: Computed Motion |
| **Link2** | Brick Solid | Second arm link — dimensions: 0.20 m × 0.02 m × 0.02 m; density: 1000 kg/m³ |
| **End-Effector** | Brick Solid | Tool/end point of the arm, attached via a Rigid Transform |
| **Thermal Sensor** | Simscape Thermal | Subsystem simulating temperature measurement on the arm structure |
| **Mechanism Configuration** | Utility | Defines uniform gravity: [0, 0, −9.80665] m/s² (Z-down) |
| **Solver Configuration** | Utility | Network engine solver for Simscape physical network |
| **Scope** | Output | Displays simulation output signals |

### Joint Configuration (Joint1 & Joint2)

Both joints are **Revolute Joints** with the following properties:
- Actuation mode: **Computed Motion** (kinematically driven)
- Torque actuation: None
- Spring stiffness: 0 N·m/deg
- Damping coefficient: 0 N·m/(deg/s)
- Joint limits: ±90° (soft limits with stiffness 10,000 N·m/deg and damping 10 N·m/(deg/s))

### Thermal Sensor Subsystem

The `Thermal Sensor` subsystem uses Simscape Thermal components including:
- **Controlled Temperature Source** — drives a thermal input
- **Temperature Sensor** — measures temperature at a point on the arm
- **Thermal Reference** — ground reference for the thermal network
- PS-Simulink / Simulink-PS Converters — bridge physical signals to Simulink

---

## Simulation Settings

| Parameter | Value |
|---|---|
| Simulation stop time | 10.0 s |
| Solver type | Variable-step (VariableStepAuto) |
| Max step size | Auto |
| Fixed step size | Auto |
| Simscape solver relative tolerance | 0.001 |
| Simscape solver absolute tolerance | 0.001 |
| Gravity vector | [0, 0, −9.80665] m/s² |
| Sample time (`Ts`) | 0.001 s |

---

## Physical Parameters Summary

| Component | Parameter | Value |
|---|---|---|
| Link1 | Dimensions (L × W × H) | 0.25 m × 0.03 m × 0.03 m |
| Link1 | Density | 1000 kg/m³ |
| Link2 | Dimensions (L × W × H) | 0.20 m × 0.02 m × 0.02 m |
| Link2 | Density | 1000 kg/m³ |
| Both links | Inertia calculation | Computed from geometry |

---

## Notes

- The `.slx.original` file preserves the initial state of the model before modifications; refer to it if you need to compare changes or revert.
- The `untitled.slx` file appears to be a working/scratch model; check its contents if exploring alternative configurations.
- The model was last saved on **MATLAB R2025b (Update 5, Feb 24 2026)** on a Windows 64-bit machine. Opening on older MATLAB releases may produce warnings.
- Inertia for both links is calculated automatically from geometry; manual inertia values in the model are placeholder defaults.

---

## License

This project is intended for academic/educational purposes. Please add your own license information as appropriate.
