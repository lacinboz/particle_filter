# Particle Filter Project

This repository contains a particle filter implementations for tracking indistinguishable objects in noisy environments:

#  Multimodal Particle Filter for Tracking Multiple Balls

This project implements a **multimodal particle filter** that estimates the **positions and velocities of multiple indistinguishable balls** thrown simultaneously under noise and uncertainty.

##  Problem Setup

We simulate the parabolic motion of several balls (e.g., 3) thrown with random speed and angle. The challenge is:
- The balls are indistinguishable (no ID).
- The sensor data is **noisy** and contains **dropouts** (missing frames).
- There may be **multiple likely positions** for each ball at each time — hence, **multimodal estimation** is required.

##  Core Ideas

###  Particle Filter Basics

- Each particle represents a hypothesis about the full system state.
- Our particle state = concatenated `[x, y, vx, vy]` for all balls.
- We propagate particles using physics + process noise.
- When there’s a measurement:
  - We compute the **likelihood** of each particle, given the observation.
  - Likelihood is computed by comparing particle predictions to actual noisy measurements (using distance-based Gaussian likelihood).
- These likelihoods are used to **reweight** the particles. The resulting distribution is the **posterior**.
- We **resample** the particles based on these weights.

###  Multimodal Estimation

- When the sensor fails (dropout), we can’t use measurements to correct particle weights.
- Instead, we apply **KMeans clustering** to the particle cloud.
  - This lets us identify **multiple candidate modes** (clusters) for each ball’s possible position.
  - The most populated cluster is taken as the estimate (mode estimation).

## Terminology

| Term | Meaning in Code |
|------|------------------|
| **Posterior** | Weighted particle set after correction (in `weights` and `particles`) |
| **Likelihood** | Computed in `update()` as how likely the observed data matches each particle |
| **Weighted Particles** | The combination of `particles` and `weights` — represents the posterior belief |
| **Measurement (Z)** | Noisy observations of ball positions (`observed_positions`) |
| **Dropout** | Frames where all measurements are missing (`np.nan`) |

##  Structure

| File/Section | Purpose |
|--------------|---------|
| `initialise_ball_params()` | Creates true initial positions/velocities for each ball |
| `particles` | Stores all particle hypotheses |
| `weights` | Represents probability of each particle |
| `predict()` | Moves particles forward in time with noise |
| `update()` | Adjusts weights based on observations (likelihood computation) |
| `resample()` | Draws new particles proportional to weights |
| KMeans in loop | Handles missing data via clustering |
| `estimated_positions` | Final filtered (and clustered) output |

##  Output

- True trajectories (ground-truth)
- Noisy observations (with gaps)
- Estimated trajectories (from PF + KMeans)
- Final particle clouds

The system prints bias, standard deviation, and RMSE of position estimates for each ball.

##  Sample Visualization

- Dashed lines = true trajectories  
- Crosses = noisy observations  
- Dotted lines = estimated positions  
- Transparent clouds = particle distributions  

##  Notes

- Code handles up to 4 balls with exact permutations for measurement-particle associations.
- For more than 4 balls, the **Hungarian algorithm** (linear sum assignment) is used for approximate best matching.
- Particle count can be scaled (default = 100,000) for smoother posteriors.

## Author
Created by Laçin Boz and Bilal Bilal for the Portfolio Exam of "Reasoning and Decision Making under Uncertainty" at THWS.
