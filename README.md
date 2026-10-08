# Second order Simulation in Simulink

A Simulink model of a second order mass spring-damper system. 

The model:

<img width="230" height="230" alt="image" src="https://github.com/user-attachments/assets/4c95596a-7d8e-4144-a219-ce9d52bb8f4d" />


The Simulink model that represents it

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/a2b7f843-10e9-40b3-9e68-5ad98f9a110a" />

## How the model works

| Block | Purpose |
|---|---|
| Integrator | Integrates acceleration to give velocity. Its initial condition is the **initial velocity**. |
| Integrator1 | Integrates velocity to give position. Its initial condition is the **initial position**. |
| Gain (C/M Damper) | Feeds back velocity scaled by `c/m` (damping term). |
| Gain (K/M Spring) | Feeds back position scaled by `k/m` (spring term). |
| Sum block | Subtracts both feedback terms to produce acceleration. |
| Scopes | Display acceleration, velocity and position. |

With no force acting on the mass, the system only moves if you give it a non-zero initial position or velocity.


## Key things to know

- when C = 1.3, the system becomes critically damped
