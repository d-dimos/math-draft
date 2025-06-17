---
layout: default
title: Differential Dynamic Programming
permalink: /topics/ddp/
---

## Quadratic expansion

The cost-to-go function can be expressed as:

$$Q(x,u) = l(x,u) + V\!\bigl(f(x,u)\bigr)$$

where:
- $Q(x,u)$ is the Q-function
- $l(x,u)$ is the immediate cost
- $V\bigl(f(x,u)\bigr)$ is the value function of the next state

Alternative inline notation: $$Q(x,u) = l(x,u) + V(f(x,u))$$

[← Back to Topics]({{ site.baseurl }}/topics/)

[← Back to Home]({{ site.baseurl }}/)
