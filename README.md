# Adaptive Cruise Control (ACC) using MATLAB/Simulink

## Overview

This project presents the development of an **Adaptive Cruise Control (ACC)** system using MATLAB/Simulink. The system is designed to automatically maintain a driver-set speed while ensuring a safe distance from a lead vehicle.

The model combines **vehicle longitudinal dynamics** with **control algorithms (PID)** to simulate real-world driving scenarios such as cruising and vehicle following.

---

## Problem Statement

In real-world driving, maintaining both a desired speed and a safe distance from other vehicles is challenging for drivers.

This project aims to:

- Maintain a constant **cruise speed**
- Automatically adjust speed based on **lead vehicle distance**
- Prevent collisions by maintaining a **safe following distance**
- Switch intelligently between **acceleration and braking**

---

## System Architecture

The system consists of the following components:

### Vehicle Model
- 1DOF longitudinal vehicle dynamics
- Force-based motion (Newton’s law)

### Driver Interface
- Knob → Desired speed input
- Toggle switch → ACC ON/OFF

### Controllers
- Speed PID Controller
- Distance PID Controller

### Decision Logic
- Minimum selector block (`min()`)

### Distance Estimation
- Lead vehicle distance (integrated)
- Ego vehicle distance (integrated)

---

## Workflow

1. **Driver Input**
   - Driver sets desired speed using a knob
   - Toggle switch enables/disables ACC

2. **Set Speed Capture**
   - When ACC is ON, current ego speed is captured as `SetSpeed`

3. **Speed Control**
   - Error:
     ```
     Error = SetSpeed - EgoSpeed
     ```
   - PID controller generates `AccCmdSpeed`

4. **Distance Calculation**
   - Lead and ego distances are calculated using integrators
   - Relative distance:
     ```
     ActualDistance = LeadDistance - EgoDistance
     ```

5. **Safe Distance Calculation**
   - SafeDistance = MinimumDistance + TimeGap × EgoSpeed

6. **Distance Control**
- Error:
  ```
  DistanceError = ActualDistance - SafeDistance
  ```
- PID controller generates `AccCmdDistance`

7. **Decision Logic**
   - FinalAccCmd = min(AccCmdSpeed, AccCmdDistance)

8. **Force Conversion**
- If `FinalAccCmd > 0` → Acceleration → Front wheel force (FwF)
- If `FinalAccCmd < 0` → Braking → Rear wheel force (FwR)

9. **Vehicle Response**
- Force → Acceleration → Velocity → Distance

## Control Strategy

### Speed Controller
Maintains desired speed:
- e_v = V_set - V_ego

### Distance Controller
Maintains safe following distance:
- D_safe = D_min + T_gap × V_ego
- e_d = D_actual - D_safe

### Decision Logic (Key Concept)
- FinalAccCmd = min(AccCmdSpeed, AccCmdDistance)

 Ensures:
- Safe operation always has priority
- Braking overrides acceleration when needed

---

## Results

### Observed Outputs:
- Ego Speed (`xdot`)
- Distance Error
- Safe Distance
- Acceleration Commands
- Wheel Forces (FwF, FwR)

### Key Behaviors:
- Smooth speed tracking
- Stable response in cruise mode
- Distance controller activates when unsafe
- Automatic switching between throttle and brake

---

## Observations

- Speed controller dominates when:
  ActualDistance > SafeDistance
- Distance controller activates when:
  ActualDistance < SafeDistance

- Integrator-based distance grows continuously over time
- Braking is only triggered in unsafe conditions

---


## Tools Used

- MATLAB
- Simulink
- PID Controller Toolbox

---

## Conclusion

This project demonstrates a working **Adaptive Cruise Control system** using model-based design. It highlights how:

- Multiple controllers can work together
- Safety constraints override performance goals
- Vehicle dynamics interact with control systems

The model provides a strong foundation for building more advanced ADAS features such as **AEB, LKA, and full autonomous driving systems**.

---

## Author

Ramasubramanian

---
