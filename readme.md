# HydrogenAtomSimulations

Explore the quantum mechanics of the **hydrogen atom** with clean, reproducible notebooks: bound states, **radial wavefunctions (R_{n\ell}(r))**, **spherical harmonics (Y_\ell^m(\theta,\phi))**, densities, nodal structure, and selection-rule demos. Includes interactive 1D/2D/3D visualizations.

![Python](https://img.shields.io/badge/Python-3.9+-blue) ![Jupyter](https://img.shields.io/badge/Jupyter-Notebook%2FLab-yellow) ![License](https://img.shields.io/badge/License-MIT-green)

---

## Overview

* **Radial Wave Functions** — Solve the radial Schrödinger equation in the Coulomb potential, compute (R_{n\ell}(r)), and **radial probability** (P_{n\ell}(r)=r^2|R_{n\ell}(r)|^2).
* **Spherical Harmonics** — Visualize (Y_\ell^m) on (S^2); inspect **nodal lines** and phases.
* **Composite Orbitals** — Build (\psi_{n\ell m}(r,\theta,\phi)=R_{n\ell}(r)Y_\ell^m(\theta,\phi)), plot (|\psi|^2), and slice/isosurface views.
* **Units & Scaling** — Natural atomic units by default ((\hbar=m_e=e=a_0=1)); toggle to SI if you enjoy pain.

---

## Physics (what you’re actually computing)

Time-independent Schrödinger in central potential:
[
\left[-\frac{\hbar^2}{2m}\nabla^2-\frac{e^2}{4\pi\varepsilon_0}\frac{1}{r}\right]\psi=E\psi.
]
Separation of variables gives:
[
\psi_{n\ell m}(r,\theta,\phi)=R_{n\ell}(r),Y_\ell^m(\theta,\phi),\quad
E_n=-\frac{13.6~\text{eV}}{n^2}.
]
Closed-form radial (atomic units):
[
R_{n\ell}(r)=\frac{2}{n^2}\sqrt{\frac{(n-\ell-1)!}{(n+\ell)!}},
e^{-r/n}\left(\frac{2r}{n}\right)^\ell
L_{n-\ell-1}^{2\ell+1}!\left(\frac{2r}{n}\right),
]
with orthonormality (\int_0^\infty |R_{n\ell}|^2 r^2,dr=1) and (\int |Y_\ell^m|^2 d\Omega=1).

If your plots don’t respect these, your “results” are trash.

---

## Features

* Exact **analytic** (R_{n\ell}) via associated Laguerre polynomials (SciPy).
* High-res **radial density** and **expectation values** (\langle r \rangle,\langle r^2\rangle).
* **Spherical harmonics** heatmaps + interactive 3D lobes (Plotly).
* Full **(|\psi|^2)** volume/surface visualizations; configurable (n,\ell,m).
* Optional **selection rules** demo: (\Delta \ell=\pm1,\ \Delta m=0,\pm1) (dipole).

---

## Installation

```bash
git clone https://github.com/yourusername/HydrogenAtomSimulations.git
cd HydrogenAtomSimulations
pip install -r requirements.txt
```

**requirements.txt** (typical)

```
numpy
scipy
matplotlib
sympy
ipywidgets
plotly
```

Enable widgets if needed:

```bash
jupyter nbextension enable --py widgetsnbextension
```

---

## Getting Started

```bash
jupyter notebook
```

Open:

* **RadialWaveFunction.ipynb** — plots (R_{n\ell}(r)), (P_{n\ell}(r)), checks normalization.
* **SphericalHarmonics.ipynb** — renders (Y_\ell^m) on the sphere and 3D lobes.
* **HydrogenStates3D.ipynb** *(optional)* — builds (\psi_{n\ell m}) grids and (|\psi|^2) isosurfaces.

---

## Usage Snippets

Compute and plot a radial density:

```python
from src.hydrogen_radial import radial_R, radial_density
import numpy as np
n, l = 3, 1
r = np.linspace(0, 60, 2000)
R = radial_R(n, l, r)                 # atomic units
Pr = radial_density(R, r)             # r^2 |R|^2
```

Plot a spherical harmonic:

```python
from src.spherical_harmonics import Ylm_grid
theta, phi, Y = Ylm_grid(l=2, m=1, n_theta=200, n_phi=400)
```

Build |ψ|² grid (careful with memory):

```python
from src.psi_builder import psi_nlm_grid
psi = psi_nlm_grid(n=2,l=1,m=0, grid=(128,128,128), extent=40.0)
rho = (psi.conj()*psi).real
```

---

## Repro & Tests (so reviewers can’t roast you)

* Atomic units by default; switch **consistently** if using SI.
* **Unit tests**: norms (\int r^2|R|^2dr=1), (\int|Y_\ell^m|^2 d\Omega=1), orthogonality in (\ell,m).
* Grid convergence checks for 3D densities; log parameters in notebook metadata.

---

## Common Pitfalls

* **Wrong measure**: radial norm needs (r^2 dr), not (dr).
* **Phase vs probability**: don’t interpret complex phase as density.
* **Undersampling**: too coarse grids kill nodes; increase points or domain.
* **Mixing units**: AU vs SI—pick one and stick to it.

---

## License

MIT 

---
