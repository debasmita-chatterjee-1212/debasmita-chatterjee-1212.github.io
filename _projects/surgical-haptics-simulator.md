---
layout: page
title: Haptic-Assisted Surgical Simulation
description: "IIT Kharagpur (2022-2023)"
img: assets/img/surgical/liver-loaded.png
importance: 5
category: biomedical
github: https://github.com/debasmita-chatterjee-1212/surgical-simulation-with-haptics
---

Master's research at IIT Kharagpur on interactive surgical simulation. The original work used Unity3D, volume-rendered anatomical models and a Phantom Omni haptic device for force feedback. The [C++ implementation on GitHub](https://github.com/debasmita-chatterjee-1212/surgical-simulation-with-haptics) presents the computational core in CHAI3D: tissue mechanics, instrument contact, incision and numerical verification against analytical solutions. In that version the instrument is mouse-controlled and the reaction forces are computed.

## Tissue modelling

- **Soft tissue.** Liver, kidney, pancreas and ear cartilage are tetrahedral meshes with a linear-elastic tensor-mass formulation, advanced by explicit central-difference integration with a spectral timestep estimate. Precomputed stiffness tensors and multithreading keep it interactive.
- **Bone.** The temporal bone is a rigid voxel model with constrained contact and Hertzian indentation, so the force is estimated without integrating the deformation of stiff bone.
- **Contact and cutting.** A virtual proxy couples the instrument to distributed surface loading. A scalpel mode estimates cutting force from fracture energy and draws a deformation-dependent surface incision.

## Temporal bone

The temporal bone is a voxel model with a Young's modulus of about 10 GPa, so it is treated as rigid. A constrained virtual instrument touches the surface, and the simulation estimates contact force and indentation and logs the interaction data.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/surgical/bone-1.png' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Temporal bone, first view" loading="lazy">
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/surgical/bone-2.png' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Temporal bone, second view" loading="lazy">
  </div>
</div>
<div class="caption">The rigid temporal bone model in the simulator, from two viewpoints.</div>

## Ear cartilage

The ear is a deformable tetrahedral mesh with a Young's modulus of 1.66 MPa, the stiffest of the deformable tissues, so the same probe produces a much smaller deformation here than on the organs below.

<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    <img src="{{ '/assets/img/surgical/cartilage-probe.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Ear cartilage probing" loading="lazy">
  </div>
</div>
<div class="caption">Probing the ear cartilage model.</div>

## Kidney

The kidney (19.6 kPa) is a soft deformable mesh. In probe mode the proxy presses on the surface and the simulation estimates the reaction force and the maximum tissue displacement. In scalpel mode the same tissue is cut with a shallow surface incision, with the cutting force estimated from fracture energy.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/surgical/kidney-probe.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Kidney probing" loading="lazy">
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/surgical/kidney-scalpel.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Kidney incision" loading="lazy">
  </div>
</div>
<div class="caption">Probing (left) and scalpel incision (right) on the kidney.</div>

## Pancreas

The pancreas (1.4 kPa) is the softest tissue modelled, so a light touch produces visible deformation. The same solver is used for every organ, and only the material parameters change.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/surgical/pancreas-probe.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Pancreas probing" loading="lazy">
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/surgical/pancreas-scalpel.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Pancreas incision" loading="lazy">
  </div>
</div>
<div class="caption">Probing (left) and scalpel incision (right) on the pancreas, the softest tissue modelled.</div>

## Liver

The liver (15 kPa) shows how the proxy spreads the instrument load over the surface, giving a smooth, material-dependent deformation. The incision is drawn on the deforming surface and follows the tissue as it moves, while the underlying volumetric mesh is unchanged.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/surgical/liver-probe.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Liver probing" loading="lazy">
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <img src="{{ '/assets/img/surgical/liver-scalpel.gif' | relative_url }}" class="img-fluid rounded z-depth-1" alt="Liver incision" loading="lazy">
  </div>
</div>
<div class="caption">Probing (left) and scalpel incision (right) on the liver.</div>

**Tools:** C++, CHAI3D, OpenGL, Python, Unity3D, Phantom Omni.