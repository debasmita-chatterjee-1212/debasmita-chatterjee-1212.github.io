---
layout: page
title: Cardiac Cine-MRI Analysis and Classification
description: "University of Warwick (2022)"
img: assets/img/mri/lv-contour-chanvese.png
importance: 4
category: biomedical
github: https://github.com/debasmita-chatterjee-1212/cardiac-cine-mri-analysis
---

WMG Summer Research Internship, Warwick Manufacturing Group, University of Warwick. A MATLAB pipeline that segments the left ventricle (LV) in cardiac cine-MRI, extracts functional features, and tests whether they can separate healthy subjects from patients with myocardial infarction (MI). [Code on GitHub](https://github.com/debasmita-chatterjee-1212/cardiac-cine-mri-analysis).

{% include figure.liquid path="assets/img/mri/lv-contour-chanvese.png" title="LV segmentation" class="img-fluid rounded z-depth-1" %}
<div class="caption">A 2-chamber cine frame (left) and its LV segmentation with Chan-Vese active contours (right).</div>

## Pipeline

1. **Image processing.** DICOM import and preprocessing of 2-chamber and 4-chamber cine sequences. The LV is segmented from a manual contour that is refined with Chan-Vese active contours, then tracked across the cardiac cycle with a cyclic propagation scheme and checked frame by frame.
2. **Feature extraction.** End-diastolic and end-systolic LV cavity areas, fractional area change and temporal area-curve features, giving one feature set per patient.
3. **Classification.** Feature selection, then logistic regression, support vector machine and random forest models, evaluated with patient-level validation.
4. **Integration.** The stages are joined into a single workflow that produces a patient-level report.

{% include figure.liquid path="assets/img/mri/endo-epicardium-2ch.png" title="Endocardium and epicardium" class="img-fluid rounded z-depth-1" %}
<div class="caption">Example of endocardial and epicardial contouring on a 2-chamber FIESTA cine frame.</div>

{% include figure.liquid path="assets/img/mri/four-chamber-segmentation.png" title="Four-chamber segmentation" class="img-fluid rounded z-depth-1" %}
<div class="caption">Segmentation panels from an early trial of 4-chamber contouring on a single patient dataset, showing the ventricles, atria and myocardium.</div>

## Result

In exploratory healthy-versus-MI classification on a small patient cohort, the random forest reached 89.4% accuracy on a held-out test set.

**Tools:** MATLAB (Image Processing, Statistics and Machine Learning toolboxes), Segment, RadiAnt DICOM Viewer.