![amrita_logo](assets/Amrita.png)
Mathematics for Computing-3

---

## Team

- Nimmagadda Kesav Satya Sai  
- Nakka Saampotth Maddileti  
- Pabbathi Gnana Vikas Sai  
- Vemula Poorna Chandra  

---
# A total of 2 projects were done increasing work quality

---

# Project-1: Smooth & Spike Signal Separation using Overcomplete Dictionary and ADMM

## Overview

This project focuses on decomposing a signal into two components:

- **Smooth component**: low-frequency structure  
- **Spike component**: sparse, high-frequency noise  

We solve this using:

- Overcomplete Dictionary Representation  
- Sparse Optimization (L1 Regularization)  
- ADMM (Alternating Direction Method of Multipliers)  

---

## Main Idea of this project

Given a signal:

$$
x = v + w
$$

Where:
- \( v \): smooth signal  
- \( w \): sparse spike noise  

We represent:

$$
x = A z
$$

Where:

$$
A = [A_{DCT} \;\; I]
$$

$$
z =
\begin{bmatrix}
\alpha_{smooth} \\
\alpha_{spike}
\end{bmatrix}
$$

---

## Discrete Cosine Transform (DCT)

The DCT basis matrix is defined as:

$$
A_{k,n} = \alpha_k \cos \left( \frac{\pi}{N} (n + \frac{1}{2}) k \right)
$$

Where:

$$
\alpha_k =
\begin{cases}
\sqrt{\frac{1}{N}} & k = 0 \\
\sqrt{\frac{2}{N}} & k > 0
\end{cases}
$$

---

## Overcomplete Dictionary

We construct:

$$
A = [\text{DCT basis} \;\; I]
$$

- Here DCT captures smooth structure  
- Identity matrix part captures sparse spikes  

---

## Optimization Problem

We want a sparse solution:

$$
\min_z \|z\|_1 \quad \text{subject to} \quad Az = x
$$

Relaxed form:

$$
\min_z \|z\|_1 + \frac{\rho}{2} \|Az - x\|_2^2
$$

---

## ADMM Formulation

We split the problem:

$$
\min_{\alpha, z} \|\alpha\|_1 + I_{\{Az = x\}}(z)
$$

---

## Augmented Lagrangian

$$
\mathcal{L}(\alpha, z, \lambda) =
\|\alpha\|_1 + \lambda^T(\alpha - z) + \frac{\rho}{2}\|\alpha - z\|_2^2
$$

---

## Update Equations

### 1. Alpha Update (Shrinkage)

$$
\alpha^{k+1} = S_{\frac{1}{\rho}} \left(z^k - \frac{1}{\rho}\lambda^k \right)
$$

---

### 2. Z Update

$$
z^{k+1} = (I - A^\dagger A)(\alpha^{k+1} + \frac{1}{\rho}\lambda^k) + A^\dagger x
$$

---

### 3. Lambda Update

$$
\lambda^{k+1} = \lambda^k + \rho(\alpha^{k+1} - z^{k+1})
$$

---

## Shrinkage Function

$$
S_k(a) =
\begin{cases}
a - k & a > k \\
a + k & a < -k \\
0 & |a| \le k
\end{cases}
$$

---

---

## Experiments

| SNR (dB) | N   | k₁ (smooth) | k₂ (spike) |
|----------|-----|------------|------------|
| 20 dB    | 100 | 5          | 10         |
| 12 dB    | 120 | 3          | 5          |
| 10 dB    | 150 | 8          | 20         |
| 6 dB     | 60  | 4          | 6          |
| -12 dB   | 100 | 15         | 10         |
| -20 dB   | 80  | 8          | 8          |

---

## Results

- Accurate separation at **high SNR**
- Gradual degradation at **low SNR**
- Structure preserved even under heavy noise

---

## References

K. Harikumar, S. Athira, Y. C. Nair, V. Sowmya and K. P. Soman, "ADMM based algorithm for spike and smooth signal separation using over-complete dictionary," 2015 International Conference on Communications and Signal Processing (ICCSP), Melmaruvathur, India, 2015, pp. 1617-1622, doi: 10.1109/ICCSP.2015.7322792

## Future Work

- This project has been shelved and replaced with another project after review-1, due to simplicity of the work

---

# Project-2: Optimization of Sparse Underwater Acoustic Channels using Robust ADMM

