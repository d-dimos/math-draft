---
layout: default
title: Cheatsheet
permalink: /topics/cheatsheet/
exclude: true
---

# Cheatsheet

$\textbf{Why the LQR matrices are assumed symmetric WLOG}$

<a name="nonlinear-system"></a>
Running cost and terminal cost are defined as:

$$q_k(x_k, u_k) = x_k^T Q_k x_k + u_k^T R_k u_k$$

$$q_K(x_K) = x_K^T Q_K x_K$$

where we assume $Q_k$ and $R_k$ are symmetric. This does not violate generality and the reason is the following.

Suppose $Q_k$ is not symmetric, then we can write:

$$Q_k = \underbrace{\frac{Q_k + Q_k^T}{2}}_{Q_{sym}} + \underbrace{\frac{Q_k - Q_k^T}{2}}_{Q_{skew}}$$

where $Q_{sym}$ is a symmetric matrix and $Q_{skew} = -Q_{skew}^T$ is a skew-symmetric matrix. We have that:

$$
Q_{skew} = -Q_{skew}^T \implies x_k^T Q_{skew} x_k = 0
$$

Therefore:

$$
x_k^T Q_k x_k = x_k^T Q_{sym} x_k + \underbrace{x_k^T Q_{skew} x_k}_{=0} = x_k^T Q_{sym} x_k
$$

the skew-symmetric part of $Q_k$ does not affect the cost function and we can safely assume $Q_k$ is symmetric.









