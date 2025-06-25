---
layout: default
title: Cross-Entropy Method
permalink: /topics/ce/
exclude: true
---

# Cross-Entropy (CE) Method

## Definitions

$\textbf{Definition 1: [Kullback-Leibler Divergence/Relative Entropy]}$ Let $p(x)$ and $q(x)$ be two probability density functions (pdfs) defined on the same continuous space $\mathcal{X}$. The Kullback-Leibler (KL) divergence of $p$ from $q$ is defined as

$$
D_{KL}(p||q) = \mathbb{E}_{p(x)}\left[\log \frac{p(x)}{q(x)}\right] = \int_{\mathcal{X}} p(x) \log \frac{p(x)}{q(x)} dx
$$

$\textbf{Definition 2: [Cross-Entropy]}$ Let $p(x)$ and $q(x)$ be two pdfs defined on the same continuous space $\mathcal{X}$. The cross-entropy of $q$ relative to $p$ is defined as

$$
H(p, q) = \mathbb{E}_{p(x)}\left[-\log q(x)\right] = -\int_{\mathcal{X}} p(x) \log q(x) dx
$$

## Importance Sampling

Say we care about estimating the expected value of a function $F(x)$ assuming the samples $x$ are drawn from a distribution $p(x)$:

$$
\mathbb{E}_{x \sim p(x)} \left[ F(x) \right] = \int F(x) p(x) dx
$$

Using Monte Carlo integration we approximate the expected value as:

$$
\mathbb{E}_{x \sim p(x)} \left[ F(x) \right] \approx \dfrac{1}{N} \sum_{i=1}^N F(x_i)
$$

where $x \sim p(x)$. However, practically computing this summation may be prohibitive. For example, $p(x)$ may be difficult to sample from, or the estimator $\mathbb{E}_{x \sim p(x)}$ may have very high variance, therefore necessitating a prohibitively large number of samples for convergence. 

Observe the following:

$$
\mathbb{E}_{x \sim p(x)} \left[ F(x) \right] =
\int F(x) p(x) dx = \int F(x) \dfrac{p(x)}{q(x)}q(x) dx = \mathbb{E}_{x\sim q(x)}\left [ F(x) \dfrac{p(x)}{q(x)} \right]
$$

This rewriting allows us to estimate the desired expected value by drawing samples from an arbitrary distribution $q(x)$ namely $\textit{proposal distribution}$. We want to choose $q(x)$ to reduce variance of the estimator and sample efficiently. 

If we find a good choice for the proposal distribution then we estimate the expected value as:

$$
\mathbb{E}_{x \sim p(x)} \left[ F(x) \right] \approx
\dfrac{1}{N} \sum_{i=1}^N F(x_i) \dfrac{p(x_i)}{q(x_i)}
$$ 

where $x_i \sim q(x_i)$. The better the choice of $q(x)$ the lower the variance of the estimator.

## Generic Cross-Entropy Method

The optimal proposal distribution is: $q^*(x) = \dfrac{F(x)p(x)}{\mathbb{E}_{x \sim p(x)} \left[ F(x) \right]}$, which has the lowest possible variance (a single sample is enough). Of course, we cannot have access to $q^*(x)$ since it depends on $\mathbb{E}_{x \sim p(x)} \left[ F(x) \right]$ which is the unknown quantity that we are trying to estimate in the first place.

Therefore, we would like to approximate $q^*(x)$ in order to use it to estimate $\mathbb{E}_{x \sim p(x)} \left[ F(x) \right]$ with a low variance via importance sampling.

Cross-entropy method is an algorithm used to approximate $q^*(x)$. 

We define $g(x ; \theta)$ to be a pdf parameterized by $\theta$ and we desire $g(x ; \theta)$ to be as close to $q^*(x)$ as possible in the sense of KL divergence, a.k.a. we desire:

$$
\theta^* = \argmin_\theta D_{KL}(q^*(x) || g(x ; \theta))
$$

Using the definition of KL divergence:

$$
D_{KL}(q^*(x) || g(x ; \theta)) = 
\mathbb{E}_{q^*(x)}\left[\log \frac{q^*(x)}{g(x ; \theta)}\right] = 
\mathbb{E}_{q^*(x)}\left[ \log q^*(x) \right] - \mathbb{E}_{q^*(x)}\left[\log g(x ; \theta) \right]
$$

the first term is constant with respect to $\theta$, hence the objective becomes:

$$
\theta^* = \arg \min_\theta \mathbb{E}_{q^*(x)}\left[- \log g(x ; \theta) \right]
$$

Hence, the objective has turned into minimizing the Cross-Entropy $H(q^*, g) = \mathbb{E}_{q^*(x)}\left[- \log g(x ; \theta) \right]$.

We flip the objective's sign and turn this into a maximization problem:

$$
\theta^* = \arg \max_\theta \mathbb{E}_{q^*(x)}\left[\log g(x ; \theta) \right]
$$

We go on analyzing the objective function further:

$$
\begin{align*}
\mathbb{E}_{q^*(x)}\left[\log g(x ; \theta) \right] &= \int q^*(x)\log g(x ; \theta)dx \\
&= \int \dfrac{F(x)p(x)}{\mathbb{E}_{x \sim p(x)} \left[ F(x) \right]}\log g(x ; \theta)dx
\end{align*}
$$

the quantity $\mathbb{E}_{x \sim p(x)} \left[ F(x) \right]$ is constant wrt $\theta$, therefore we can safely ignore it in the maximization:

$$
\begin{align*}
\int F(x)p(x) \log g(x ; \theta)dx &= \int \left(F(x) \dfrac{p(x)}{g(x ; w)} \log g(x ; \theta)\right)g(x; w)dx \\
&= \mathbb{E}_{g(x;w)} \left[F(x) \dfrac{p(x)}{g(x ; w)} \log g(x ; \theta) \right] \\
&\approx \dfrac{1}{N} \sum_{i=1}^N F(x_i) \dfrac{p(x_i)}{g(x_i ; w)} \log g(x_i ; \theta)
\end{align*}
$$

We reached this point by using Importance Sampling (introducing $g(x;w)$) and then Monte Carlo integration. The final objective is:

$$
\theta^* = \arg \max_\theta \dfrac{1}{N} \sum_{i=1}^N F(x_i) \dfrac{p(x_i)}{g(x_i ; w)} \log g(x_i ; \theta)
$$


The cross entropy method is iterative and after each iteration it replaces $w$ with the new estimate $\theta^{(k)}$, aka:

$$
\theta^{(k+1)} = \arg \max_\theta \dfrac{1}{N} \sum_{i=1}^N F(x_i) \dfrac{p(x_i)}{g(x_i ; \theta^{(k)})} \log g(x_i ; \theta)
$$

where the samples $x$ are drawn from $g(x ; \theta^{(k)})$ and we keep iterating until convergence.