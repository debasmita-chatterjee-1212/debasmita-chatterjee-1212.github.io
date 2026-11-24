---
layout: page
title: Risk-Aware Trajectory Planner
description: "IIT Kharagpur (2021)"
img: assets/img/risk-aware/dashboard.png
importance: 3
category: autonomy
github: https://github.com/debasmita-chatterjee-1212/risk_aware_planner
---

A simulation-based trajectory planner for autonomous highway driving that chooses manoeuvres by estimated collision risk under uncertain traffic. [Code on GitHub](https://github.com/debasmita-chatterjee-1212/risk_aware_planner).

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    <img src="{{ '/assets/img/risk-aware/planner_demo.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Planner simulation">
    <div class="caption">The ego vehicle evaluates four candidate manoeuvres and follows the one with the lowest weighted cost.</div>
  </div>
</div>

## Approach

1. **Environment.** A three-lane highway (3.5 m lanes) with the ego vehicle, starting at 20 m/s, and six surrounding vehicles. The planning horizon is 3 s with a 0.1 s timestep.
2. **Candidate manoeuvres.** Lane keeping, braking, left lane change and right lane change.
3. **Stochastic traffic prediction.** Monte Carlo sampling of the surrounding vehicles' trajectories to account for uncertainty.
4. **Collision risk.** Axis-aligned bounding box collision checks and minimum-distance evaluation across the horizon, giving a collision probability for each manoeuvre.
5. **Selection.** A multi-objective score combining collision probability, proximity to other vehicles and comfort. The lowest-cost manoeuvre is chosen.

{% include figure.liquid path="assets/img/risk-aware/dashboard.png" title="Planning dashboard" class="img-fluid rounded z-depth-1" %}
<div class="caption">Planning dashboard for one illustrative scenario (50 rollouts per candidate): speed profile, collision probability and minimum distance of the selected trajectory, scores of all four candidates, and the initial traffic layout.</div>

## Evaluation

I tested the planner on 200 simulated scenarios in standard and challenging traffic, using 100 Monte Carlo rollouts per candidate manoeuvre (80,000 rollouts in total). In standard traffic no collisions were observed. In challenging traffic, the planner's selected manoeuvres had a mean estimated collision probability of 44.59%, against 74.83% for fixed lane keeping, a 40.4% relative reduction, and the mean minimum distance to other vehicles rose from 1.38 m to 3.06 m. These are model-based risk estimates from simulation.

**Tools:** Python, NumPy, Matplotlib, Monte Carlo simulation.