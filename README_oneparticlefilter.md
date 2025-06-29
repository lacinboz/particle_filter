# One-Particle-Per-Ball Particle Filter

This project implements a particle filter system where each ball has its own dedicated filter. It simulates the motion of `n ≥ 1` balls thrown simultaneously, and each filter estimates the position and velocity of a single ball using noisy observations.

## Key Features

- Separate particle filter for each ball.
- Handles noisy observations and random dropout intervals.
- Realistic parabolic motion simulation.
- Dropout periods are visualized with red crosses.
- Visualization highlights ground-truth vs. estimation per ball.

##  Assumptions

- **Ball-Observation Matching is Known**: Each particle filter knows which observation belongs to which ball.
- This simplifies the estimation but is not realistic in real-world sensor fusion with indistinguishable objects.

##  File Structure

- `ParticleFilter` class: Individual PF logic (state = [x, y, vx, vy]).
- `simulate_ball_throwing()`: Simulates parabolic motion.
- `simulate_noisy_observations()`: Adds noise and dropout to positions.
- `visualize_with_dropout_highlight()`: Plots true, observed, and estimated trajectories.

##  How to Run

```bash
Open `oneparticlefilter.ipynb` in Jupyter Notebook and run all cells.
```

## Parameters

- `N_BALLS = 3`: Number of balls
- `num_particles = 20000`: Per-ball particles
- `dropout_prob = 0.15`: Probability of observation loss
- `obs_noise_std = 0.9`: Sensor noise
- Particle initial state ranges can be adjusted in `ParticleFilter` init.

##  Results

- Estimated paths closely follow true trajectories.(accuracy is good)
- Dropout periods are smoothly handled.
- Suitable for benchmarking and understanding filter dynamics in ideal settings.

##  Limitations

- Unrealistic assumption that ball IDs are known in observations.
- Doesn't scale well to indistinguishable/multimodal scenarios.
