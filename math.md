# Structured Wave Geometry (SWG) — Canonical Mathematics

This document defines the **canonical mathematics** of Structured Wave Geometry (SWG) and its diagnostic projection, Structured Coherence Geometry (SCG).
It presents the governing PDEs, conserved energy laws, coherence invariants, projection geometry, and barrier functions.
This is the authoritative and domain-agnostic reference.

---

## 1. Primitives

An SWG system is defined by three fields:

* **Amplitude**

  $$
  A(x,t)\ge 0
  $$
* **Phase**

  $$
  \theta(x,t)\in\mathbb{R}\pmod{2\pi}
  $$
* **Memory field**

  $$
  \chi(x,t)\in\mathbb{R}
  $$

### Curvature as memory diagnostic

Curvature $\kappa(x,t)$ is **not a primitive**. It is the **diagnostic face of memory**: persistent $\nabla\chi$ appears in projection as curvature.

* Memory stores shape/persistence.
* Curvature measures the stored shape.
  When $\chi$ is observable, derive curvature directly from $\chi$; when $\chi$ is latent, instantiate curvature on available fields (amplitude, embeddings, spectrograms) that track the same coherence geometry.

---

## 2. Governing Equations (second-order in time)

> **Assumptions.** Periodic or Neumann boundary conditions; fields $A,\theta,\chi$ sufficiently smooth (e.g., $H^2$) for EL calculus; damping coefficients non-negative.
> $\varepsilon$ denotes a universal small positive regularizer, applied in denominators for both $A$ and $\chi$ unless otherwise noted ($\chi_\varepsilon = \chi + \varepsilon$).

### 2.1 Amplitude equation

$$
\begin{aligned}
A_{tt} - A(\theta_t)^2
&= \nabla\!\cdot(\chi^{-1}\nabla A)
- \frac{A}{\chi}\,|\nabla\theta|^2
- \lambda A^3
-\frac{\alpha A^5(\alpha A^2+3)}{2(1+\alpha A^2)^3} \\
&\quad
-\frac{\hbar^2}{2m}\,\frac{\nabla^2 A}{A+\varepsilon}
+\frac{\hbar^2}{4m}\,\frac{|\nabla A|^2}{(A+\varepsilon)^2}
- \beta(\chi-\rho)A
- \gamma_A A_t \, .
\end{aligned}
$$

### 2.2 Phase equation

Expanded inertial form:

$$
2A A_t\,\theta_t + A^2 \theta_{tt}
= \nabla\!\cdot\!\big(A^2 \chi^{-1}\nabla \theta\big)
- \nu_\theta \theta_t
$$

Equivalent conservative form (numerics-friendly):

$$
\boxed{(A^2 \theta_t)_t
= \nabla\!\cdot\!\big(A^2 \chi^{-1}\nabla \theta\big)
- \nu_\theta \theta_t}
$$

### 2.3 Memory field equation

$$
\chi_{tt}
= c_\chi^2 \nabla^2 \chi
- \kappa_\chi(\chi-\rho)
- \mu(\chi-\rho)^3
+ \gamma_\psi A^2
- \tfrac{\beta}{2}A^2
+ \tfrac{1}{2\chi^2}\big(|\nabla A|^2 + A^2|\nabla\theta|^2\big)
- \gamma_\chi \chi_t \, .
$$

> *Back-reaction term:* the final fraction couples amplitude/phase gradients into the memory field.

> **Note.** Coefficient placement in the Bohm and saturation terms follows the **canonical representative** used for EL consistency; SWT drafts used algebraically equivalent variants.

---

## 3. Energy Functional (Euler–Lagrange consistent)

SWG is deterministic and energy-conserving in the undamped, variational-coupling case.

$$
E_{\text{total}}(t)=E_\psi(t)+E_\chi(t)
$$

### 3.1 Amplitude–phase energy

