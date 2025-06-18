---
layout: default
title: Linear Quadratic Regulator (LQR)
permalink: /topics/lqr/
exclude: true
---

# Linear Quadratic Regulator (LQR)

$\textbf{Introduction}$

LQR is an optimal control algorithm. It uses Dynamic Programming (DP) to compute optimal feedback controls for $\textbf{discrete}$ systems that are $\textbf{linear}$ and cost functions that are $\textbf{quadratic}$. LQR assumes a $\textbf{finite}$ time horizon $K$.

$\textbf{System Dynamics}$

$$x_{k+1} = Ax_k + Bu_k$$

$$x_k \in \mathcal{X}, u_k \in \mathcal{U}$$

The states and controls are real vectors and $A$ and $B$ are appropriately sized real matrices.

$\textbf{Cost function}$

$$J(x_1^K, u_0^{K-1} ; x_0) = q_K(x_K) + \sum_{k=0}^{K-1}q_k(x_k, u_k)$$

where 

$$\underbrace{q_k(x_k, u_k) = x_k^TQ_kx_k + u_k^TR_ku_k}_{\text{running cost}}$$

and 

$$\underbrace{q_K(x_K) = x_K^TQ_Kx_K}_{\text{terminal cost}}$$

We note that LQR allows us to define time-variant cost matrices.

The cost function $J$ is typically chosen (tuned) by selecting appropriate matrices $Q_k$ and $R_k$. For the LQR problem to be well-posed, $Q_k \succcurlyeq 0$ (positive semi-definite) and $R_k \succ 0$ (positive definite). These conditions ensure the cost function is bounded below and that the optimal control is unique.

$\textbf{Optimization Problem}$

Our aim is to compute a trajectory $\{x_k,  u_k\}$, which starts from a given state $x_0$ and minimizes $J$ given the system dynamics:

$$
\begin{equation}
\begin{aligned}
\underset{u_0, u_1, \ldots, u_{K-1}}{\text{minimize}} \quad & J(x_1^K, u_0^{K-1} ; x_0) = q_K(x_K) + \sum_{k=0}^{K-1}q_k(x_k, u_k) \\
\text{subject to} \quad & x_{k+1} = Ax_k + Bu_k, \quad k = 0, 1, \ldots, K-1 \\
& x_0 \text{ given}
\end{aligned}
\end{equation}
$$

A $\textit{policy}$ (or control law) $\pi$ is a sequence of mappings $\pi_k : \mathcal{X} \to \mathcal{U}$ such that $u_k = \pi_k(x_k)$. The policy that minimizes $J$ is the $\textit{optimal policy}$ and is denoted $\pi^{\ast}$. Its associated control sequence is $\{u_k^*\}_{k=0}^{K-1}$. 

$\textbf{DP principles applied to LQR}$

To solve the finite-horizon problem we employ Dynamic Programming. For a given policy $\pi$ we define the time-indexed $\textbf{value function}$
$V_k^{\pi} : \mathcal{X} \to \mathbb{R}$ as the remaining cost when the system is in state $x_k$ at time $k$ and the policy $\pi$ is followed thereafter (this quantity is also called the $\textit{cost-to-go}$):

$$
V_k^{\pi}(x_k) = J(x_{k+1}^K, u_k^{K-1}; x_k).
$$

Equivalently:

$$
V_k^{\pi}(x_k) = q_K(x_K) + \sum_{i=k}^{K-1} q_i(x_i, u_i).
$$

We can rewrite the above expression using recursion:

$$
V_k^{\pi}(x_k) = q_k(x_k, u_k) + V_{k+1}^{\pi}(x_{k+1}).
$$

By definition, $J(x_1^K, u_0^{K-1} ; x_0) = V_0^{\pi}(x_0)$.



