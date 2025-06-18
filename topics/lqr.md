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

Our aim is to compute a trajectory $\\\{x_k,  u_k\\\}$, which starts from a given state $x_0$ and minimizes $J$ given the system dynamics:

$$
\begin{equation}
\begin{aligned}
\underset{u_0, u_1, \ldots, u_{K-1}}{\text{minimize}} \quad & J(x_1^K, u_0^{K-1} ; x_0) = q_K(x_K) + \sum_{k=0}^{K-1}q_k(x_k, u_k) \\
\text{subject to} \quad & x_{k+1} = Ax_k + Bu_k, \quad k = 0, 1, \ldots, K-1 \\
& x_0 \text{ given}
\end{aligned}
\end{equation}
$$

A $\textit{policy}$ (or control law) $\pi$ is a mapping $\pi : \mathcal{X} \to \mathcal{U}$ such that $u_k = \pi(x_k)$. The policy that minimizes $J$ is the $\textit{optimal policy}$ and is denoted $\pi^{\ast}$. Its associated control sequence is $\\\{u_k^*\\\}_{k=1}^{K-1}$. 

$\textbf{Posing LQR as a DP problem}$

To approach the problem, we define the time-indexed $\textbf{value function}$
$V_k^{\pi} : \mathcal{X} \to \mathbb{R}$ as the remaining cost when the system is in state $x_k$ at time $k$ and the policy $\pi$ is followed thereafter:

$$
V_k^{\pi}(x_k) = J(x_{k+1}^K, u_k^{K-1}; x_k)
$$

Equivalently:

$$
V_k^{\pi}(x_k) = q_K(x_K) + \sum_{i=k}^{K-1} q_i(x_i, u_i)
$$

We can rewrite the above expression using recursion:

$$
V_k^{\pi}(x_k) = q_k(x_k, u_k) + V_{k+1}^{\pi}(x_{k+1})
$$

By definition, $J(x_1^K, u_0^{K-1} ; x_0) = V_0^{\pi}(x_0)$.

We call optimal value function $V^*_k(x)$ the function yielded by following the optimal policy $\pi^*$ from timestep $k$ and forward. The optimal value function is also called the $\textit{cost-to-go}$, and we denote it from now on as simply $V_k(x)$.

The Principle of Optimality is an inherent structural property of the LQR problem, i.e. any tail of an optimal policy must still be optimal. More explicitly, if $\pi_1 = [u_1, u_2, \dots, u_{K}]$ is an optimal policy over $K$ steps, then $\pi_2 = [u_2, \dots, u_{K}]$ is also optimal over the remaining $K-1$ steps. Therefore, the optimal policy when in state $x_0$ is the one that yields the minimum sum of running cost and cost-to-go from the next state.

Mathematically:

$$
\begin{equation}
\begin{aligned}
V^*_k(x_k) = \min_{u_k} \\\{q_k(x_k, u_k) + V^*_{k+1} \Big({f(x_k, u_k)}\Big) \}
\end{aligned}
\end{equation}
$$


Equation (2) is the Bellman equation (or the discrete time Hamilton-Jacobi-Bellman equation). 