$$
E_\psi=\int\!\Big[
\tfrac12 A_t^2
+ \tfrac12 A^2(\theta_t)^2
+ \tfrac{1}{2\chi}\big(|\nabla A|^2 + A^2|\nabla\theta|^2\big)
+ \tfrac{\lambda}{4}A^4
+ \tfrac{\alpha}{4}\,\frac{A^6}{(1+\alpha A^2)^2}
+ \tfrac{\hbar^2}{4m}\,\frac{|\nabla A|^2}{A+\varepsilon}
\Big]\,dx
$$

> **Note.** The Bohm and saturation terms are written here in their **canonical representative, EL-consistent form**. SWT texts used slightly different placements; this is the authoritative choice.

### 3.2 Memory energy

$$
E_\chi=\int\!\Big[
\tfrac12 \chi_t^2
+ \tfrac{c_\chi^2}{2}|\nabla\chi|^2
+ \tfrac{\kappa_\chi}{2}(\chi-\rho)^2
+ \tfrac{\mu}{4}(\chi-\rho)^4
- \gamma_\psi A^2\chi
+ \tfrac{\beta}{2}(\chi-\rho)A^2
\Big]\,dx
$$

### 3.3 Conservation law

In the undamped case with **variational couplings** (e.g., energy includes $+\tfrac{\beta}{2}(\chi-\rho)A^2$ and $\delta=0$):

$$
\frac{d}{dt}E_{\text{total}}(t)=0.
$$

With damping or non-variational couplings, $\tfrac{d}{dt}E_{\text{total}}\le 0$.

---

## 4. Amplitude Bound

The rational saturation enforces the invariant set

$$
0\;\le\;A(x,t)\;\le\;A_{\max}=\sqrt{\tfrac{1}{\alpha}}.
$$

---

## 5. SCG Projection

SCG projects $(A,\theta,\kappa)$ into coherence space.

### 5.1 Stable phase frame

Compute phases in a **stable 2-D reference frame** (e.g., PCA basis) before projection.

> **Why:** Without a stable frame, diagnostics such as the phase order parameter $R$ drift artificially. Stable frame selection is performed once per analysis window; drift re-estimation resets only at window boundaries.

### 5.2 SCG-3 coordinates (per window $W$)

$$
w_i=\frac{A_i}{\sum_j A_j+\varepsilon},\qquad
x=\sum_i w_i\cos\theta_i,\quad
y=\sum_i w_i\sin\theta_i,\quad
z=\sum_i w_i\,\kappa_i.
$$

*When $\chi$ is observable, use χ-curvature for $\kappa_i$; otherwise use the modality-specific curvature from §6.*

---

## 6. Curvature Operators (faces of memory)

**Preferred χ-curvature (when available).**

* Temporal:

  $$
  \kappa_\chi[i]=\frac{|\chi_{i+1}-2\chi_i+\chi_{i-1}|}{|\chi_i|+\varepsilon}
  $$
* Spatial:

  $$
  \kappa_\chi=\frac{|\nabla^2\chi|}{|\chi|+\varepsilon}
  $$

**Modality-specific (when $\chi$ is latent).**

* **Time series (amplitude):**

  $$
  \kappa_t[i]=\frac{|A_{i+1}-2A_i+A_{i-1}|}{A_i+\varepsilon}
  $$
* **Embeddings:**

  $$
  \kappa_{\text{kNN}}(i)=\Big\|E_i-\frac{1}{|N(i)|}\sum_{j\in N(i)}E_j\Big\|
  $$
* **Spectrograms:** 2-D Laplacian of STFT magnitude (mid-band averaged).

---

## 7. Invariants

Two classes:

* **PDE-derived invariants** (from conservation/boundedness).
* **Canonical operational invariants** (fixed diagnostics, not Noether-conserved).

### Defaults (diagnostic conventions)

$$
\varepsilon=10^{-8},\quad A_{\min}=10^{-4},\quad k=12,\quad \alpha_{\text{MAD}}=2.0.
$$

### 7.1 PDE-derived

* **Energy conservation** $E_{\text{total}}(t)$
* **Amplitude bound** $A_{\max}=\sqrt{1/\alpha}$

### 7.2 Canonical operational

