---
layout: post
title: Introduction of Common Optimizers in Deep Learning
toc:
  sidebar: left
---

## Basic Algorithms
### Stochastic Gradient Descent (SGD)
Stochastic gradient descent is an extension of the gradient descent algorithm. Instead of being computed on the entire training sets, the SGD is computed on one minibatch drawn uniformly from the training set. The insight behind this is that the gradient is an expectation and the expectation may be approximately estimated using a small set of samples.

$$
\begin{aligned}
    \boldsymbol{g} &\leftarrow \frac{1}{m} \nabla_\theta \sum_{i=1}^{m}L(f(x^{(i)};\boldsymbol{\theta}), y^{(i)}) \\
    \boldsymbol{\theta} &\leftarrow \boldsymbol{\theta} - \epsilon \boldsymbol{g}
\end{aligned}
$$

![](/assets/images/Optimizer/SGD_without_momentum.gif){:width=50% .align-center}
*Figure 1. SGD without momentum.*

Challenges:
- Prone to oscillation. When the horizontal and vertical gradients are different (c.f. Fig. 1), the horizontal gradient updates slowly, while the vertical gradients updates quickly, as the learning rate for gradient updates in each direction is the same, causing oscillation in vertical direction.
- Difficult to choose a proper learning rate. As shown in Figure 1, a too large learning rate can hinder convergence or cause oscillation (vertical), while a too small learning rate leads to slow convergence (horizontal).
- Saddle points, i.e. points where one dimension slopes up and another slopes down. The gradient is close to zero in all dimensions, which makes it hard for SGD to escape. 

### Momentum
The momentum algorithm introduces a variable $v$ that plays the role of velocity, set to an exponentially decay average of the negative gradient. 

$$
\begin{aligned}
    \boldsymbol{g} &\leftarrow \frac{1}{m} \nabla_\theta \sum_{i=1}^{m}L(f(x^{(i)};\boldsymbol{\theta}), y^{(i)}) \\
    \boldsymbol{v} &\leftarrow \alpha\boldsymbol{v}-\epsilon\boldsymbol{g} \\
    \boldsymbol{\theta} &\leftarrow \boldsymbol{\theta} + \boldsymbol{v}
\end{aligned}
$$

Momentum helps accelerate SGD in the relevant direction and dampens oscillations as can be seen in Figure 2. The update step size is largest when many successive gradients point in exactly the same direction. In Figure 2, the horizontal gradients always point to the right and thus, will be updated faster.

![](/assets/images/Optimizer/SGD_with_momentum.gif){:width=50% .align-center}
*Figure 2. SGD with momentum.*

As this algorithm considers the gradient from the previous time step, the saddle point of current step could be escaped, although the current gradient is close to zero. Common values of the momentum term $\alpha$ used in practice include 0.5, 0.9 and 0.99.

### Nesterov Momentum
Nesterov momentum is a slightly different version of the momentum update. The gradient now is evaluated after the current velocity is applied. Thus one can interpret Nesterov momentum as attempting to add a **correction factor** to the standard method of momentum.

![](/assets/images/Optimizer/nesterov.jpeg){:width=80% .align-center}
*Figure 3. Difference between Momentum and Nesterov momentum.*

The update rules are given by:

$$
\begin{aligned}
    \hat{\boldsymbol{\theta}} &\leftarrow \boldsymbol{\theta} + \alpha \boldsymbol{v} \\
    \boldsymbol{g} &\leftarrow \frac{1}{m} \nabla_{\hat{\theta}} \sum_{i=1}^{m}L(f(x^{(i)};\boldsymbol{\theta}), y^{(i)}) \\
    \boldsymbol{v} &\leftarrow \alpha\boldsymbol{v}-\epsilon\boldsymbol{g} \\
    \boldsymbol{\theta} &\leftarrow \hat{\boldsymbol{\theta}} -\epsilon\boldsymbol{g}
\end{aligned}
$$

## Algorithms with Adaptive Learning Rates

