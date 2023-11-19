---
layout: post
toc:
  sidebar: left
title: Why is the Denominator of Sample Variance n-1?
---

## Mathematical Expectation
If $f(x)$ is the [probability mass function](https://en.wikipedia.org/wiki/Probability_mass_function) (p.m.f.) of the discrete random variable $X$ with support $S$, and if the summation:

$$
\sum_{x\in S} u(x)f(x)
$$

exists (less than $\infty$), then the resulting sum is called the **mathematical expectation**, or the expected value of the function $u(X)$, where $x$ denotes individual sample point. The expectation of the function $u(X)$ is denoted as $E[u(X)]$:

$$
E[u(X)] = \sum_{x\in S} u(x)f(x).
$$

## Population Mean and Variance
If we consider one particular function: $u(X) = X$, then the expectation of $u(X)$:

$$
E[u(X)] = E[X] = \sum_{x\in S} xf(x),
$$

is called the **expected value** of $X$, denoted as $E[X]$. Or, it is called the **mean** of $X$, denoted as $\mu$. 

Now, let's consider another function: $u(X) = (X-\mu)^2$, the corresponding expectation:

$$
E[u(X)] = E[(X-\mu)^2] = \sum_{x\in S}(x-\mu)^2f(x),
$$

is called the **variance** of $X$, denoted as $\mathrm{Var}(X)$ or $\sigma^2$.

## Sample Mean and Variance
If the population is too large and we still want to decribe the population using mean and variance, then it would be useful to select a sample and calculate the sample mean and sample variance.

The **sample mean** is simply the average of the $n$ data points $x_1$, $x_2$, ..., $x_n$:

$$
\bar{x} = \frac{x_1+x_2+...+x_n}{n} = \frac{1}{n}\sum_{i=1}^n x_i.
$$

The **sample variance** summaries the "spread" or "variation" of the data:

$$
\begin{aligned}
    s^2 &= \frac{(x_1-\bar{x})^2+(x_2-\bar{x})^2+ ... +(x_n-\bar{x})^2}{n-1} \\
    &= \frac{1}{n-1}\sum_{i=1}^n(x_i-\bar{x})^2.
\end{aligned}

$$

## Important Properties
Note that the denominator of the sample variance is $n-1$. Why is that? Before we start to analyse that, we first look at some important properties of expectation. The proof will be given in the next section.

**Property 1**:

if c is a constant, then $E[c] = c$ and $E[cu(X)] = cE[u(X)]$.

**Property 2**:

$E[u_1(X) + u_2(X)] = E[u_1(X)] + E[u_2(X)]$.

**Property 3**:

if $X_1$, $X_2$, ..., $X_n$ are $n$ independent random variables with means $\mu_1$, $\mu_2$, ..., $\mu_n$ and variances $\sigma_1^2$, $\sigma_2^2$, ..., $\sigma_n^2$. Then, the mean and variance of the linear combination $Y = \sum_{i=1}^{n}a_iX_i$ ($a_i$ are real constants) are:

$$
\begin{aligned}
    \mu_Y &= \sum_{i=1}^n a_i\mu_i \\
\sigma_Y^2 &= \sum_{i=1}^n a_i^2 \sigma_i^2.
\end{aligned}
$$


## Proof
Let's do some proof in this section.

**Property 1**:

$$
\begin{aligned}
    E[c] &= \sum_{x\in S}cf(x) = c\sum_{x\in S}f(x) = c \times 1 = c \\
    E[cu(X)] &= \sum_{x\in S}cu(X)f(x) = c\sum_{x\in S}u(X)f(x) = cE[u(X)].
\end{aligned}
$$

**Property 2**:

$$
\begin{aligned}
    E[u_1(X) + u_2(X)] &= \sum_{x\in S} (u_1(X) + u_2(X))f(x) \\
    &= \sum_{x\in S} u_1(X)f(x) + \sum_{x\in S} u_2(X)f(x) \\
    &= E[u_1(X)] + E[u_2(X)]
\end{aligned}
$$

**Property 3**:

$$
\begin{aligned}
    \mu_Y &= E[Y] = E[\sum_{i=1}^n a_iX_i] = \sum_{i=1}^n E[a_iX_i] \\
    &= \sum_{i=1}^n a_i E[X_i] = \sum_{i=1}^n a_i\mu_i
\end{aligned}
$$

$$
\begin{aligned}
    \sigma_Y^2 &= E[(Y-\mu_Y)^2] \\
    &= E[(\sum_{i=1}^n a_iX_i - \sum_{i=1}^n a_i\mu_i)^2] \\
    &= E[(\sum_{i=1}^na_i(X_i-\mu_i))^2] \\
    &= E[(\sum_{i=1}^na_i(X_i-\mu_i))\cdot (\sum_{j=1}^na_j(X_j-\mu_j))] \\
    &= E[\sum_{i=1}^n\sum_{j=1}^n a_ia_j(X_i-\mu_i)(X_j-\mu_j)] \\
    &= \sum_{i=1}^n\sum_{j=1}^n a_ia_j E[(X_i-\mu_i)(X_j-\mu_j)].
\end{aligned}
$$

Since $X_1$, $X_2$, ..., $X_n$ are independent random variables, the [correlation](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient) between two arbitrary variables $X_i$ and $X_j$ with $i\neq j$ is zero. This leads us to:

$$
\sigma_Y^2 = \sum_{i=1}^n a_i^2 E[(X_i-\mu_i)^2] = \sum_{i=1}^n a_i^2 \sigma_i^2.
$$

## Why is the Denominator of Sample Variance n-1?
If we take the number of data points $n$ as the denominator of the sample variance, we will see that this sample variance is a biased estimator of the population variance:

$$
\begin{aligned}
    s_n^2 &= E[\frac{1}{n}\sum_{i=1}^n(x_i-\bar{x})^2] \\
    &= E[\frac{1}{n}\sum_{i=1}^n[(x_i-\mu)-(\bar{x}-\mu)]^2] \\
    &= E[\frac{1}{n}\sum_{i=1}^n(x_i-\mu)^2-2(x_i-\mu)\underbrace{(\bar{x}-\mu)}_{const.}+\underbrace{(\bar{x}-\mu)^2}_{const.}] \\
    &= E[\frac{1}{n}\sum_{i=1}^n(x_i-\mu)^2 - \frac{2(\bar{x}-\mu)}{n}\underbrace{\sum_{i=1}^n(x_i}_{n\cdot\bar{x}}-\mu) + (\bar{x}-\mu)^2] \\
    &= E[(X-\mu)^2]- E[2(\bar{x}-\mu)(\bar{x}-\mu)] + E[(\bar{x}-\mu)^2]]\\
    &= E[(X-\mu)^2] -E[(\bar{x}-\mu)^2] \\
    &= \sigma^2 - \mathrm{Var}(\bar{x}) \\
