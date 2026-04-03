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

## Project Overview
Underwater Acoustic (UWA) communication systems rely on **sound waves** to transmit data through water. However, these channels are extremely challenging due to:

- Severe multipath propagation  
- Large delay spreads  
- Impulsive + Gaussian noise  

Traditional channel estimation techniques fail under such harsh conditions.

This project proposes a **Robust ADMM (Alternating Direction Method of Multipliers)** based optimization framework to efficiently estimate **sparse underwater channels**, ensuring reliability and robustness.

---

## Problem Statement

The received signal is modeled as:

\[
y = Ax + n
\]

Where:
- \( y \in \mathbb{R}^{M \times 1} \): Received signal vector  
- \( A \in \mathbb{R}^{M \times N} \): Known pilot/training matrix  
- \( x \in \mathbb{R}^{N \times 1} \): Sparse channel coefficients  
- \( n \): Noise (impulsive + Gaussian)  

### Key Challenge
- The system is **underdetermined (M < N)**
- However, the channel \( x \) is **sparse**, enabling compressed sensing approaches

---

## Optimization Formulation

We formulate the estimation as:

\[
\min_x \|y - Ax\|_1 + \tau \|x\|_1
\]

### Interpretation
- \( \|y - Ax\|_1 \): Robust to impulsive noise  
- \( \|x\|_1 \): Promotes sparsity  
- \( \tau \): Regularization parameter  

---

## ADMM Reformulation

Introduce auxiliary variable:

\[
z = y - Ax
\]

### Variables:
- \( x \): Channel estimate  
- \( z \): Residual  
- \( \gamma \): Lagrange multiplier  
- \( \rho \): Penalty parameter  

---

## ADMM Iterative Updates

The optimization is solved iteratively:

\[
x^{(k+1)} = \arg\min_x f_1(x) + f_2(x)
\]

\[
z^{(k+1)} = \arg\min_z g_1(z) + g_2(z)
\]

\[
\gamma^{(k+1)} = \gamma^{(k)} + \rho(z^{(k+1)} + Ax^{(k+1)} - y)
\]

---

## Handling Non-Solvability

Direct solutions are not feasible, so we use:

- **Proximal operators**
- **Linearization of functions**

---

## Proximal Operator

\[
\text{prox}_{f,t}(x) = \arg\min_{x^+} \left( \frac{1}{2t} \|x^+ - x\|_2^2 + f(x^+) \right)
\]

---

## Soft Thresholding (L1 Regularization)

\[
S_\alpha(\beta) =
\begin{cases}
\beta - \alpha, & \beta > \alpha \\
0, & |\beta| \le \alpha \\
\beta + \alpha, & \beta < -\alpha
\end{cases}
\]

---

## Final Update Equations

\[
x^{(k+1)} = S_{t_x/\rho}\left(x^{(k)} - \frac{t_x}{\rho} \nabla f_1(x^{(k)})\right)
\]

\[
z^{(k+1)} = S_{t_z\tau/\rho}\left(z^{(k)} - \frac{t_z}{\rho} \nabla g_1(z^{(k)})\right)
\]

---

## Convergence Criteria

The algorithm stops when:

### Primal Residual:
\[
r_p = Ax^{(k+1)} + z^{(k+1)} - y
\]

### Dual Residual:
\[
r_d = \rho A^H (r_p^{(k+1)} - r_p^{(k)}) - \frac{1}{t_x}(x^{(k+1)} - x^{(k)})
\]

### Stopping Condition:
- Both residuals fall below predefined tolerance(1e-4)

---

## Results & Performance

### Simulation Conditions:
- SINR = -∞ (extreme noise)
- SINR = 40
- SINR = 50  

### Observations:
- ADMM outperforms:
  - OMP (Orthogonal Matching Pursuit)
  - FISTA (Fast Iterative Shrinkage Thresholding Algorithm)

### Metrics:
- NMSE (Normalized Mean Square Error)
- Signal norm comparison
- Channel reconstruction accuracy  


---