### AdaGrad
The AdaGrad algorithm individually adapts the learning rates of all model parameters by scaling them inversely proportional to the square root of the sum of all of their historical squared values. The parameters that receive high gradients will have their effective learning rate reduced, while parameters that receive small or infrequent updates will have their effective learning rate increased. Amusingly, the square root operation turns out to be very important and without it the algorithm performs much worse.

$$
\begin{aligned}
    \boldsymbol{g} &\leftarrow \frac{1}{m} \nabla_{\theta} \sum_{i=1}^{m}L(f(x^{(i)};\boldsymbol{\theta}), y^{(i)}) \\
    \boldsymbol{r} &\leftarrow \boldsymbol{r} + \boldsymbol{g} \odot \boldsymbol{g} \\
    \boldsymbol{\theta} &\leftarrow \boldsymbol{\theta} - \frac{\epsilon}{\delta+\sqrt{\boldsymbol{r}}} \odot \boldsymbol{g}
\end{aligned}
$$

The main weakness is its accumulation of the squared gradients in the denominator: Since every added term is positive, the accumulated sum keeps growing during training. This in turn causes the learning rate to shrink and eventually become infinitesimally small, at which point the algorithm is no longer able to acquire additional knowledge.

### RMSProp
The RMSProp update adjusts the AdaGrad method in a very simple way in an attempt to reduce its aggressive, monotonically decreasing learning rate. In particular, it uses a exponentially moving average of squared gradients instead. The RMSProp still modulates the learning rate of each parameter based on the magnitudes of its gradients, which has a beneficial equalizing effect, but unlike AdaGrad the updates do not get monotonically smaller.

$$
\begin{aligned}
    \boldsymbol{g} &\leftarrow \frac{1}{m} \nabla_{\theta} \sum_{i=1}^{m}L(f(x^{(i)};\boldsymbol{\theta}), y^{(i)}) \\
    \boldsymbol{r} &\leftarrow \rho\boldsymbol{r} + (1-\rho)\boldsymbol{g} \odot \boldsymbol{g} \\
    \boldsymbol{\theta} &\leftarrow \boldsymbol{\theta}-\frac{\epsilon}{\sqrt{\delta+\boldsymbol{r}}}\odot \boldsymbol{g}
\end{aligned}
$$

### Adam
The name "Adam" derives from the phrase "adaptive moments". In addition to storing an exponentially decaying average of past squared gradients like RMSProp, Adam also keeps an exponentially decaying average of past gradients, similar to momentum. Furthermore, Adap includes bias corrections to the estimates of both the first-order moments and the second-order moments to account for their initialization at the origin.

$$
\begin{aligned}
    \boldsymbol{g} &\leftarrow \frac{1}{m} \nabla_{\theta} \sum_{i=1}^{m}L(f(x^{(i)};\boldsymbol{\theta}), y^{(i)}) \\
    \boldsymbol{s} &\leftarrow \rho_1\boldsymbol{s} + (1-\rho_1)\boldsymbol{g} \\
    \boldsymbol{r} &\leftarrow \rho_2\boldsymbol{r} + (1-\rho_2)\boldsymbol{g} \odot \boldsymbol{g} \\
    \hat{s} &\leftarrow \frac{\boldsymbol{s}}{1-\rho_1^t} \\
    \hat{\boldsymbol{r}} &\leftarrow \frac{\boldsymbol{r}}{1-\rho_2^t} \\
    \boldsymbol{\theta} &\leftarrow \boldsymbol{\theta}-\epsilon\frac{\hat{\boldsymbol{s}}}{\sqrt{\hat{\boldsymbol{r}}}+\delta} (operations applied element-wise)
\end{aligned}
$$

> In practice Adam is currently recommended as the default algorithm to use, and often works slightly better than RMSProp. However, it is often also worth trying SGD+Nesterov Momentum as an alternative.

## Reference
- Deep Learning Book
- https://cs231n.github.io/neural-networks-3/
- https://ruder.io/optimizing-gradient-descent/
- https://ocxs.gitbooks.io/deep-learning/content/cs231n/001_training_optimization.html
