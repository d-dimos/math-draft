---
layout: default
title: Cross-Entropy Method
permalink: /topics/ce/
exclude: true
---

# Cross-Entropy Method

## Definitions

$\textbf{Definition 1: [Kullback-Leibler Divergence/Relative Entropy]}$ Let $p(x)$ and $q(x)$ be two probability density functions (pdfs) defined on the same continuous space $\mathcal{X}$. The Kullback-Leibler (KL) divergence of $p$ from $q$ is defined as

$$
D_{KL}(p||q) = \int_{\mathcal{X}} p(x) \log \frac{p(x)}{q(x)} dx = \mathbb{E}_{p(x)}\left[\log \frac{p(x)}{q(x)}\right]
$$

$\textbf{Definition 2: [Cross-Entropy]}$ Let $p(x)$ and $q(x)$ be two pdfs defined on the same continuous space $\mathcal{X}$. The cross-entropy of $q$ relative to $p$ is defined as

$$
H(p, q) = -\int_{\mathcal{X}} p(x) \log q(x) dx = \mathbb{E}_{p(x)}\left[-\log q(x)\right]
$$

## Importance Sampling

Say we care about estimating the expected value of a known function $H(x)$ assuming the samples $x$ are drawn from a distribution $p(x)$:

$$
\mathbb{E}_{x \sim p(x)} \left[ H(x) \right] = \int H(x) p(x) dx
$$

Using Monte Carlo integration we approximate the expected value as:

$$
\mathbb{E}_{x \sim p(x)} \left[ H(x) \right] \approx \dfrac{1}{N} \sum_{i=1}^N H(x_i)
$$

where $x \sim p(x)$. However, practically computing this summation may be prohibitive. For example, $p(x)$ may be difficult to sample from, or the estimator $\mathbb{E}_{x \sim p(x)}$ may have very high variance, therefore necessitating a prohibitively large number of samples for convergence. 

Observe the following:

$$
\mathbb{E}_{x \sim p(x)} \left[ H(x) \right] =
\int H(x) p(x) dx = \int H(x) \dfrac{p(x)}{q(x)}q(x) dx = \mathbb{E}_{x\sim q(x)}\left [ H(x) \dfrac{p(x)}{q(x)} \right]
$$

This rewritting allows us to estimate the desired expected value by drawing samples from an arbitrary distribution $q(x)$ namely $\textit{proposal distribution}$. We want to choose $q(x)$ to reduce variance of the estimator and sample efficiently. 

If we find a good choice for the proposal distribution then we estimate the expected value as:

$$
\mathbb{E}_{x \sim p(x)} \left[ H(x) \right] \approx
\dfrac{1}{N} \sum_{i=1}^N H(x_i) \dfrac{p(x_i)}{q(x_i)}
$$ 

where $x_i \sim q(x_i)$. The better the choice of $q(x)$ the lower the variance of the estimator should be. In theory the optimal proposal distribution $q^*(x)$ reduces the number of necessary samples for convergence of the summation to one.