\end{aligned}
$$

The bias is therefore $\mathrm{Var}(\bar{x})$, i.e. variance of the sample mean. According to the Property 3, this term can be calculated by:

$$
\begin{aligned}
    \mathrm{Var}(\bar{x}) &= \mathrm{Var}(\frac{1}{n}\sum_{i=1}^nx_i) = \mathrm{Var}(\sum_{i=1}^n \frac{1}{n}x_i) \\
    &= \sum_{i=1}^n \frac{1}{n^2}\mathrm{Var}(x_i) = \frac{1}{n^2}\sum_{i=1}^n\mathrm{Var}(x_i).
\end{aligned}
$$

Since $x_1$, $x_2$, …, $x_n$ are a random sample from a distribution with variance $\sigma^2$, it follows that for each $i = 1, 2, …, n$: $\mathrm{Var}(x_i)=\sigma^2$. Thus, the variance of the sample mean can be further transformed to:

$$
\mathrm{Var}(\bar{x}) = \frac{1}{n}\sigma^2.
$$

Now, we know that the uncorrected sample variance is $s_n^2 = \frac{n-1}{n}\sigma^2$. Thus, the corrected one would be:

$$
\begin{aligned}
    s^2 &= \frac{n}{n-1}s_n^2 = \frac{n}{n-1}\cdot \frac{1}{n}\sum_{i=1}^n(x_i-\bar{x})^2 \\
    &= \frac{1}{n-1}\sum_{i=1}^n(x_i-\bar{x})^2.
\end{aligned}
$$

## Reference
- [Bessel Correction Wiki](https://en.wikipedia.org/wiki/Bessel%27s_correction)
- [PennState Statistics Lecture Sample Means and Variance](https://online.stat.psu.edu/stat414/lesson/8/8.5)
- [PennState Statistics Lecture Mean and Variance of Sample Mean](https://online.stat.psu.edu/stat414/lesson/24/24.4)