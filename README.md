<div align="center">
<h1>KEMAR + VR Headset — Acoustic Beamforming with Mesh2HRTF</h1>
</div>
<br>

**Dr. Stéphane Dedieu** 
<br>Applied Mathematics | Digital Signal Processing | ML  <br>
September 2026  <br>
<a href="https://www.linkedin.com/in/sdedieu/">
  <img src="https://upload.wikimedia.org/wikipedia/commons/c/ca/LinkedIn_logo_initials.png" alt="LinkedIn" width="30" height="30">
</a>

<br>


**Numerical acoustic array processing and 4-microphone MVDR beamforming for KEMAR with a generic VR headset using an open Boundary Element Method code:  Mesh2HRTF.**



<div align="center">

| <p align="center"> <img src="./pictures/Mesh_VR_Kemar_v002.png" alt="Sphere validation" width="40%"> </p> |
| - |
|<p align="center"><i>Open-source CAD & BEM model: KEMAR dummy integrated with a generic VR headset <br> (Source: bloo-audio.com / Bloo Audio Inc.)</i></p>|

</div>



### Notebooks

Notebooks will be added


## Overview

Can a free Burton–Miller + FMM solver (Mesh2HRTF / NumCalc) replace a
closed BEM code for array design on a dummy + headset in the
$100\,\mathrm{Hz}$–$8\,\mathrm{kHz}$ AR/VR band?

This repo publishes the meshes, the transfer functions, and example
MVDR patterns. The optimiser is not included. White-noise gain is
floored at $-25\,\mathrm{dB}$ below $500\,\mathrm{Hz}$, then ramps to
$-30\,\mathrm{dB}$ above $1\,\mathrm{kHz}$.

**Part II** (this page, first) — KEMAR-style dummy + generic VR headset,
four reciprocal point sources on a $2.5\,\mathrm{cm}$ line, far-field
TFs toward $\mathbf{r}=(1,0,0)\,\mathrm{m}$, boundary $|p|$, planar
beampattern, DI and WNG.

**Part I** — rigid sphere $a=0.10\,\mathrm{m}$, Ico-4 versus Morse,
point source $2\,\mathrm{mm}$ off the skin, observers at $10\,\mathrm{m}$.
That run fixes units, standoff, and trust in NumCalc.

**Part III** (later) — near-field beamforming, three vertical microphones.