---

## Project Overview
Underwater Acoustic (UWA) communication systems rely on **sound waves** to transmit data through water. However, these channels are extremely challenging due to:

- Severe multipath propagation  
- Large delay spreads  
- Impulsive + Gaussian noise  

Traditional channel estimation techniques fail under such harsh conditions.

This project proposes a **Robust ADMM (Alternating Direction Method of Multipliers)** based optimization framework to efficiently estimate **sparse underwater channels**, ensuring reliability and robustness.

---

## Problem Statement

The received signal model:

y = A x + n

Where:
- y ∈ R^(M×1): Received signal  
- A ∈ R^(M×N): Known pilot/training matrix  
- x ∈ R^(N×1): Sparse channel coefficients  
- n: Noise (impulsive + Gaussian)  

### Key Challenge
- The system is **underdetermined (M < N)**
- The channel vector **x is sparse**
- This enables **compressed sensing-based recovery**

---

## Optimization Formulation

We solve the following problem:

minimize   ||y - A x||_1 + τ ||x||_1

### Interpretation
- ||y - Ax||₁ → Robust against impulsive noise  
- ||x||₁ → Promotes sparsity  
- τ → Regularization parameter controlling trade-off  

---

# Mathematical derivation

---

## Original Optimization Problem

min_x  ||y - Ax||_1 + τ ||x||_1

### Problem Difficulty:
- L1 norm is non-smooth (non-differentiable)
- Cannot be solved using standard gradient methods
- Requires advanced optimization techniques

---

## Introduce Auxiliary Variable

Let:

z = y - Ax

Rewrite problem:

minimize   ||z||_1 + τ ||x||_1  
subject to: z = y - Ax

---

## Convert to ADMM Standard Form

Rewrite constraint:

Ax + z = y

Now:

minimize   f(x) + g(z)  
subject to Ax + z = y

Where:
- f(x) = τ ||x||₁  
- g(z) = ||z||₁  

---

## Augmented Lagrangian

L(x, z, γ) =  
||z||₁ + τ||x||₁  
+ γᵀ(Ax + z - y)  
+ (ρ/2) ||Ax + z - y||₂²  

Where:
- γ = Lagrange multiplier  
- ρ = penalty parameter  

---

## Scaled Form

Let:

u = γ / ρ

Then:

L(x, z) = ||z||₁ + τ||x||₁ + (ρ/2)||Ax + z - y + u||₂²

---

## ADMM Iterations

### (1) x-update

x^(k+1) = argmin_x [  
τ||x||₁ + (ρ/2)||Ax + z^k - y + u^k||₂²  
]

Gradient:

∇f1(x) = ρ Aᵀ(Ax + z - y + u)

Using proximal step:

x^(k+1) = prox_(τ/ρ)( x^k - (1/ρ) ∇f1(x^k) )

---

### (2) z-update

z^(k+1) = argmin_z [  
||z||₁ + (ρ/2)||Ax^(k+1) + z - y + u^k||₂²  
]

Gradient:

∇g1(z) = ρ (Ax + z - y + u)

Update:

z^(k+1) = prox_(1/ρ)( z^k - (1/ρ) ∇g1(z^k) )

---

### (3) Dual update

u^(k+1) = u^k + (Ax^(k+1) + z^(k+1) - y)

---

## Proximal Operator

prox_f(x) = argmin_u [ (1/2)||u - x||₂² + f(u) ]

---

## Soft Thresholding

S_α(x) = sign(x) * max(|x| - α, 0)

---

## Final Closed-Form Updates

x^(k+1) = S_(τ/ρ) [ x^k - (1/ρ) Aᵀ(Ax^k + z^k - y + u^k) ]

z^(k+1) = S_(1/ρ) [ z^k - (Ax^(k+1) - y + u^k) ]

u^(k+1) = u^k + (Ax^(k+1) + z^(k+1) - y)

---

## Convergence Criteria

Primal residual:

r_p = Ax + z - y

Dual residual:

r_d = ρ Aᵀ (r_p^(k+1) - r_p^(k))

Stopping condition:

||r_p||₂ < 1e-4 AND ||r_d||₂ < 1e-4

---

# Results & Performance

### Simulation Conditions:
- SINR = -∞ (extreme noise)
- SINR = 40
- SINR = 50  

---

