---
layout: default
title: Cross-Entropy Method
permalink: /topics/ce/
exclude: true
---

# Cross-Entropy Method (CEM)

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

The optimal proposal distribution is:
$$
q^*(x) = \dfrac{F(x)p(x)}{\mathbb{E}_{x \sim p(x)} \left[ F(x) \right]}
$$, which has the lowest possible variance (a single sample is enough). Of course, we cannot have access to $q^*(x)$ since it depends on $\mathbb{E}_{x \sim p(x)} \left[ F(x) \right]$ which is the unknown quantity that we are trying to estimate in the first place.

Therefore, we would like to approximate $q^*(x)$ in order to use it to estimate $\mathbb{E}_{x \sim p(x)} \left[ F(x) \right]$ with a low variance via importance sampling.

The cross-entropy method is an algorithm used to approximate $q^*(x)$. 

We define $g(x ; \theta)$ to be a pdf parameterized by $\theta$ and we desire $g(x ; \theta)$ to be as close to $q^*(x)$ as possible in the sense of KL divergence, a.k.a. we desire:

$$
\theta^* = \arg \min_\theta D_{KL}(q^*(x) || g(x ; \theta))
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

Hence, the objective has turned into minimizing the Cross-Entropy $$H(q^*, g) = \mathbb{E}_{q^*(x)}\left[- \log g(x ; \theta) \right]$$.

We flip the objective's sign and turn this into a maximization problem:

$$
\theta^* = \arg \max_\theta \mathbb{E}_{q^*(x)}\left[\log g(x ; \theta) \right]
$$

We now analyze the objective further:

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


## CEM in Stochatic Optimal Control

Consider a discrete-time stochastic system with dynamics $p(x_{k+1} | x_k, u_k)$. We symbolize a state trajectory as 
$$\tau = \{x_1, \dots, x_K\}$$
. Let $$\pi_\theta(u_k|x_k)$$ be a policy parameterized by $\theta$. In stochastic optimal control we define the following objective:

$$
J(\theta) = \mathbb{E}_{\tau \sim p_{\theta}(\tau)} \left[ \sum_{k=1}^K \gamma^k r(x_k, u_k) \bigg| x_0\right]
$$

where $\tau$ are trajectories sampled by unrolling the policy $\pi_\theta$, $\sum_{k=1}^K \gamma^k r(x_k, u_k)$ is the discounted average return. When the policy is optimal the objective is optimal and vice versa. We note that:

$$
p_\theta(\tau) = \prod_{k=0}^{K-1} p(x_{k+1} | x_k, u_k)\pi_{\theta}(u_k|x_k)
$$

The aim in optimal control is to optimize (either minimize or maximize depending on our definition of the rewards $r(x_k, u_k)$) the objective function $J(\theta)$. This is equivalent to finding parameters for our policy that yield better cumulative rewards on average.

The objective function $J(\theta)$ is generally non-convex. We do not have a way of getting to the global optimum directly. However, we can at least get to a local optimum by working as follows.

We model the control sequence (or policy parameters) as drawn from a parametric distribution $\theta \sim q_{\phi}(\theta)$. The problem of finding an optimal $\theta$ is cast to a problem of finding the best parameters of its modeling distribution.


Now we fix $\phi$ to a value $\phi'$ and draw some samples from it $\theta_1, \dots, \theta_N$. Consider the following distribution (assuming that we want to maximize $J$ wlog):

$$
p(\theta) \propto \mathbb{1}_{\{J(\theta_i) > \gamma\}}
$$

This is a uniform distribution among the samples that yield higher average rewards than $\gamma$. We ignore the normalizing constant, as it does not affect the optimization — a fact that will become clear shortly.