- Company: [bloo-audio.com/array51](https://www.bloo-audio.com/array51)
- Solver: [Mesh2HRTF / NumCalc](https://github.com/Any2HRTF/Mesh2HRTF)

## Acknowledgements

Mesh2HRTF / NumCalc started at ARI (ÖAW, Vienna) with Harald
Ziegelwanger, Wolfgang Kreuzer and Piotr Majdak, and continues with
Fabian Brinkmann (TU Berlin) and Katharina Pollack (ARI).
<https://github.com/Any2HRTF/Mesh2HRTF>

### Key References

- Ziegelwanger, Majdak, Kreuzer, *"Numerical calculation of head-related transfer functions: A review"* — **J. Acoust. Soc. Am.**, 2015.
- Brinkmann et al., *"A Blender-Based Open-Source Pipeline for Head-Related Transfer Function Calculation"* — **J. Audio Eng. Soc.**, 2023.
- Kreuzer et al., *"An open-source boundary element method solver for acoustics"* — **Eng. Anal. Bound. Elem.**, 2024 (Burton–Miller + FMM).
- Morse & Ingard, *Theoretical Acoustics*, McGraw-Hill / Princeton University Press, 1968/1986.

<br>
<br>


## Part II: Microphone array I — far field MVDR beamforming <br> Fixed look direction $(1,0,0) m$


### 1. KEMAR + VR headset: the CAD file

High-resolution geometry of a KEMAR-style head integrated with a **generic** VR headset.  
Used as the domain for **open-source BEM** (Mesh2HRTF / NumCalc) to compute microphone transfer functions.
This is an in-house concept mesh for open BEM (Mesh2HRTF / NumCalc).
It is **not** a vendor product and is **not** affiliated with any commercial
VR headset.

#### Head and torso

KEMAR-style dummy CAD developed at **ICAR**
(*Infrastructure commune en acoustique pour la recherche*,
ÉTS–IRSST, École de technologie supérieure, Montréal).

#### Headset

A high-quality generic VR-headset CAD by **Chris Leung** on GrabCAD:
https://grabcad.com/chris.leung-5/models

The headset was simplified and edited: the headband was extended around the head, and reduced to about
$5–6\,\mathrm{cm}$ width. The edited headset was then merged with the
KEMAR-style dummy into a single watertight skin.

The working file distributed here is an **STL** surface mesh (plus the
Mesh2HRTF `ObjectMeshes` export).

<div align="center">

| <p align="center"> <img src="./pictures/head001-350x400.png" alt="Four microphone seats" width="150"></p> | <p align="center"> <img src="./pictures/CLeung_VRHeadset_v001.png" alt="Four microphone seats" width="150"></p>   |  <p align="center"> <img src="./pictures/VR_Kemar_TopView.png" alt="Four microphone seats" width="150"></p>    |  <p align="center"> <img src="./pictures/Kemar+VRHeadset_v01.png" alt="Four microphone seats" width="150"></p>    |
| ---  |  ---  |  ---  | ---   |
| <p align="center"> <i> Kemar CAD (and mesh) </i> </p>    |  <p align="center"> <i> C. Leung VR Headset </i> </p>    |<p align="center"> <i> Kemar + VR headset - Top view  </i> </p>    | <p align="center"> <i> Kemar + VR headset - 3D  </i> </p>    |

</div>

#### Coordinates and units

Units are **metres**.

- Origin: midway between the two ear-canal / pinna references.
- $+x$: look-ahead (nose / headset front).
- $+y$: left–right axis through the two ears (sign: state whether $+y$ is
  **left** or **right** when you check in Blender).
- $+z$: up.

The skin is **not** a topological sphere. A gap between the headset strap and the head, just forward of each pinna, makes two handles (homeomorphic to a sphere with two handles, genus 2). BEM treats the surface as a rigid sound-hard boundary; the strap–head tunnels are part of the exterior domain. 

#### What the mesh is for

- beamforming (MVDR / LCMV)
- sound-source localisation
- binaural beamforming
- Ambisonics / array processing on a dummy + headset

Microphone examples in this repo use a small linear subset on one side of the headset (2.5 cm spacing). Reciprocal point sources sit $2\,\mathrm{mm}$ off the skin.

This repository documents **validation** and **far-field TFs** for a 4-microphone subset of the array. Beamforming examples (MVDR, near-field) can be built from these TFs; **the optimizer is not published**.

Company page: [bloo-audio.com/array51](https://www.bloo-audio.com/array51/)

### 2. Linear microphone array — look direction $(1,0,0) m$

Four reciprocal point sources sit $2\,\mathrm{mm}$ off the skin on the right side of the headset, $2.5\,\mathrm{cm}$ apart, on a linear end-fire line. Each NumCalc run is the transfer function $H_m(f;\mathbf{r})$ between seat $m=1,2,3,4$ and the field. 
The design look is the $1\,\mathrm{m}$ station $\mathbf{r}=(1,0,0)\,\mathrm{m}$ ($+x$, nose). The beam is **fixed frontal**: one steering vector toward that point.
The rest of the $\sim 1850$-point unit sphere is only used to plot the pattern and to build the isotropic covariance.

Against a reference FEM/BEM run, magnitude stays within about $0.2\,\mathrm{dB}$; phase matches after the $e^{\pm j\omega t}$
convention (`-angle` on NumCalc). MVDR uses these TFs with a WNG floor of $-25\,\mathrm{dB}$ below $500\,\mathrm{Hz}$, ramping to $-30\,\mathrm{dB}$
above $1\,\mathrm{kHz}$


<div align="center">

|<p align="center"> <img src="./pictures/array51_kemar_VR_headset_mics1234.png" alt="Four microphone seats" width="300"></p>|<p align="center"><img src="./pictures/array51_kemar_VR_headset_pboundary_mics4.png" alt="magnitude(p) on the skin, one point source" width="470"></p>|   
|:---:|:---:|
|<p align="center"> <i> 4 microphones linear array - Configuration  </i> </p>|<p align="center"> <i> Boundary pressure field - mic_4 at 4 kHz </i> </p>|

</div>



Boundary $|p|$ for one $2\,\mathrm{mm}$ point source (vertex-interpolated
display). The optimiser is not published; the TFs are.


---

### 3. Transfer functions at $(1,0,0)$

The four curves are
$20\log_{10}\bigl|4\pi p (f;\mathbf{r}_{\mathrm{look}})\bigr|$
for $m=1,2,3,4$, look $\mathbf{r}=(1,0,0)\,\mathrm{m}$.
Seats and standoff are those of §2.

<div align="center">

|<p align="center"><img src="./pictures/array51_kemar_VR_headset_TFs_mics1234.png" alt="Four microphone seats" width="500"></p>|
|:---:|
| <p align="center"> <i> Transfer Functions - mic1,2,3,4 to point (1,0,0) m on the Unit Sphere </i> </p>   |

</div>

Below $400\,\mathrm{Hz}$, mic\_1 and mic\_2 sit about $0.2\,\mathrm{dB}$
off a reference FEM/BEM run — the same low $ka$ bias as on the rigid sphere in Part I.
That mismatch is enough to wrinkle a superdirective MVDR. The WNG floor
in the next section, is there so the weights follow the physics, not the solver noise.

---

### 4. Computation of MVDR beamforming weights — DI and WNG

The four TFs at $\mathbf{r}_{\mathrm{look}}=(1,0,0)\,\mathrm{m}$ form the
steering vector $\mathbf{d}(f)$. The noise field is taken **isotropic**:
the covariance $\Gamma(f)$ is the Gram matrix of the TFs on the $1\,\mathrm{m}$
evaluation sphere. Standard MVDR ($\mathbf{w}^H\mathbf{d}=1$) is then
diagonally loaded until the white-noise gain stays above $-25\,\mathrm{dB}$.

That floor is a robustness knob, not a performance target. It keeps
$w_{\mathrm{opt}}(f)$ smooth below $500\,\mathrm{Hz}$, where Mesh2HRTF
is about $0.2\,\mathrm{dB}$ off a reference solver, and it stops the
beam from fitting solver noise. Directivity index (DI) and the
*realised* WNG are plotted against frequency for the same weights.

Note: For an $N$-element array in an ideal **free-field** environment, the theoretical maximum directivity index approaches $10 \log_{10}(N^2) \approx 12 dB$, while the maximum white-noise gain scales as $10 \log_{10}(N) \approx 6 dB$ (see, e.g., Gary W. Elko's foundational chapters on microphone array spatial filtering in Digital Signal Processing Handbook).


The linear algebra is in the Appendix. The optimiser itself is not
published; the TFs and the example patterns are.


<div align="center">

|<p align="center"> <img src="./pictures/array51_kemar_VR_headset_MVDR_Wopt.png" alt="mvdr  wopt" width="350"></p>|<p align="center"><img src="./pictures/array51_kemar_VR_headset_MVDR_DI_WNG.png" alt="mvdt di and wng" width="650"></p>|   
|:---:|:---:|
|<p align="center"> <i> MVDR Optimal Weights - Look direction 0 deg  </i> </p>|<p align="center"> <i> Directivity Index & White Noise Gain </i> </p>|

</div>

---

### 5. 3D Directivity Patterns

The following 3D polar plots illustrate the spatial directivity and directional gain of the 4-microphone MVDR beamformer at representative frequencies (200 Hz, 1 kHz, and 4 kHz).

These spatial snapshots provide a direct visual counterpart to the frequency-dependent Directivity Index (DI) and White Noise Gain (WNG) curves shown above, highlighting how spatial selectivity and lobe shaping evolve across the spectrum—from the broad low-frequency response to tighter directional control at higher frequencies.


<div align="center">

| <p align="center"><img src="./pictures/array51_kemar_VR_headset_Beam3D_200Hz.png" alt="MVDR 200 Hz" width="260"></p>  | <p align="center"><img src="./pictures/array51_kemar_VR_headset_Beam3D_1kHz.png" alt="MVDR 1 kHz" width="260"></p>  | <p align="center"><img src="./pictures/array51_kemar_VR_headset_Beam3D_4kHz.png" alt="MVDR 4 kHz" width="260"></p>  |
|:------------------:|:-----------------:|:-----------------:|
|<p align="center"><i> 200 Hz </i></p> | <p align="center"> <i> 1000 Hz </i></p> | <p align="center"><i> 4000 Hz </i></p>  |

</div>

---

### 6. Directivity v. Frequency (Hz) - Horizontal Plane z=0 

The directivity pattern in the $z=0$ plane clearly reveals the onset of spatial aliasing starting around $6.5\text{–}7\text{ kHz}$. This behavior is directly governed by the inter-element microphone spacing of $d = 2.5\text{ cm}$. Following the fundamental spatial Nyquist criterion in a free-field environment ($f_c = c / 2d$, where $c \approx 346\text{ m/s}$), the critical aliasing frequency evaluates to approximately $6.9\text{ kHz}$. Beyond this threshold, the spatial sampling interval exceeds $\lambda/2$, leading to the emergence of unwanted grating lobes and a loss of directional integrity in the horizontal plane.



<div align="center">

|<p align="center"><img src="./pictures/array51_kemar_VR_headset_PlanarDirectivity.png" alt="Four microphone seats" width="500"></p>|
|:---:|
| <p align="center"> <i> Directivity v. Frequency, in the plane Z=0  </i> </p>   |

</div>


---

### 7. Reproducing the BEM run

Project files (`NC.inp`, skin, $1\,\mathrm{m}$ grid) live in `KemarVR_bem/`. <br>

At a fixed frequency the BEM unknown is the boundary field.
NumCalc assembles a square *self-influence* matrix $A(k)$ from the
Helmholtz kernel on this skin (KEMAR + headset). $A$ depends on
geometry and on frequency $k=\omega/c$ only --- not on where the point source
sits. The source position appears in the right-hand side $b(k)$.
Four seats should therefore be four $b$'s and **one** factorisation
of $A(k)$. Today each `source_*` folder still rebuilds $A$.
$A$ cannot be reused from one frequency to the next.

Parameters: 
$c=346.18\,\mathrm{m/s}$, FMM cluster $0.05\,\mathrm{m}$,
standoff $2\,\mathrm{mm}$: see `NC.inp`.


NumCalc is a Unix binary. On Windows, install **WSL2** with Ubuntu
22.04 and open that terminal (not PowerShell). <br>
In the Ubuntu window:

```bash
cd KemarVR_bem/NumCalc
./NumCalc
```

---




<br>
<br> 

## Part I — Validation of the Mesh2HRTF BEM Solver <br> (Rigid Sphere, $a = 0.1\,\mathrm{m}$)

### 1. Overview & Objectives

To establish numerical tolerances and build trust in our BEM workflow, we validate the open-source pipeline against an analytical solution. We compare two related problems that should agree closely on the rigid-sphere boundary and, by reciprocity, at far-field points ($r = 10\,\mathrm{m}$):
- **Analytical scattering** of a plane wave (Morse & Ingard solution).
- **Reciprocal point source** placed a few millimeters outside the skin (Mesh2HRTF / NumCalc), acting as a stand-in for a surface microphone.

Evaluations span $100\,\mathrm{Hz}$ to $8\,\mathrm{kHz}$ with pressure magnitude $\vert{}p\vert{}$ reported across meridional angles ($0^\circ$ to $180^\circ$).

We compare two related but distinct problems that should agree closely
on the rigid-sphere boundary and, by reciprocity, at far-field points
at \(r = 10\,\mathrm{m}\):

- analytical scattering of a plane wave (Morse);
- a point source placed a few millimetres outside the skin (Mesh2HRTF / NumCalc),
  used as a reciprocal stand-in for a surface microphone.

$|p|$ is reported on the unit sphere at $0^\circ, 30^\circ, 60^\circ, 90^\circ, 120^\circ, 150^\circ, 180^\circ$.
Overall the match is excellent from $100\,\mathrm{Hz}$ to $8\,\mathrm{kHz}$

---

### 2. BEM Mesh & Solver Configuration

The rigid sphere and its icosahedral (Ico) mesh were generated in Blender and exported via `mesh2input` to create the NumCalc input files (`NC.inp`).

- **Mesh Resolution:** 4 subdivisions yielding **5120 triangular elements** and **2562 nodes**. 
- **Mean Edge Length:** $h \approx 7.53\,\mathrm{mm}$ ($\approx 7.5\,\mathrm{mm}$).
- **Sound Speed:** $c = 346.18\,\mathrm{m/s}$.
- **Frequency Limit ($\lambda/6$ rule):** 
  $$f_{\lambda/6} = \frac{c}{6h} \approx 7.7\,\mathrm{kHz}$$



####  BEM mesh

At $8\,\mathrm{kHz}$ the mesh is slightly coarser than $\lambda/6$ ($\approx\lambda/5.75$). Burton–Miller collocation BEM often needs **more than
six elements per wavelength** at high $ka$, so part of the residual mismatch above $ka \approx 10$ ($\approx 5.5\,\mathrm{kHz}$) — in particular the
$0.2\,\mathrm{dB}$ drop at $0^\circ$ and $30^\circ$ toward $6$–$8\,\mathrm{kHz}$ — is consistent with discretisation / quadrature rather than a geometry error. A five-subdivision Ico mesh ($20\,480$ faces, $h \approx 3.8\,\mathrm{mm}$) would put $\lambda/6$ well above $8\,\mathrm{kHz}$ if a tighter high-frequency check is required.

#### Solver Engine (NumCalc)

NumCalc solves the Helmholtz equation with a **Burton–Miller collocation BEM**.
Optionally the **multilevel fast multipole method (ML-FMM)** replaces
element-to-element coupling by cluster-to-cluster coupling.
We used ML-FMM (cluster diameter 0.05 m). Changing it to 0.025 m did not
change the look-direction TFs on this mesh.

NumCalc solves the Helmholtz equation using a **Burton–Miller collocation BEM**, optionally accelerated by the **Multilevel Fast Multipole Method (ML-FMM)** for cluster-to-cluster coupling. 
- *Working configuration:* ML-FMM with a cluster diameter of $0.05\,\mathrm{m}$ (changing this to $0.025\,\mathrm{m}$ showed no noticeable change on the look-direction transfer function).

<div align="center">

| <p align="center"> <img src="./pictures/Blender_Sphere_BEMV02.png" alt="Sphere validation" width="55%"> </p> | <p align="center"> <img src="./pictures/Sphere_PointSource_1kHz.png" alt="Sphere validation" width="90%"> </p> |
| :---: | :---: |
| <p align="center"> <i> BEM model - Rigid Sphere, radius $a = 0.1\,\mathrm{m}$ <br> 5120 triangular elements, 2562 nodes (Blender) </i> </p> | <p align="center"> <i> Mesh2HRTF: Pressure field on boundary at $1\,\mathrm{kHz}$ <br> point source at $(0.102, 0, 0)\,\mathrm{m}$ </i> </p> |

</div>

---

### 3. Point-Source Standoff Tuning

The Wiki guideline suggests a source standoff $\geq 0.3\,\mathrm{mm}$ outside the skin, while Kreuzer recommends approximately one mean edge length. We scanned **$5\,\mathrm{mm}$, $2\,\mathrm{mm}$, and $1\,\mathrm{mm}$** along the $+x$ axis:

<div align="center">

| Standoff | $x$-position | High-Frequency Behavior | Low-Frequency Behavior |
| :---: | :---: | :--- | :--- |
| **$5\,\mathrm{mm}$** | $0.105\,\mathrm{m}$ | Drop above $5\,\mathrm{kHz}$ ($0^\circ$ and $30^\circ$) | Good |
| **$2\,\mathrm{mm}$** | $0.102\,\mathrm{m}$ | **Best**, near $6\,\mathrm{dB}$ baffle step | Good |
| **$1\,\mathrm{mm}$** | $0.101\,\mathrm{m}$ | Crushed above $3\,\mathrm{kHz}$ (~$5.5\,\mathrm{dB}$ at $7\text{–}8\,\mathrm{kHz}$) | Best LF collapse to $0\,\mathrm{dB}$ |

</div>

> **Working Choice:** **$2\,\mathrm{mm}$**. This exact offset is carried over later for the VR headset microphone positions.

---

### 4. Results and residual discrepancies

We compare two fields that reciprocity says should agree closely:

- the analytical rigid-sphere scattering of a plane wave (Morse []);
- a Mesh2HRTF / NumCalc BEM run with a point source $2\,\mathrm{mm}$ outside the skin, pressure sampled at $r = 10\,\mathrm{m}$ from $0^\circ$ to $180^\circ$ in a meridional plane.

The two problems are not identical, but the far-field patterns should match. They do, to a fraction of a decibel over most of the $100\,\mathrm{Hz}$–$8\,\mathrm{kHz}$ band.


|<p align="center"> <img src="./pictures/Sphere_PresPlaneWav_001.png" alt="Sphere validation" width="80%">  </p>  |<p align="center"> <img src="./pictures/Sphere_TFs_FarField_001.png" alt="Sphere validation" width="90%">  </p> |
|                              ---                                               |  -----   |
| <p align="center"> <i> Analytical Model - Sound pressure on the sphere <br> Plane  z=0  - Various angles </i> </p>   |    <p align="center"> <i> mshr2HSRTF BEM Model - Sound pressure at 10 m  <br> Plane  z=0  - Various angles </i>        </p>              |

At $ka \approx 0.1$ the Mesh2HRTF far-field samples at $r = 10\,\mathrm{m}$ are:

<div align="center">

| Angle | $\|p\|$ | Level re $0^\circ$ |
|---|---|---|
| $0^\circ$ | $1.0212\times 10^{-1}$ | $0.00\,\mathrm{dB}$ |
| $30^\circ$ | $1.0161\times 10^{-1}$ | $-0.04\,\mathrm{dB}$ |
| $60^\circ$ | $1.0044\times 10^{-1}$ | $-0.14\,\mathrm{dB}$ |
| $90^\circ$ | $9.9313\times 10^{-2}$ | $-0.24\,\mathrm{dB}$ |
| $120^\circ$ | $9.8742\times 10^{-2}$ | $-0.29\,\mathrm{dB}$ |
| $150^\circ$ | $9.8670\times 10^{-2}$ | $-0.30\,\mathrm{dB}$ |

</div>

The angular spread is **$0.30\,\mathrm{dB}$**. The analytical plane-wave solution (and the reference BEM) is essentially isotropic at this $ka$. The bias is therefore numerical: Burton–Miller collocation and FMM / quadrature at low frequency, not the $2\,\mathrm{mm}$ standoff and not the $10\,\mathrm{m}$ station.

**Low frequency** (\(50\)–\(100\,\mathrm{Hz}\), \(ka \approx 0.1\)–\(0.2\)).  
The analytical field is essentially isotropic (\(\sim 0\,\mathrm{dB}\) spread across angles).
Mesh2HRTF shows a slightly larger angular variance, about \(0.3\)–\(0.4\,\mathrm{dB}\).

**High frequency** (around \(ka = 10\)).  
On the illuminated side (\(0^\circ\) and \(30^\circ\)) the computed amplitude sags by about \(0.2\,\mathrm{dB}\).
The same droop appears in other Mesh2HRTF validations. Likely causes are the
Burton–Miller discretisation, FMM clustering, and/or the quadrature — not the
geometry itself.

**Parameters that do *not* move the look-direction TF.**  
FMM cluster diameter \(0.05\,\mathrm{m}\) vs \(0.025\,\mathrm{m}\) has no significant effect
on this Ico-5 mesh.

**Parameter that *does* matter.**  
Point-source standoff from the skin. After a \(5 / 2 / 1\,\mathrm{mm}\) scan on \(+x\),
**\(2\,\mathrm{mm}\)** is the working choice (clean high-frequency baffle step,
acceptable low-frequency collapse). The same offset is used later for headset
microphone positions.


- **Mesh Topology (Ico vs. UV):** Elongated polar triangles on UV meshes distort low frequencies ($100\,\mathrm{Hz}$) and pole calculations; Ico triangulation avoids this entirely.
- **Piston vs. Point Sources:** A piston radiator requires area factor $S$, whereas a point source uses $P_0 = 1$ (i.e., $e^{ikR}/(4\pi R)$). At $1\,\mathrm{m}$, $20\log_{10}(4\pi) \approx +22\,\mathrm{dB}$ is required to reach $1\,\mathrm{Pa}$.
- **High-Frequency Discretization:** At $8\,\mathrm{kHz}$, the mesh is slightly coarser than $\lambda/6$ ($\approx\lambda/5.75$). Burton–Miller collocation requires adequate elements per wavelength at high $ka$, meaning the minor $\sim 0.2\,\mathrm{dB}$ drop at $0^\circ$ and $30^\circ$ toward $6\text{–}8\,\mathrm{kHz}$ stems from numerical quadrature rather than geometry error. (A 5-subdivision mesh with $20\,480$ faces would push $\lambda/6$ past $8\,\mathrm{kHz}$).

---

### 5. Practical Summary & Takeaways

Treat Mesh2HRTF as a solid open BEM tool for research and array design —
MVDR / LCMV, binaural beamforming, and SSL — in the **\(100\)–\(8000\,\mathrm{Hz}\)**
band that matters for AR/VR devices. Use a \(\sim 2\,\mathrm{mm}\) reciprocal
point source for surface microphones, keep an eye on the low-frequency angular
spread and the mild high-frequency look-direction loss, and add a targeted
check when a new mesh or frequency grid is introduced.

More mature commercial BEM codes pass the $ka \approx 0.1$ test to a few hundredths of a dB. Mesh2HRTF does not: the $0.3\,\mathrm{dB}$ front-to-back tilt is a low-frequency discretisation / quadrature error.

That matters for **low-frequency array design**. In a superdirective beamformer (MVDR, LCMV) a few tenths of a dB of false magnitude — and the associated phase — change the white-noise gain and the realised directivity. Treat Mesh2HRTF TFs below a few hundred hertz with extra regularisation, or cross-check that band with another solver, before freezing weights.

<br>
<br>












## References

Ziegelwanger, Majdak, Kreuzer, JASA 2015  
Pipeline Mesh2HRTF : Brinkmann et al., JAES 2023  
NumCalc (solver) : Kreuzer et al., Eng. Anal. Bound. Elem. 2024 — Burton–Miller + FMM
Morse and Ingard, "Theoretical Acoustics" (1968)

[4] P. M. Morse and K. U. Ingard, *Theoretical Acoustics*,
Princeton University Press, Princeton, NJ, 1986
(reprint of the 1968 McGraw-Hill edition).
ISBN 0-691-02401-4.


##  APPENDIX:  Robust Beamforming: MVDR, WNG Constraint & LCMV

### 1. Problem Formulation – Standard MVDR

We seek the optimal beamformer weights $\mathbf{w}(f)$ that minimize the output noise power while enforcing a distortionless response in the look direction $\mathbf{d}(f)$:

$$
\min_{\mathbf{w}} \quad \mathbf{w}^H \mathbf{R}_{vv}(f) \mathbf{w}
\quad \text{subject to} \quad \mathbf{w}^H \mathbf{d}(f) = 1
$$

The closed-form solution is the classic MVDR beamformer:

$$
\mathbf{w}_{\text{MVDR}}(f) = \frac{\mathbf{R}_{vv}(f)^{-1} \mathbf{d}(f)}{\mathbf{d}^H(f) \mathbf{R}_{vv}(f)^{-1} \mathbf{d}(f)}
$$

### 2. White Noise Gain (WNG) Constraint

In practice, the pure MVDR solution is often overly sensitive to sensor noise, calibration errors and steering vector mismatches.  
To improve robustness we impose a **White Noise Gain** constraint:

$$
\text{WNG}(f) = \frac{|\mathbf{w}^H \mathbf{d}|^2}{\mathbf{w}^H \mathbf{w}} \geq \text{WNG}_{\min}
$$

This is equivalent to limiting the $\ell_2$-norm of the weight vector.

#### Practical realisation – Diagonal Loading

A simple and effective way to enforce the WNG constraint is **diagonal loading** (Tikhonov regularisation):

$$
\mathbf{w}(f,\alpha) = \frac{ \bigl(\mathbf{R}_{vv}(f) + \alpha \mathbf{I}\bigr)^{-1} \mathbf{d}(f) }
{ \mathbf{d}^H(f) \bigl(\mathbf{R}_{vv}(f) + \alpha \mathbf{I}\bigr)^{-1} \mathbf{d}(f) }
$$

- $\alpha = 0$ → pure MVDR (highest directivity, lowest robustness)
- $\alpha \to \infty$ → approaches Delay-and-Sum (highest robustness, lower directivity)

By sweeping the loading factor $\alpha$ (or $\sigma^2$) we obtain the classic trade-off between Directivity Index (DI) and White Noise Gain (WNG).

### 3. Linearly Constrained Minimum Variance (LCMV)

When more than one spatial constraint is required we generalise MVDR to the **LCMV** beamformer.

We now enforce a set of linear constraints:

$$
\mathbf{C}^H \mathbf{w} = \mathbf{g}
$$

where
- $\mathbf{C} = [\mathbf{d}_0,\; \mathbf{d}_{180},\; \dots]$ contains the steering vectors of the constrained directions,
- $\mathbf{g}$ is the desired response vector (e.g. $[1, 0]^T$ for look-direction distortionless + null at 180°).

The closed-form LCMV solution is:

$$
\mathbf{w}_{\text{LCMV}} = \mathbf{R}_{vv}^{-1}\mathbf{C}\bigl(\mathbf{C}^H\mathbf{R}_{vv}^{-1}\mathbf{C}\bigr)^{-1}\mathbf{g}
$$

With diagonal loading the same regularisation principle applies:

$$
\mathbf{w}_{\text{LCMV}}(\alpha) = (\mathbf{R}_{vv}+\alpha\mathbf{I})^{-1}\mathbf{C}
\Bigl(\mathbf{C}^H(\mathbf{R}_{vv}+\alpha\mathbf{I})^{-1}\mathbf{C}\Bigr)^{-1}\mathbf{g}
$$

Typical use-cases on the Kemar + VR Headset array:
- **Look beamformer**: $\mathbf{g}=[1,0]^T$ (distortionless at 0°, null at 180°)
- **Noise-channel beamformer**: $\mathbf{g}=[0,1]^T$ (null at 0°, distortionless at 180°)

### 4. Directivity Index (DI) on the Sphere

The Directivity Index quantifies how much the array concentrates energy in the look direction relative to an isotropic response.

**Continuous definition** (exact theoretical expression):

$$
\text{DI}(f) = 10\log_{10}\left(\frac{4\pi\,|\mathbf{w}^H\mathbf{d}_0|^2}{\displaystyle\int_{S^2}|\mathbf{w}^H\mathbf{d}(\Omega)|^2\,d\Omega}\right)
$$

where the integral is performed over the unit sphere $S^2$ and $d\Omega=\sin\theta\,d\theta\,d\phi$.

**Discrete approximation** used with the 2522-point COMSOL sphere:

$$
\text{DI}(f) \approx 10\log_{10}\left(\frac{N}{\displaystyle\sum_{i=1}^{N}|\mathbf{w}^H\mathbf{d}_i|^2}\right)
$$

or, more accurately with the surface element:

$$
\text{DI}(f) \approx 10\log_{10}\left(\frac{4\pi}{\Delta\Omega\displaystyle\sum_{i=1}^{N}|\mathbf{w}^H\mathbf{d}_i|^2\sin\theta_i}\right)
$$

where $N=2522$ and $\Delta\Omega$ is the solid-angle element corresponding to the spherical grid.  
When the look-direction constraint $\mathbf{w}^H\mathbf{d}_0=1$ is enforced, the numerator simplifies to $4\pi$ (continuous) or $N$ (discrete uniform weighting).



### 5. The Pareto Front

When optimising two conflicting objectives (Directivity Index versus White Noise Gain) the set of optimal trade-off solutions forms the **Pareto front**.

- Any point on the front is optimal: one metric cannot be improved without degrading the other.
- Points above/left of the front are impossible.
- Points below/right of the front are sub-optimal.

In our implementation the Pareto front is traced simply by sweeping the diagonal-loading parameter $\alpha$ (or $\sigma^2$) and recording the resulting (DI, WNG) pairs.





