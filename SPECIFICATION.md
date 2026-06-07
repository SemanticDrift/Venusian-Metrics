# System Specification: Parametric Architecture and Kinematic Group Composition

This document establishes the formal mathematical and computational specification for the Venusian Resonance System Engine. It defines the precise boundaries between axiomatic inputs, derived scaling operators, discrete state encodings, and the group transformations executed by the rendering loop.

---

## 1. Architectural Parameter Classification

The system parameter layer is a hybrid structure comprising elements of the rational field $\mathbb{Q}$ and the discrete integer sign space $\mathbb{Z}$.

### A. Axiomatic Parameter Space (Elements of $\mathbb{Q}$)
These values function as primitive constants within the codebase, establishing a 4-dimensional input parameter space ($\vec{\psi} \in \mathbb{Q}^4$). They are defined as exact rational fractions to eliminate floating-point truncation errors during initialization.

* **System Baseline Period ($\psi_1$):** $1.0 \in \mathbb{Q}$  
  * *Role:* Temporal master clock baseline (Earth sidereal year proxy).
* **Proportional Frequency Ratio ($\mu$):** $\frac{13}{8} = 1.625 \in \mathbb{Q}$  
  * *Role:* Fixed harmonic scaling constant across the inner chamber.
* **Hexagonal Operator Limit ($\lambda$):** $\frac{1}{6} \in \mathbb{Q}$  
  * *Role:* Dimensionless geometric boundary contraction limit.
* **Admissible Node Index ($n$):** $48 \in \mathbb{Q}$  
  * *Role:* Discrete resonant coordinate constraint.

### B. Discrete Structural State Encodings (Elements of $\mathbb{Z}$)
These parameters do not represent continuous physical magnitudes or angular velocities. They function as signed categorical variables to dictate directional orientation within a 2D Euclidean plane.

* **Mercury Axial Direction State ($\sigma_M$):** $+1 \in \mathbb{Z}$ (Counterclockwise)
* **Earth Axial Direction State ($\sigma_E$):** $+1 \in \mathbb{Z}$ (Counterclockwise)
* **Venus Axial Direction State ($\sigma_V$):** $-1 \in \mathbb{Z}$ (Clockwise)

---

## 2. Derived Parametric Scaling Chain

The execution engine evaluates a deterministic, linear cascading chain from the axiomatic inputs. Every derived value is a strict, closed-form function of the input vector $\vec{\psi}$ and the time parameter $t \in \mathbb{R}$.

### A. Synodic and Sector Segmentation
The synodic period ($S$) and phase sectors ($\Phi$) are defined by continuous rational constraints:

$$S = \frac{1}{\left(\frac{1}{\psi_1 \cdot \mu^{-1}}\right) - \frac{1}{\psi_1}} = \frac{1}{\frac{13}{8} - 1} = \frac{8}{5} = 1.6 \text{ units}$$

$$\Phi = \left(\frac{8}{S}\right) \cdot 2 = 5 \cdot 2 = 10 \text{ segments}$$

### B. Temporal and Rotational Projections
Translating the dimensionless system variables into local temporal approximations uses a fixed baseline scaler ($D_{\text{base}} = 8 \cdot 365.25625 = 2922.05$). The axial rotation period ($P_{V\text{-spin}}$) emerges via scalar contraction matching the hexagonal boundary limit:

$$T_{\text{segment}} = \frac{D_{\text{base}}}{\Phi} = 292.205 \text{ days}$$

$$P_{V\text{-spin}} = T_{\text{segment}} \cdot (1 - \lambda) = 292.205 \cdot \left(1 - \frac{1}{6}\right) = 243.504 \text{ days}$$

---

## 3. Algebraic Closure of the Velocity Shear System

The kinematic boundary conditions assume that the middle node functions as an absolute coupling interface between the inner and outer boundaries. The net velocity shear ($V_{\Delta}$) must equal zero to maintain structural equilibrium across the discrete nodes:

$$V_{\Delta(\text{Inner})} = \sigma_M - \sigma_V = 1 - (-1) = 2$$

$$V_{\Delta(\text{Outer})} = \sigma_V - \sigma_E = -1 - 1 = -2$$

$$\sum V_{\Delta} = V_{\Delta(\text{Inner})} + V_{\Delta(\text{Outer})} = 2 + (-2) = 0$$

The system satisfies antisymmetric algebraic closure by construction within its defined sign operator space.

---

## 4. Kinematic Transformation Group Specification

The visual motion of the engine is governed by explicit affine coordinate transformations mapping to the Special Euclidean Group $SE(2)$. The mapping functions as a deterministic linear-time parametric embedding: $f(t; \vec{\psi}, \vec{\sigma}) \in SE(2)$, translating time directly into a geometric configuration.

### A. Transformation Group Composition
The total spatial position matrix ($T_{\text{total}}$) for any given planetary body is a non-commutative composition of the global orbital translation frame and the local axial rotation frame. Vector alignment is preserved under rigid $SE(2)$ composition at each evaluation step:

$$T_{\text{total}} = T_{\text{orbit}}(\theta_{\text{orbit}}, x_c, y_c) \circ T_{\text{spin}}(\theta_{\text{spin}}, x_l, y_l)$$

### B. Pure Affine Matrix Execution Loops
The coordinates are processed in 2D space ($x, y$) relative to the primary solar origin $(250, 250)$ using two decoupled rotational frequencies. Because the engine evaluates absolute angles directly as a closed-form function of the frame counter ($t$) rather than integrating velocity over time, numerical integration drift is absent from the execution layer:

$$\theta_{\text{orbit}}(t) = \left(\frac{360^{\circ}}{P_{\text{orbit}}}\right) \cdot k \cdot t$$

$$\theta_{\text{spin}}(t) = \sigma \cdot \left(\frac{360^{\circ}}{P_{\text{spin}}}\right) \cdot k \cdot t$$

Where $k$ is a visual scaling variable (`stepScale = 0.15`) used to scale the raw refresh rate to an observable speed without altering the underlying geometric ratios.

#### 1. Global Track Rotation ($T_{\text{orbit}}$)
$$\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} \cos\theta_{\text{orbit}} & -\sin\theta_{\text{orbit}} & 250(1-\cos\theta_{\text{orbit}}) + 250\sin\theta_{\text{orbit}} \\ \sin\theta_{\text{orbit}} & \cos\theta_{\text{orbit}} & 250(1-\cos\theta_{\text{orbit}}) - 250\sin\theta_{\text{orbit}} \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}$$

#### 2. Local Node Spin ($T_{\text{spin}}$)
Executed at the fixed radius offset $(x_l, y_l)$ relative to the local origin $(0,0)$ established by the parent transform node:
$$\begin{bmatrix} x'' \\ y'' \\ 1 \end{bmatrix} = \begin{bmatrix} \cos\theta_{\text{spin}} & -\sin\theta_{\text{spin}} & 0 \\ \sin\theta_{\text{spin}} & \cos\theta_{\text{spin}} & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix}$$

---

## 5. Definitive Execution Status

* **Classification:** Deterministic parametric kinematic simulation.
* **System Coordinate Volatility:** Zero internal dynamical degrees of freedom (all evolutionary states are strictly determined by the static 4-dimensional parameter baseline vector $\vec{\psi}$; the system features no dynamic state variables or autonomous evolution equations).
* **Numerical Drift Profile:** No numerical integration drift occurs, due to the closed-form evaluation of $\theta(t)$ per execution frame.
* **Physical Domain Constraints:** This model does not implement gravitational potential solvers, variational mass actions, or Newtonian differential force equations. It evaluates the geometric execution of predefined phase-locked constraints.
