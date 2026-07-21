# Inverted Pendulum Control

> State-feedback controllers that balance a pendulum on a cart in an upright position, simulated in Python. 

![demo](cart_pole_control.gif)

---

## Overview

**The problem:** An inverted pendulum mounted on a motor-driven cart that slides along a horizontal rail. The upright positon is an open-loop unstable equilibrium, and the system is underactuated with the cart motor controling both cart position and the pole angle. The objective of this project is bring the pole to the upright position and hold it there, while keeping the cart close to its starting position and recover from disturbances. 

**The approach:** The essence of the controllers is the LQR (Linear-quadratic regulator). I derived the equations of motion using lagrangian mechanics and sympy, linearized them about the upright position, and used the state-feedback law **u = -Kx** that minimizes a quadratic cost on state error and control effort. I chose LQR because the gain is optimized and outperforms hand-tuning. The Q and R weighting matrices give clear control over the characteristics of the controller, allowing me to set which variables are more important and affect settling times and overshoots. A "swing-up" mode is used to ensure that that pole can reach the region where the LQR can catch and stabilize it. 

**The outcome:** The LQR controller is capable of swinging the pole up from rest and balancing it upright within ~8 seconds while recentering the cart over ~20 seconds. The PID baseline keeps the pole up, but revealed that full state feedback was necessary. 

---

## Results

*This is the section reviewers actually read. Lead with evidence.*

| Metric | Target | Achieved |
|---|---|---|
| Settling time (2%) | < 1.0 s | 0.82 s |
| Overshoot | < 5% | 2.1% |
| Steady-state error | 0 | ~0 |
| Phase margin | > 45° | 58° |

![Step response](docs/step_response.png)
![Tracking / disturbance rejection](docs/tracking.png)

[A sentence interpreting the plots — what to notice and why it's good.]

---

## System & Method

- **Single Pendulum:** State is [x, ẋ, θ, θ̇], with θ = π defined as upright. The nonlinear cart-pole equations are integrated with scipy.integrate.solve_ivp. For the LQR, the dynamics are linearized analytically about the upright equilibrium to get A and B, and the gain K is found by solving the continuous-time algebraic Riccati equation (scipy.linalg.solve_continuous_are), giving u = −Kx. Weightings Q = diag(0.1, 1, 50, 400) and R = 1/F_max² prioritize pole angle and rate while penalizing actuator effort. Force is saturated at F_max (20–50 N depending on the notebook).
- **Controller:** When the pole is far from upright (|angle error| > 45°) and moving slowly, the controller applies a force aligned with the pole's energy gradient to pump it up, reduced when the cart is already displaced so it doesn't run out of rail. Once inside the catch window, control switches to LQR.
- **Double Pendulum:** Same state as single pendulum, but θ = 0 is defined as upright. The full nonlinear equations of motion are derived symbolically with SymPy's LagrangesMethod from the system's kinetic and potential energy, then linearzied about the target via the symbolic Jacobian, with the LQR gain computed through python-control (control.lqr). 

---

## Tech Stack

- **Language:** Python (Jupyter Notebooks)
- **Modeling:** SymPy (Lagrangian mechanics, symbolic linearization
- **Control and Numerics:** NumPy, SciPy, python-control
- **Visualization:** Matplotlib (Plots and animations; FFmpeg for GIF/MP4 export)

---

## Repository Structure

```
.
├── notebooks/            # plots, GIFs, design notes, report PDF
├── figures/           # MATLAB/Simulink or Python model & controller design
├── requirements.txt
└── README.md
```

---

## How to Run

**Simulation**
```bash
[e.g. python sim/run.py   —or—   open model/design.slx and press Run]
```



**Requirements:** [MATLAB + Control System Toolbox, or `pip install -r requirements.txt`, plus any hardware]

---

## What I'd Improve Next

- [e.g. add integral action to remove residual offset under load]
- [e.g. replace hand-tuned filter with a proper Kalman filter]