* **Curvature variance**
  $\sigma_\kappa^2(W)=\operatorname{Var}\{\kappa_i\}_{i\in W}$ (dispersion of memory curvature)
* **Coherence entropy**

  $$
  \mathcal{H}(W)=-\sum_{i\in W}p_i\log(p_i+\varepsilon),\quad p_i=\frac{A_i}{\sum_j A_j+\varepsilon}
  $$
* **Phase dislocation density**
  **Tier-1 (canonical):**

  $$
  \rho_\phi(W)=\frac{\sum_{i\in W}\mathbf{1}\big(|\Delta_i|>\text{med}+\alpha_{\text{MAD}}\text{MAD}\big)\,A_i}{\sum_{i\in W}A_i+\varepsilon},\quad
  \Delta_i=\operatorname{wrap}(\theta_i-\theta^{\text{mean}}_i)
  $$

  where:
  • $\operatorname{wrap}(\phi)$ maps to principal value in $(-\pi,\pi]$.
  • $N(i)$ is k-NN in the **stable phase frame** (default $k=12$).
  • Apply amplitude gate $A_{\min}$ to center and neighbors.
  **Minimal (toy):** $\rho_\phi=\tfrac{1}{|W|-1}\sum_i \mathbf{1}(|\operatorname{wrap}(\theta_{i+1}-\theta_i)|>\tau)$.
  **Tier-2 (advanced):** vortex/phase-slip counting (loops with small $A$ and $2\pi$ winding).
* **Energy flux**
  **Tier-1 (operational):**

  $$
  \Phi_E^{\text{op}}=\tfrac{1}{|W|}\sum_i \frac{dE_i/dt}{E_i+\varepsilon},\quad E=A^2
  $$

  *Here $E=A^2$ is the local amplitude-squared, used as a proxy for local energy density (not the full functional energy).*
  **Tier-2 (advanced):**

  $$
  \Phi_E^{\text{loc}}=\tfrac{1}{|W|}\sum_i \frac{\partial_t e_i}{e_i+\varepsilon}
  $$

  where $e_i$ is the canonical energy density from §3.
* **Alignment score** $\text{Align}(W)=\bar A/\max A$
* **Phase order parameter** $R=\big|\tfrac{1}{N}\sum e^{i\theta_i}\big|$
* **Referential misalignment** (local vs. global phase frame)
* **Barrier function** $B$ (safe-set invariance)

---

## 8. Critical Manifold (collapse)

Collapse when multiple invariants breach thresholds:

$$
\sigma_\kappa^2 > \text{thr}_\kappa,\quad
\mathcal{H} < \text{thr}_{\mathcal{H}},\quad
\Phi_E > \text{thr}_\Phi
$$

Optionally include:

$$
\rho_\phi > \text{thr}_\rho,\qquad R < \text{thr}_R.
$$

---

## 9. Barrier Functions

Barrier functions enforce forward invariance:

$$
B(s)\le 0\ \ (\text{safe}),\qquad B(s)>0\ \ (\text{unsafe}),
$$

with condition

$$
B(F_\Theta(s,u))-B(s)\le 0.
$$

> *This guarantees forward invariance: trajectories starting in the safe set remain in it under the dynamics.*

---

## 10. Summary

* **Primitives:** amplitude, phase, memory; curvature = diagnostic face of memory.
* **PDEs:** deterministic, A–θ–χ coupled; rational saturation; Bohm smoothing; symmetric β-coupling; damping options.
* **Energy:** conserved and EL-consistent under undamped, variational couplings. Bohm/saturation terms are representative canonical forms (updates from SWT).
* **Bounds:** $0\le A\le \sqrt{1/\alpha}$.
* **SCG:** stable-frame projection into $(x,y,z)$; χ-curvature preferred when available. Stable frames prevent spurious drift in $R$.
* **Invariants:** PDE-derived = {energy, amplitude bound}; operational = {curvature variance, entropy, dislocation, flux, alignment, misalignment, barrier}. Defaults are diagnostic conventions.
* **Critical manifold:** multi-invariant breach.
* **Barrier functions:** guarantee forward safety.
