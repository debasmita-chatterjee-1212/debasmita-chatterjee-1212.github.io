---
layout: page
title: Vision-Based UAV Pursuit Control
description: "University of Alberta (2022)"
img: assets/img/uav/annotation-keypoints-1.png
importance: 6
category: autonomy
github: https://github.com/debasmita-chatterjee-1212/uav_pursuit_control
---

Mitacs Globalink Research Internship, Department of Mechanical Engineering, University of Alberta. 

The project investigated autonomous pursuit between two UAVs: a pursuer detects, localises and tracks a target UAV from visual observations using closed-loop motion control. [Code on GitHub](https://github.com/debasmita-chatterjee-1212/uav_pursuit_control).

The work covered both the hardware and software sides of the drones: mounting and calibrating cameras on real Parrot platforms, flying them with a Vicon motion-capture system, and writing the control, pose-processing and flight software that ran them.

## System

The research framework combined five parts: monocular UAV detection and relative 3D position estimation, state processing from a Vicon motion-capture system, moving-reference generation from relative pose, PID and linear model predictive control, and ROS and MAVLink flight execution. It was developed on Parrot AR.Drone 2.0 and ANAFI platforms.

## My contributions

- **Motion control.** Modular PID control in C++ and ROS for position and yaw, with world-to-body frame transformation, an interactive interface for tuning 12 gains, and CSV logging of experiments.
- **Pose processing.** Quaternion-to-Euler conversion and coordinate transformations for the Vicon pose data.
- **Flight operations.** MAVSDK setup and flight, takeoff and landing routines, evaluated in PX4 and jMAVSim simulation.
- **Perception data.** Annotation of in-flight imagery of the target UAV with keypoints, component labels and bounding boxes (LabelMe), developed a pipeline for identification of the same by an UAV in-flight, used to investigate monocular 3D detection.
- **Neural networks for 3D detection.** Worked as a team in the lab to implement neural networks that convert 2D bounding boxes to 3D bounding boxes in real time.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/uav/annotation-keypoints-1.png" title="Annotated frame" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/uav/annotation-keypoints-2.png" title="Annotated frame" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">Frames of the target UAV captured by the pursuer in flight, shown with keypoints and a bounding box from the lab's bounding-box pipeline.</div>

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/uav/drone-segmentation.png" title="Labelled drone" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/uav/vicon-tracker.png" title="Vicon Tracker" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">Left: a drone detected in a frame captured by another UAV while both were flying. Right: the Vicon Tracker software used for motion-capture ground truth.</div>

## Results

The final results were based on indoor experiments with Vicon ground truth. For the broader framework, it was a monocular detector with 84.6% average precision (IoU 0.5) on about 2,000 annotated frames, depth errors of 8-12 cm at 3-5 m, and relative position estimates within about 10 cm of Vicon over short pursuit runs.

**Tools:** C++, Python, ROS, OpenCV, MAVSDK, MAVROS, PX4, jMAVSim, Vicon, LabelMe.