###  Observations:
- ADMM outperforms:
  - OMP (Orthogonal Matching Pursuit)
  - FISTA (Fast Iterative Shrinkage Thresholding Algorithm)

---

### Metrics:
- NMSE (Normalized Mean Square Error)
- Signal norm comparison
- Channel reconstruction accuracy  

---

#  Conclusion

The Robust ADMM algorithm:

- Efficiently estimates sparse underwater channels  
- Maintains performance under extreme noise  
- Outperforms OMP and FISTA  
- Converges faster and reliably  

---

# Reference

Optimization of Sparse Underwater Acoustic Channels using Robust ADMM

---
---

# Analog Computing Implementation

## Overview
In addition to the digital optimization using ADMM, this project also explores **analog computation of differential equations** using electronic circuits. Analog computing provides a continuous-time physical realization of mathematical models using components like operational amplifiers, resistors, capacitors, and inductors. This enables real-time computation without discretization.

##  Governing Differential Equation
The system is modeled using a second-order differential equation:

d²v/dt² + dw/dt + v₀ = u₀

Where:
- d²v/dt² → Acceleration (second derivative of v)
- dw/dt → Rate of change of variable w
- v₀ → Constant bias / initial condition
- u₀ → External input (forcing function)

---

## Physical Interpretation
This equation represents a dynamic system:
- Second derivative → inertia / acceleration
- First derivative → damping / energy dissipation
- Constant term → equilibrium / bias
- Input → external forcing

This is analogous to:
- Mass-spring-damper systems
- RLC circuits
- Control systems

---

# Mathematical Derivation

## Rearranging Equation
We isolate the highest derivative:

d²v/dt² = u₀ - v₀ - dw/dt

## Convert to Integrator Form
Analog circuits prefer integration over differentiation:

dv/dt = ∫ (d²v/dt²) dt  
v = ∫ (dv/dt) dt  

Thus, we use two cascaded integrators to compute v.


## Block Representation
Break system into stages:

1. Compute RHS:
RHS = u₀ - v₀ - dw/dt  

2. First integrator:
dv/dt = ∫ RHS dt  

3. Second integrator:
v = ∫ (dv/dt) dt  

## Circuit Mapping

### Integrator (Op-Amp Based)
Vout = - (1/RC) ∫ Vin dt  

Used to compute:
- dv/dt
- v

### Differentiator Circuit
Vout = -RC (dVin/dt)  

Used to compute:
dw/dt  

### Summing Amplifier
To compute RHS:

u₀ - v₀ - dw/dt  

We use an op-amp summer:
Vout = -(V1 + V2 + V3)


## Full Analog Architecture
The complete system consists of:
- Summing amplifier (forms RHS)
- Differentiator (computes dw/dt)
- Two integrators (produce v)


##  Equivalent Physical System (RLC)
This system is equivalent to:

L d²i/dt² + R di/dt + (1/C)i = V(t)

Where:
- Inductor → second derivative
- Resistor → damping
- Capacitor → integration



## Hardware Implementation
The physical circuit includes:
- Operational amplifiers configured as integrators and summers
- Resistors and capacitors for scaling
- Breadboard wiring with signal input


## Output Measurement
Signals were captured using an oscilloscope in scopy software


## Waveform Analysis

### Channel 1 (Orange)
- Represents system output v
- Smooth sinusoidal waveform

### Channel 2 (Purple)
- Represents derivative or input signal
- Higher amplitude waveform


##  Observations

### Oscillatory Behavior
- Sinusoidal output confirms second-order dynamics

### Phase Shift
- Signals are phase shifted due to integration/differentiation

### Amplitude Difference
- Higher amplitude in derivative signal

### Frequency
- Around 1–2 kHz range

---

## Interpretation
- The circuit successfully solves the differential equation
- Output matches expected theoretical behavior
- Demonstrates real-time analog computation

---

##  Advantages
- Real-time computation
- Continuous signal processing
- No discretization error
- Efficient for differential equations

---

##  Limitations
- Sensitive to noise
- Component inaccuracies
- Hard to scale
- Less flexible than digital methods

---

##  Conclusion
The analog computing implementation:
- Successfully models a second-order system
- Produces correct oscillatory outputs
- Matches theoretical expectations
- Validates physical computation of differential equations

---

## Reference
Analog paradigm- https://analogparadigm.com/