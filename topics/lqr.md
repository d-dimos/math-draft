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

The cost function $J$ is typically chosen (tuned) by selecting appropriate matrices $Q_k$ and $R_k$. For the LQR problem to be well-posed, $Q_k \succcurlyeq 0$ (positive semi-definite) and $R_k \succ 0$ (positive definite). These conditions ensure the cost function is bounded below and that the minimizer is unique. Additionally, without loss of generality we assume that $Q=Q^T$ and $R=R^T$. We explain this [here](./#symmetric-matrices).

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

$\textbf{Solving the Bellman Equation}$

Substituting the system dynamics and the running cost into the Bellman equation, we get:

$$
V_k(x_k) = \min_{u_k} \bigg\{x_k^TQ_kx_k + u_k^TR_ku_k + V_{k+1} \Big({Ax_k + Bu_k}\Big) \bigg\}
$$

Let us solve this for $k=K-1$:

$$
\begin{aligned}
V_{K-1}(x_{K-1}) &= \min_{u_{K-1}} \bigg\{x_{K-1}^TQ_{K-1}x_{K-1} + u_{K-1}^TR_{K-1}u_{K-1} + V_{K} \Big({Ax_{K-1} + Bu_{K-1}}\Big) \bigg\} \\
&= \min_{u_{K-1}} \bigg\{x_{K-1}^TQ_{K-1}x_{K-1} + u_{K-1}^TR_{K-1}u_{K-1} + (Ax_{K-1} + Bu_{K-1})^TQ_K(Ax_{K-1} + Bu_{K-1}) \bigg\}
\end{aligned}
$$


To compute $u_{k-1}$ we set the gradient of the quantity inside the minimization with respect to $u_{k-1}$ to zero:

$$
\begin{aligned}

\nabla_{u_{K-1}} \bigg\{x_{K-1}^TQ_{K-1}x_{K-1} + u_{K-1}^TR_{K-1}u_{K-1} + (Ax_{K-1} + Bu_{K-1})^TQ_K(Ax_{K-1} + Bu_{K-1}) \bigg\} &= 0 \\

\nabla_{u_{K-1}} \bigg\{u_{K-1}^TR_{K-1}u_{K-1} + (Ax_{K-1} + Bu_{K-1})^TQ_K(Ax_{K-1} + Bu_{K-1}) \bigg\} &= 0 \\

(R_{K-1} + R_{K-1}^T)u_{K-1} + 
\nabla_{u_{K-1}} \bigg\{(Ax_{K-1} + Bu_{K-1})^TQ_K(Ax_{K-1} + Bu_{K-1}) \bigg\} &= 0 \\

2R_{K-1}u_{K-1} + 
\nabla_{u_{K-1}} \bigg\{(Ax_{K-1} + Bu_{K-1})^TQ_K(Ax_{K-1} + Bu_{K-1}) \bigg\} &= 0 \\

2R_{K-1}u_{K-1} + 
\nabla_{u_{K-1}} \bigg\{ x_{K-1}^T A^T Q_K A x_{K-1} + x_{K-1}^T A^T Q_K B u_{K-1} + u_{K-1}^T B^T Q_K A x_{K-1} + u_{K-1}^T B^T Q_K B u_{K-1} \bigg\} &= 0 \\

2R_{K-1}u_{K-1} + 
\nabla_{u_{K-1}} \bigg\{ x_{K-1}^T A^T Q_K B u_{K-1} + u_{K-1}^T B^T Q_K A x_{K-1} + u_{K-1}^T B^T Q_K B u_{K-1} \bigg\} &= 0 \\

\end{aligned}
$$

The quantities $x_{K-1}^T A^T Q_K B u_{K-1}$ and $u_{K-1}^T B^T Q_K A x_{K-1}$ are the transpose of each other and represent scalars, hence they're equal.

$$
\begin{aligned}

2R_{K-1}u_{K-1} + 
\nabla_{u_{K-1}} \bigg\{ 2x_{K-1}^T A^T Q_K B u_{K-1} + u_{K-1}^T B^T Q_K B u_{K-1} \bigg\} &= 0 \\

2R_{K-1}u_{K-1} + 
2 B^T Q_K^T A x_{K-1} +
2 B^T Q_K B u_{K-1} &= 0 \\

(R_{K-1} + B^TQ_KB)u_{K-1} + 
B^T Q_K^T A x_{K-1} &= 0 \\

\end{aligned}
$$

which leads to:

$$
u_{K-1} = - \underbrace{(R_{K-1} + B^TQ_KB)^{-1} B^T Q_K^T A}_{K_{K-1}} x_{K-1}
$$

where $K_{K-1}$ is called the $\textbf{gain matrix}$. The quantity $R_{K-1} + B^TQ_KB$ is clearly symmetric and also positive definite, therefore it is invertible. If both $Q_k$ and $R_k$ were allowed to be positive semi-definite, then $R_{K-1} + B^TQ_KB$ would be positive semi-definite and the gain matrix is not necessarily invertible.

Plugging in the solution for the $K-1$ time step into the Bellman equation, we get:

$$
\begin{aligned}
V_{K-1}(x_{K-1}) =&  x_{K-1}^TQ_{K-1}x_{K-1} \\

+ & \bigg((R_{K-1} + B^TQ_KB)^{-1} B^T Q_K^T A x_{K-1}\bigg)^TR_{K-1}\bigg( (R_{K-1} + B^TQ_KB)^{-1} B^T Q_K^T A x_{K-1} \bigg)  \\

+ & \bigg(Ax_{K-1} + B \Big((R_{K-1} + B^TQ_KB)^{-1} B^T Q_K^T A x_{K-1}\Big)\bigg)^TQ_K\bigg(Ax_{K-1} + B\Big((R_{K-1} + B^TQ_KB)^{-1} B^T Q_K^T A x_{K-1}\Big)\bigg)
\end{aligned}
$$





---
---
$\textbf{Why the LQR matrices are assumed symmetric WLOG}$ 

<a name="symmetric-matrices"></a>
Running cost and terminal cost are defined as:

$$q_k(x_k, u_k) = x_k^T Q_k x_k + u_k^T R_k u_k
\qquad \text{and} \qquad
q_K(x_K) = x_K^T Q_K x_K$$

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