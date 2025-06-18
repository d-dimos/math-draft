---
layout: default
title: Linear Quadratic Regulator (LQR)
permalink: /topics/lqr/
exclude: true
---

# Linear Quadratic Regulator (LQR)

LQR is an optimal control algorithm. It uses Dynamic Programming (DP) to compute optimal feedback controls for $\textbf{discrete}$ systems that are $\textbf{linear}$ and cost functions that are $\textbf{quadratic}$. LQR assumes a $\textbf{finite}$ time horizon $K$.

## Problem Formulation

$\textbf{System Dynamics}$

$$x_{k+1} = Ax_k + Bu_k$$

$$x_k \in \mathcal{X}, u_k \in \mathcal{U}$$

The states and controls are real vectors and $A$ and $B$ are appropriately sized real matrices.

$\textbf{Cost function}$

$$J(x_1^K, u_0^{K-1} ; x_0) = q_K(x_K) + \sum_{k=0}^{K-1}q(x_k, u_k)$$

where $q(x_k, u_k) = x_k^TQx_k + u_k^TRu_k$ and $q_K(x_K) = x_K^TQ_Kx_K$. The term $q_K(x_K)$ is called the $\textit{terminal cost}$ and $q(x_k, u_k)$ is called the $\textit{running cost}$ at time step $k$.

The cost function $J$ is typically chosen (tuned) by selecting appropriate matrices $Q$ and $R$. For the LQR problem to be well-posed, $Q \succcurlyeq 0$ (positive semi-definite) and $R \succ 0$ (positive definite). These conditions ensure the cost function is bounded below and that the optimal control is unique.

$\textbf{Optimization Problem}$

Our aim is to compute a trajectory $\{x_k,  u_k\}$, which starts from a given state $x_0$ and minimizes $J$ given the system dynamics:

$$
\begin{equation}
\begin{aligned}
\underset{u_0, u_1, \ldots, u_{K-1}}{\text{minimize}} \quad & J(x_1^K, u_0^{K-1} ; x_0) = q_K(x_K) + \sum_{k=0}^{K-1}q(x_k, u_k) \\
\text{subject to} \quad & x_{k+1} = Ax_k + Bu_k, \quad k = 0, 1, \ldots, K-1 \\
& x_0 \text{ given}
\end{aligned}
\end{equation}
$$

The solution $\{u_k\}_{k=1}^{K-1}$ to the optimization problem is called $\textit{policy}$ and is symbolized as $\pi$. 

## DP principles applied to LQR

To help us solve this problem, we employ the principles of Dynamic Programming. We recall the definition of the $\textbf{Value Function}$, $V^\pi : X \to \mathbb{R}$, which is the cost that is yielded if we started from a state and followed a policy $\pi$ afterwards. Since the Value function represents the cost remaining until the end of the trajectory, it is also called $\textit{cost-to-go}$. The relation between $V(x)$ and the cost function is:

$$
V(x_k) = J(x_{k+1}, \dots, x_K, u_k, \dots, u_K ; x_k)
$$

(of course $J$ in this context has a different domain depending on the length of the trajectory, but we abuse the same symbol for notational simplicity)

Equivalently:

$$
V(x_k) = q_K(x_K) + \sum_{i=k}^{K-1}q(x_i, u_i)
$$

We can rewrite the above expression using recursion:

$$
V(x_k) = q(x_k, u_k) + V(x_{k+1})
$$

By definition: $J(x_1^K, u_0^{K-1} ; x_0) = V(x_0)$.