Notice that (assuming large enough $N$) for any choice of $q_{\phi'}(\theta)$ the handcrafted $p(\theta)$ is far more likely to generate better $\theta$, especially when our choice of $\phi'$ is bad. In other words, the handcrafted $p(\theta)$ is simply the uniform distribution over the best samples that we drew from $q_{\phi'}(\theta)$. This is where CEM comes into play.

We would like to minimize the KL divergence between $q_{\phi}$ and the better distribution $p$, which is equivalent to minimizing the Cross-Entropy $$H(p, q_{\phi}) = \mathbb{E}_{p}\left[-\log q_{\phi}(\theta)\right]$$.

$$
\begin{align*}
\phi^* &= \arg \min_{\phi}  \mathbb{E}_{p}\left[-\log q_{\phi}(\theta)\right] \\[10pt]
&= \arg \max_{\phi}  \mathbb{E}_{p}\left[\log q_{\phi}(\theta)\right] \\[10pt]
&= \arg \max_{\phi}  \mathbb{E}_{g(\theta)}\left[\dfrac{p(\theta)}{g(\theta)}\log q_{\phi}(\theta)\right] \\[10pt]
\end{align*}
$$

where at the last step we used importance sampling.

$$
\begin{align*}
\phi^* &\approx \arg \max_{\phi} \sum_{i=1}^N \dfrac{p(\theta_i)}{g(\theta_i)}\log q_{\phi}(\theta_i)\\[10pt]
&= \arg \max_{\phi} \sum_{i=1}^N \dfrac{\mathbb{1}_{\{J(\theta_i) > \gamma\}}(\theta_i)}{g(\theta_i)}\log q_{\phi}(\theta_i)\\[10pt]
\end{align*}
$$

where $\theta_i \sim g_{u}(\theta_i)$ and we have ignored the normalization constant of $p(\theta)$ since it does not contribute to the optimization.

We are allowed to choose any distribution $g(\theta)$ to sample from. It makes sense for us to use $q_{\phi'}(\theta)$, aka our initial guess of the distribution of $\theta$. We get:

$$
\begin{align*}
\phi^* &\approx \arg \max_{\phi} \sum_{i=1}^N \dfrac{\mathbb{1}_{\{J(\theta_i) > \gamma\}}(\theta_i)}{q_{\phi'}(\theta_i)}\log q_{\phi}(\theta_i)\\[10pt]
\end{align*}
$$

Therefore, we sample using our initial guess and optimize wrt $\phi$ in order to get a better guess, aka one that is closer to a distribution that yields higher cumulative rewards on average. If we keep doing this procedure iteratively, we get closer and closer to a distribution that yields higher rewards compared to the previous guesses. The optimization turns into:

$$
\begin{align*}
\phi^{(k+1)} &\approx \arg \max_{\phi} \sum_{i=1}^N \dfrac{\mathbb{1}_{\{J(\theta_i) > \gamma\}}(\theta_i)}{q_{\phi^{(k)}}(\theta_i)}\log q_{\phi}(\theta_i)\\[10pt]
\end{align*}
$$
 
Now consider the scenario in which out guess of $\phi^{(0)}$ is really bad. In this case it will be difficult to collect a large amount of samples that survive $$\mathbb{1}_{\{J(\theta_i) > \gamma\}}(\theta_i)$$. Additionally, if the samples that survive are assigned a very low likelihood by $q_{\phi^{(k)}}(\theta_i)$ the weights explode. To avoid this, we usually drop the $q_{\phi^{(k)}}(\theta_i)$ completely. The resulting $\phi^{(k+1)}$ may not be the same; however it will still be in the right direction, since the average is computed over "better" samples, regardless of their likelihood under the proposal distribution. The optimizer may not be as good as the one we would obtain in theory as $N \to \infty$, but it would still be good enough, and also practically computable. We arrive at:

$$
\begin{align*}
\phi^{(k+1)} &\approx \arg \max_{\phi} \sum_{i=1}^N \mathbb{1}_{\{J(\theta_i) > \gamma\}}(\theta_i) \log q_{\phi}(\theta_i)\\[10pt]
\end{align*}
$$

where $\theta_i \sim q_{\phi^{(k)}}(\theta_i)$.

Let $\mathcal{E} = \{\theta_i : i \in \{1, \dots, N\} \text{ and } J(\theta_i) > \gamma \}$ be the elite-set. The final objective can be rewritten as:

$$
\begin{align*}
\phi^{(k+1)} &\approx \arg \max_{\phi} \sum_{\theta \in \mathcal{E}} \log q_{\phi}(\theta)\\[10pt]
\end{align*}
$$

which makes it clear that the problem has been reduced to a maximum likelihood estimation over the elite samples.

## Modeling Controls as a Gaussian

If we choose our distribution $q_{\phi}$ to be Gaussian, aka $\phi = (\mu, \Sigma)$, then the problem is directly solved using the known maximizers:

$$
\mu = \dfrac{1}{|\mathcal{E}|}\sum_{\theta \in \mathcal{E}} \theta
\qquad

\text{and}

\qquad
\Sigma = \dfrac{1}{|\mathcal{E}|}\sum_{\theta \in \mathcal{E}} (\theta - \mu)(\theta - \mu)^T
$$

The problem has essentially been reduced to repeatedly perturbing our current guess and computing the average over the samples that produced better returns until convergence.

Convergence rate (in practice) depends on the number of samples we are able to collect in parallel. The optimum is not guaranteed to be globally optimal.

## Choosing the elite threshold $\gamma$

The choice of the elite threshold $\gamma$ in practice is quantile-based:

$$
\gamma = \text{Quantile}_{\rho}\left(\{J(\theta_i)\}_{i=1}^N\right)
$$

which stabilizes elite selection and avoids needing to tune $\gamma$ manually.
