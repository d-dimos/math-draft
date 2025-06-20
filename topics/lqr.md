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

$$x_{k+1} = f(x_k, u_k) = Ax_k + Bu_k$$

$$x_k \in \mathcal{X} = \mathbb{R}^n, u_k \in \mathcal{U} = \mathbb{R}^m$$

The states and controls are real vectors and $A$ and $B$ are appropriately sized real matrices.

$\textbf{Cost function}$

$$J(x_{1:K}, u_{0:K-1} ; x_0) = q_K(x_K) + \sum_{k=0}^{K-1}q_k(x_k, u_k)$$

where 

$$\underbrace{q_k(x_k, u_k) = x_k^TQ_kx_k + u_k^TR_ku_k}_{\text{running cost}}$$

and 

$$\underbrace{q_K(x_K) = x_K^TQ_Kx_K}_{\text{terminal cost}}$$

We note that in LQR we may also define time-varying cost matrices.

The cost function $J$ is typically chosen (tuned) by selecting appropriate matrices $Q_k$ and $R_k$. For the LQR problem to be well-posed, $Q_k \succcurlyeq 0$ (positive semi-definite) and $R_k \succ 0$ (positive definite). These conditions ensure the cost function is bounded below and that the minimizer is unique. Additionally, without loss of generality we assume that $Q=Q^T$ and $R=R^T$. We explain this [here](../cheatsheet.md/#nonlinear-system).

$\textbf{Optimization Problem}$

Our aim is to compute a trajectory $\\\{x_k,  u_k\\\}$, which starts from a given state $x_0$ and minimizes $J$ given the system dynamics:

$$
\begin{equation}
\begin{aligned}
\underset{u_0, u_1, \ldots, u_{K-1}}{\text{minimize}} \quad & J(x_{1:K}, u_{0:K-1} ; x_0) = q_K(x_K) + \sum_{k=0}^{K-1}q_k(x_k, u_k) \\
\text{subject to} \quad & x_{k+1} = Ax_k + Bu_k, \quad k = 0, 1, \ldots, K-1 \\
& x_0 \text{ given}
\end{aligned}
\end{equation}
$$

A $\textit{policy}$ (or control law) $\pi$ is a mapping $\pi : \mathcal{X} \to \mathcal{U}$ such that $u_k = \pi(x_k)$. The policy that minimizes $J$ is the $\textit{optimal policy}$ and is denoted $\pi^{\ast}$. Its associated control sequence is $\\\{u_k^*\\\}_{k=0}^{K-1}$. 

$\textbf{Dynamic-Programming Formulation}$

To approach the problem, we define the time-indexed optimal $\textbf{value function}$
$V_k : \mathcal{X} \to \mathbb{R}$ as the minimum cost-to-go from state $x$ at time $k$:

$$
V_k(x) = \min_{u_k, \dots, u_{K-1}} \bigg\{q_K(x_K) + \sum_{i=k}^{K-1}q_i(x_i, u_i) \bigg\}
$$

The value function at the last time step $K$ is then: $V_K(x_K) = x_K^TQ_Kx_K$.

The problem has an optimal substructure, which allows us to apply the Principle of Optimality. This principle states that an optimal policy has the property that whatever the initial state and initial decision are, the remaining decisions must constitute an optimal policy with regard to the state resulting from the first decision.

This leads to the recursive equation known as the $\textbf{Bellman equation}$:

$$
\begin{equation}
\begin{aligned}
V_k(x_k) = \min_{u_k} \bigg\{q_k(x_k, u_k) + V_{k+1} \Big({f(x_k, u_k)}\Big) \bigg\}
\end{aligned}
\end{equation}
$$


also known as the $\textbf{discrete-time Hamilton-Jacobi-Bellman equation}$. 
