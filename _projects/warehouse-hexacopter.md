---
layout: page
title: Autonomous Indoor Hexacopter for Warehouse Operations
description: "Flipkart GRiD 2.0 Robotics Challenge (2020)"
img: assets/img/hexacopter/hexacopter-cad.png
importance: 2
category: autonomy
---

**Flipkart GRiD 2.0 Robotics Challenge, semi-finalist (2020).** Team project at IIT Kharagpur.

## The problem

Warehouses need frequent inventory counts, item retrieval and package inspection, and these are slow and repetitive when done by hand. We designed a hexacopter that flies indoors without GPS, avoids obstacles, picks up a box, checks it for damage, reads its barcode and delivers it. The only instruction it receives is the start command, sent over Wi-Fi to an onboard Raspberry Pi 3 B+.

{% include figure.liquid path="assets/img/hexacopter/hexacopter-cad.png" title="SolidWorks model of the hexacopter with the adjustable gripper mounted under the frame" class="img-fluid rounded z-depth-1" %}
<div class="caption">SolidWorks model of the hexacopter (550 mm frame) with the two-actuator gripper underneath.</div>

## Platform and performance

The figures below were calculated with eCalc and taken from our submitted design report. After the design round, we also carried out real flight tests; the observations are in the flight testing section below.

| | Full load | No load |
|---|---|---|
| Weight | 3156 g (without payload) | |
| Max payload | 2300 g | |
| Average flight time | 8.4 min | 15.8 min |
| Max tilt | 40° | 60° |
| Max speed | 61 km/h | 84 km/h |
| Climb rate | 5.2 m/s | 11 m/s |

Other specifications: 550 mm frame, 10 x 4.5 inch propellers, peak thrust 14,400 g, 14.8 V 4S1P 16000 mAh battery, and a 130° field-of-view camera. The full bill of materials came to about INR 60,000.

## Autonomy stack

{% include figure.liquid path="assets/img/hexacopter/navigation-architecture.png" title="Mission architecture" class="img-fluid rounded z-depth-1" %}
<div class="caption">Mission flow: grip the payload, map with SOFT-SLAM, locate and pass through gates, then search for the landing pattern and land.</div>

- **Localisation and mapping.** SOFT-SLAM, a stereo visual SLAM method, supplies localisation and a dense map. It is fused with the altimeter and IMU through an Extended Kalman Filter.
- **Trajectory tracking.** A PID controller tracks the planned trajectory. Its gains were to be obtained in MATLAB simulation and tuned to the payload.
- **Gate navigation.** Rectangular gates are found with colour segmentation, cluster filtering and centroid estimation. SIFT features are used for tracking between frames.
- **Obstacle avoidance.** The sensed area is divided into occupancy-grid cells. Rule-based logic sends front, left or right waypoints depending on which neighbouring cells are occupied.
- **Landing.** The drone searches for a unique pattern on the landing pad and lands on it.

{% include figure.liquid path="assets/img/hexacopter/software-modules.png" title="Programming modules" class="img-fluid rounded z-depth-1" %}
<div class="caption">Overview of the software modules (SLAM, gripper adjustment, barcode scanning, damage detection, gate navigation, obstacle avoidance).</div>

## Self-adjusting gripper

Vision-based gripping was my part of the project. The 3D-printed gripper has two independent linear actuators, one per axis, that adjust to the box length and width. An OpenCV pipeline measures the object against a reference of known size. It uses Canny edge detection, closing of gaps in the edges by dilation and erosion, `cv2.findContours` for the object outline, and a pixels-per-metric calibration to get real dimensions. The measurements go to the controller, which drives the actuators. We noted that it cannot adjust for box height, and that an electroadhesive gripper would be a future upgrade.

## Warehouse workflow

{% include figure.liquid path="assets/img/hexacopter/warehouse-workflow.png" title="Warehouse operation workflow" class="img-fluid rounded z-depth-1" %}
<div class="caption">Given an item ID and department, the drone scans barcodes row by row, checks the box for damage, picks it up if it is under 2500 g and 20 cm tall, and delivers it. Failures and damage are reported to the mainframe.</div>

A downward camera reads QR codes and barcodes (ZBar/pyzbar) to update the database, and a VGG-16 classifier was proposed to detect damaged packages from labelled images.

## Structural and design analysis

The frame and weight distribution were to be optimised with static and dynamic stress analysis (SolidWorks, Autodesk Inventor Nastran), run at maximum thrust with full load.

## Flight testing

After the design round we flew the hexacopter. These are qualitative observations from those tests.

- The drone hovered over objects before picking them up.
- Obstacle avoidance worked fairly well.
- The gripper needed a few adjustments during testing and could only grip boxes. Objects with uneven surfaces were out of scope.
- For wider objects the grip was weaker and the flight became unsteady, which fits the payload and stability limits listed below.

## Limitations we identified

- Payload is capped at 2300 g by the motor power limit.
- The camera is fixed, and its fixed focal length means a loaded drone cannot do surveillance.
- The stress analysis only covers the maximum-thrust, full-load case.
- IMU error accumulates over long flights and needs recalibration.
- Gripper printing depended on 3D printer access during COVID-19.

## References

1. Cvišić et al., SOFT-SLAM, Journal of Field Robotics, 2018.
2. López et al., a multi-sensorial SLAM system for low-cost micro aerial vehicles in GPS-denied environments, Sensors, 2017.
3. Schnipke et al., autonomous navigation of a UAV through a GPS-denied indoor environment with obstacles, AIAA Infotech@Aerospace, 2015.
4. Andert et al., autonomous vision-based helicopter flights through obstacle gates, Journal of Intelligent and Robotic Systems, 2010.
