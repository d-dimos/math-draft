---
layout: default
title: LQR
permalink: /topics/lqr/
---

# Linear Quadratic Regulator (LQR)

## Introduction

Linear Quadratic Regulator (LQR) is an optimal control technique used in control theory to find the optimal control law for a linear system with a quadratic cost function.

## Mathematical Formulation

Consider a linear time-invariant system:

$$\dot{x}(t) = Ax(t) + Bu(t)$$

where:
- $x(t)$ is the state vector
- $u(t)$ is the control input
- $A$ and $B$ are system matrices

## Cost Function

The LQR controller minimizes the quadratic cost function:

$$J = \int_0^\infty [x^T(t)Qx(t) + u^T(t)Ru(t)] dt$$

where $Q$ and $R$ are positive semi-definite and positive definite matrices, respectively.

## Solution

The optimal control law is given by:

$$u^*(t) = -Kx(t)$$

where $K = R^{-1}B^TP$ and $P$ is the solution to the algebraic Riccati equation:

$$A^TP + PA - PBR^{-1}B^TP + Q = 0$$ 