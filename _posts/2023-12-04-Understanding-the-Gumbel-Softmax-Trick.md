---
layout: post
toc:
  sidebar: left
title: Understanding the Gumbel-Softmax Trick
---

Deep learning models often face challenges when dealing with categorical data, 
especially in tasks that require differentiable sampling. 
The Gumbel-Softmax distribution emerges as a solution, blending discrete choice 
modeling with gradient-based optimization. In this blog, we explore the 
Gumbel-Softmax distribution, its mechanism, applications, and advantages over 
traditional softmax in certain scenarios.

## What is Gumbel-Softmax?

Gumbel-Softmax is a technique in deep learning that enables differentiable 
sampling from a categorical distribution, useful in scenarios involving 
discrete data generation or classification.

### Background

In machine learning models, particularly classification tasks, we deal with 
categorical distributions. A categorical distribution is a discrete probability 
distribution that describes the possible results of a random variable that can 
take on one of $$K$$ possible categories, with the probability of each category separately specified.
However, traditional sampling from these distributions is non-differentiable, 
posing challenges for gradient-based learning methods. 
Gumbel-Softmax provides a solution for differentiable sampling.

## How Gumbel-Softmax Works

**Introduction of Gumbel Noise**: Add a sample from Gumbel(0, 1) to the log probabilities of categories.

   $$
   \text{score}_k = \log(p_k) + G_k
   $$

**Application of Softmax Function**: Apply softmax to these scores with a temperature parameter ($$\tau$$) controlling the output's 'softness'.
When $$\tau$$ is close to 0, the softmax function approximates a categorical sample (hard selection).
When $$\tau$$ is high, the choices become softer, allowing for a smoother gradient.

$$ 
y_k = \frac{\exp((\log(p_k) + G_k) / \tau)}{\sum_{i=1}^{K} \exp((\log(p_i) + G_i) / \tau)} 
$$

**Output**: The result is a vector approximating a one-hot encoded vector but is differentiable with respect to model parameters.

## Gumbel-Softmax vs. Regular Softmax
While softmax is common in classification models, it's limited in scenarios requiring differentiable sampling. 
Gumbel-Softmax maintains differentiability due to the incorporation of the Gumbel noise, unlike regular softmax.

## Why Gumbel Noise? Can We use Gaussian Noise instead?
The Gumbel distribution arises naturally in the context of extreme value theory, 
particularly in modeling the distribution of the maximum (or minimum) of a set of 
samples from various distributions. In the case of Gumbel-Softmax, it is used to 
transform the categorical logits into a sample from the categorical distribution.
Gaussian noise, while offering differentiability, lacks the direct mapping to 
categorical sampling provided by Gumbel noise.