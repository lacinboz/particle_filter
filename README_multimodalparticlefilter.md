# Multimodal Unified Particle Filter

This project extends traditional particle filtering to handle **multiple indistinguishable objects** (balls) using a **single unified particle filter** and **clustering-based estimation**. It simulates multiple balls, each generating noisy observations with dropout, and estimates positions via multimodal density approximation.

## Key Features

- Unified particle filter for all balls.
- Observations are unlabeled (indistinguishable targets).
- Supports dropout and noise in observations.
- Estimates object positions using:
  - `KMeans` clustering (`estimatekmeans`)
  - `Gaussian Mixture Models (GMM)` (`estimategaussian`)
- Hungarian algorithm for consistent cluster tracking.
- Smoothed trajectories using exponential filtering.
- Animation generation with `FuncAnimation`.

## Estimation Logic

- Particles represent a full density over space.
- Each frame's particles are clustered into `n_clusters = num_balls`.
- Cluster centers represent estimated positions.
- Hungarian algorithm matches centers to maintain identity over time.
- Smoothing ensures stable motion representation.

## Comparison (KMeans vs. GMM)

- Both methods yield similar center estimates.
- Average centroid difference: ~0.45 units.
- GMM adds robustness in case of uneven particle density.
- KMeans is faster and sufficient for homogeneous data.

##  File Structure

- `UnifiedParticleFilter`: Contains prediction, update, resampling, and clustering methods.
- `simulate_ball_throwing` & `simulate_noisy_observations`: Simulation logic.
- `visualize_multimodal`: Plots all true/obs/est per ball.
- `create_particle_filter_animation`: Saves animation of particle movement and estimates.
- Final comparison plots KMeans vs. GMM centroids.

##  How to Run

```bash
Open `multimodalparticlefilter.ipynb` in Jupyter Notebook and run all cells.
```

## Parameters

- `N_BALLS = 3`: Number of balls
- `num_particles = 20000`: Total particles
- `dropout_prob = 0.15`: Probability of observation loss
- `NOISE_STD = 1.0`: Sensor noise
- `estimatekmeans()` or `estimategaussian()` can be selected for different estimation functions

## Output

- Multi-object estimation plot with dropout indicators
- Animation saved as `particle_filter_animation.gif`
- Silhouette scores comparing clustering quality

##  Example Metrics for Comparison of Different Estimation Functions

- Mean center difference (KMeans vs GMM): `~0.45 units`
- Silhouette Score (KMeans): High for well-separated objects

##  Limitations

In high-dropout or close-proximity scenarios, KMeans/GMM-based estimation may:
- Confuse identities between objects (e.g., flip cluster assignments).
- Produce zigzag or broken trajectories due to center switching.

