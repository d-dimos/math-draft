---
layout: default
title: Kalman Filter
permalink: /topics/kalman-filter/
---

# Kalman Filter

## Introduction

The Kalman filter is a recursive algorithm used for estimating the state of a dynamic system from noisy measurements. It's widely used in control systems, robotics, and signal processing.

## State Space Model

The Kalman filter assumes a linear dynamic system:

**Prediction Model:**
$$x_k = F_k x_{k-1} + B_k u_k + w_k$$

**Observation Model:**
$$z_k = H_k x_k + v_k$$

where:
- $x_k$ is the state vector at time $k$
- $F_k$ is the state transition model
- $B_k$ is the control input model
- $u_k$ is the control vector
- $w_k$ is the process noise (assumed Gaussian with covariance $Q_k$)
- $z_k$ is the observation vector
- $H_k$ is the observation model
- $v_k$ is the observation noise (assumed Gaussian with covariance $R_k$)

## Algorithm Steps

### Prediction Step
$$\hat{x}_{k|k-1} = F_k \hat{x}_{k-1|k-1} + B_k u_k$$
$$P_{k|k-1} = F_k P_{k-1|k-1} F_k^T + Q_k$$

### Update Step
$$K_k = P_{k|k-1} H_k^T (H_k P_{k|k-1} H_k^T + R_k)^{-1}$$
$$\hat{x}_{k|k} = \hat{x}_{k|k-1} + K_k (z_k - H_k \hat{x}_{k|k-1})$$
$$P_{k|k} = (I - K_k H_k) P_{k|k-1}$$ 