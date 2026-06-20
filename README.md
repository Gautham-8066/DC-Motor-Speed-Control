# DC Motor Speed Control using a PI Controller in Simulink

A MATLAB/Simulink implementation of a closed-loop velocity control system using a Proportional-Integral (PI) controller to regulate the speed of a first-order dynamic plant (representing a simplified DC motor model).
<img width="1807" height="778" alt="image" src="https://github.com/user-attachments/assets/8e51989b-25ff-42d0-a9a0-530d56a5953c" />

## System Architecture

The simulation models a standard negative feedback control loop configured as follows:

1. **Input Signal:** A step input acting as the reference target speed command (setpoint step to 100 at $t = 1\text{ s}$).
2. **Error Detection:** A summing junction calculating the error signal: $e(t) = \text{Setpoint} - \text{Actual Speed}$.
3. **PI Controller:** Processes the error signal through parallel controller paths:
   * **Proportional Path:** Multiplies the error signal by a static proportional gain ($K_p = 10$) to provide immediate corrective action.
   * **Integral Path:** Integrates the error ($\frac{1}{s}$) to eliminate steady-state tracking error under constant load conditions, scaled by an integral gain ($K_i = 15$).
   * **Derivative Path:** The derivative gain is set to $K_d = 0$, reducing the controller architecture to a robust **PI configuration** to prevent high-frequency noise amplification.
4. **Plant Dynamics:** A first-order system representing the mechanical/electrical time constants of a motor, defined by the transfer function:
   $$G(s) = \frac{100}{0.5s + 1}$$
5. **Visualization:** A multi-port Scope capturing both the reference speed command (blue) and the actual motor speed output (yellow) for transient response analysis.

---

## Technical Specifications

| Parameter / Block | Value / Configuration |
| :--- | :--- |
| **Plant Transfer Function** | $\frac{100}{0.5s + 1}$ |
| **Proportional Gain ($K_p$)** | 10 |
| **Integral Gain ($K_i$)** | 15 |
| **Derivative Gain ($K_d$)** | 0 (Disabled) |
| **Feedback Type** | Negative Feedback |

---

## Performance & Results

The plot below shows the closed-loop step response captured by the Scope block. 

<img width="1908" height="920" alt="image" src="https://github.com/user-attachments/assets/62aeacd2-dcf6-410b-a795-1d605bfae195" />


### Transient Response Analysis

* **Tracking & Steady-State Error:** The system tracks the reference command with **zero steady-state error** ($\rho_{ss} = 0$). The actual speed settles exactly at the target value of 100, showing that the integral path successfully eliminated systemic offset.
* **Overdamped Response:** Due to the tuning parameters and the inherent first-order plant dynamics, the system achieves a smooth, **overdamped response with 0% overshoot**. This is highly desirable in motor applications where speed tracking must avoid mechanical strain or jerking.
* **Settling Time ($T_s$):** Following the step input command change at $t = 1.0\text{ s}$, the motor speed transitions smoothly and completely settles into the target value within approximately $2.5\text{ seconds}$ ($t \approx 3.5\text{ s}$).

---
