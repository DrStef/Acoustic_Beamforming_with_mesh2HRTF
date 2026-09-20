# KEMAR + VR headset — BEM transfer functions with Mesh2HRTF

Numerical TFs for a 4-microphone array on a dummy + headset. Open BEM (Mesh2HRTF / NumCalc). Comparison with a reference FEM/BEM model. Beamforming examples use these TFs; the optimizer itself is not published.

# KEMAR + VR headset: Mesh2HRTF BEM

High-resolution geometry of a KEMAR-style head integrated with a **generic** VR headset.  
Used as the domain for **open-source BEM** (Mesh2HRTF / NumCalc) to compute microphone transfer functions.

This repository documents **validation** and **far-field TFs** for a 4-microphone subset of the array.  
Beamforming examples (MVDR, near-field) can be built from these TFs; **the optimizer is not published**.

Company page: [bloo-audio.com/array51](https://www.bloo-audio.com/array51/)

## What this model is for

- Array design on a dummy + headset (diffraction, shadowing)
- Far-field TFs toward a 1 m sphere (and optional 10 m grid)
- Inputs for MVDR / LCMV / Ambisonics / SSL — you bring the weights

## What we computed here

- Reciprocal **point sources** at four headset microphone positions (right-side linear array, 2.5 cm spacing)
- Evaluation on a 1 m sphere (~1850 points)
- Check against a reference FEM/BEM model: magnitude within ~0.2 dB, phase matched after the \(e^{\pm j\omega t}\) convention (`-angle` on NumCalc)









## Contents

1. Sphere / piston and point-source **validation** (Ico mesh)
2. **Nahoom** TFs (KEMAR + generic VR)
3. Figures (look-direction TFs, example directivity balloons)

Mesh2HRTF: [mesh2hrtf.org](https://mesh2hrtf.org/)
