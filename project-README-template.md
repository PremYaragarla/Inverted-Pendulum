# Inverted Pendulum Control

> State-feedback controllers that balance a pendulum on a cart in an upright position, simulated in Python. 

![demo](cart_pole_control.gif)

---

## Overview

**The problem:** An inverted pendulum mounted on a motor-driven cart that slides along a horizontal rail. The upright positon is an open-loop unstable equilibrium, and the system is underactuated with the cart motor controling both cart position and the pole angle. The objective of this project is reaching stabilization with the pole upright, while keeping the cart close to its starting positon and recover from disturbances. 

**The approach:** The essence of the controllers is the LQR (Linear-quadratic regulator). I derived the equations of motion using lagrangian mechanics and sympy, linearized them about the upright position, and used the state-feedback law **u = -Kx** that minimizes a quadratic cost on state error and control effort. I chose LQR because the gain is optimized and outperforms hand-tuning. The Q and R weighting matrices give clear control over the characteristics of the controller, allowing me to set which variables are more important and affect settling times and overshoots.

**The outcome:** ________

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

- **Plant model:** [transfer function / state-space, and how you got it —
  first principles, or system ID from data]
- **Controller:** [PID / LQR / pole placement / MPC — with the design choices]
- **Estimation (if any):** [observer / Kalman filter, sensors used]
- **Assumptions & limits:** [linearization region, sample rate, saturation, etc.]

---

## Tech Stack

- **Modeling & design:** [MATLAB R2024a, Control System Toolbox / Python + control]
- **Implementation:** [C on STM32F4 / Python / Simulink Coder]
- **Hardware:** [board, sensors, actuators — or "simulation only"]

---

## Repository Structure

```
.
├── docs/            # plots, GIFs, design notes, report PDF
├── model/           # MATLAB/Simulink or Python model & controller design
├── firmware/        # embedded C for hardware deployment
├── sim/             # simulation scripts
├── data/            # logged runs (keep large files out — see .gitignore)
└── README.md
```

---

## How to Run

**Simulation**
```bash
[e.g. python sim/run.py   —or—   open model/design.slx and press Run]
```

**Hardware** (if applicable)
```
1. [Flash firmware/ to the board]
2. [Wiring / setup notes]
3. [How to start it and what you should see]
```

**Requirements:** [MATLAB + Control System Toolbox, or `pip install -r requirements.txt`, plus any hardware]

---

## What I'd Improve Next

- [e.g. add integral action to remove residual offset under load]
- [e.g. replace hand-tuned filter with a proper Kalman filter]

---

## License

Released under the [MIT](LICENSE) license.
