---
layout: default
title: PID Control
permalink: /topics/pid-control/
---

# PID Control

## Introduction

Proportional-Integral-Derivative (PID) control is one of the most widely used control strategies in industrial applications. It combines three control actions to minimize the error between a desired setpoint and the actual process variable.

## Mathematical Formulation

The PID controller output is given by:

$$u(t) = K_p e(t) + K_i \int_0^t e(\tau) d\tau + K_d \frac{de(t)}{dt}$$

where:
- $u(t)$ is the control output
- $e(t)$ is the error signal (setpoint - process variable)
- $K_p$ is the proportional gain
- $K_i$ is the integral gain  
- $K_d$ is the derivative gain

## Transfer Function

In the Laplace domain, the PID controller transfer function is:

$$C(s) = K_p + \frac{K_i}{s} + K_d s$$

## Control Actions

### Proportional Action
- Provides immediate response proportional to the current error
- Higher $K_p$ → faster response, but may cause overshoot

### Integral Action  
- Eliminates steady-state error by accumulating past errors
- Higher $K_i$ → faster elimination of steady-state error, but may cause oscillations

### Derivative Action
- Predicts future error based on rate of change
- Higher $K_d$ → improved stability and reduced overshoot, but sensitive to noise

## Tuning Methods

Common PID tuning methods include:
- Ziegler-Nichols method
- Cohen-Coon method
- Lambda tuning
- Manual tuning 