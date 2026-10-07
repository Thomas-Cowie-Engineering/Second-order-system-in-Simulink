# Mass-Spring-Damper Simulation (Simulink)

A Simulink model of a second order mass spring-damper system. 

<img width="230" height="230" alt="image" src="https://github.com/user-attachments/assets/4c95596a-7d8e-4144-a219-ce9d52bb8f4d" />



<!-- Add a screenshot of your model here, e.g. ![Model diagram](images/model.png) -->

## What it models

A mass `m` attached to a spring (stiffness `k`) and a damper (damping coefficient `c`). The equation of motion is:

```
m·ẍ + c·ẋ + k·x = 0
```

Rearranged for acceleration, which is how the model is built:

```
ẍ = -(c/m)·ẋ - (k/m)·x
```

where `x` is position (positive upward), `ẋ` is velocity and `ẍ` is acceleration.

## How the model works

The model is a chain of two integrators with two feedback loops:

```
Acceleration → [1/s] → Velocity → [1/s] → Position
```

| Block | Purpose |
|---|---|
| Integrator | Integrates acceleration to give velocity. Its initial condition is the **initial velocity**. |
| Integrator1 | Integrates velocity to give position. Its initial condition is the **initial position**. |
| Gain (C/M Damper) | Feeds back velocity scaled by `c/m` (damping term). |
| Gain (K/M Spring) | Feeds back position scaled by `k/m` (spring term). |
| Sum block | Subtracts both feedback terms to produce acceleration. |
| Scopes | Display acceleration, velocity and position. |

With no force acting on the mass, the system only moves if you give it a non-zero initial position or velocity.

## Parameters

| Parameter | Where to set it | Notes |
|---|---|---|
| `c/m` | C/M Damper gain | Damping term |
| `k/m` | K/M Spring gain | Spring term |
| Initial position | Integrator1 initial condition | Metres, positive is upward |
| Initial velocity | Integrator initial condition | m/s, negative is downward |

Gain blocks accept expressions, so you can type `2*sqrt(0.4)` directly instead of a decimal.

## Running the simulation

1. Open the `.slx` model file in MATLAB/Simulink.
2. Set the gain values and initial conditions (see above).
3. Go to **Modeling → Model Settings** and set:
   - Solver type: **Fixed-step**
   - Solver: **auto**
   - Fixed-step size: your choice (e.g. `0.001` for a smooth result)
4. Set the stop time (e.g. 50 s) and press **Run**.
5. Open the scopes to view the response. In the scope's **Measurements** tab, **Signal Statistics** gives the min/max of a signal and **Peak Finder** labels its peaks.

Note that the step size affects numerical accuracy, so results can differ slightly between step sizes.

## Damping behaviour

The damping regime depends on `c` relative to the critical value:

| Regime | Condition | Behaviour |
|---|---|---|
| Underdamped | `c < 2√(mk)` | Oscillates, with amplitude decaying over time |
| Critically damped | `c = 2√(mk)` | Returns to rest as fast as possible without oscillating |
| Overdamped | `c > 2√(mk)` | No oscillation, but returns to rest more
