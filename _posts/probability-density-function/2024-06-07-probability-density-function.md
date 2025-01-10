---
layout: post
title: "Transforming the probability density function (PDF)"
date: 2024-06-07
categories: tutorials
tags: [math]
permalink: /posts/probability-density-function/
usemathjax: true
---

Let us consider the transformation of probability density function under two cases: transformation with a random variable and with a random vector.

## Case 1: Random variable

Given:

- A random variable $$z$$ with PDF $$\pi(z)$$.
- A transformation $$x=f(z)$$.

We have the inverse transformation $$z=f^{-1}(x)$$. The target is to get the PDF of $$x$$, denoted as $$p(x)$$ of the new random variable $$x$$.

The CDF of $$z$$, $$F_Z(z)$$, is $$\mathbb{P}(Z \leq z)$$.

The CDF of $$x$$, $$F_X(x)$$, is $$\mathbb{P}(X \leq x)$$.

Using the transformation $$x=f(z)$$, we can write the CDF of $$x$$ in terms of $$z$$:

$$   F_X(x) 
    = \mathbb{P}(X \leq x) 
    = \mathbb{P}(f(Z) \leq x)
    =\mathbb{P}(Z \leq f^{-1}(x))
    =F_Z(f^{-1}(x)) .$$

Differentiate $$F_X(x)$$ with respect to $$x$$ to obtain the PDF $$p(x)$$:

$$ p(x) = \frac{dF_X(x)}{dx} = \frac{dF_Z(f^{-1}(x))}{dx} $$

Using the chain rule for differentiation:

$$ \frac{dF_Z(f^{-1}(x))}{dx} = \frac{dF_Z(f^{-1}(x))}{d(f^{-1}(x))} \cdot \frac{d(f^{-1}(x))}{dx} .$$

The derivative of the CDF $$F_Z(z)$$ with respect to $$z$$ is the PDF $$\pi(z)$$:

$$ \frac{dF_Z(z)}{dz} = \pi(z) .$$

Thus:

$$ p(x) = \pi(f^{-1}(x)) \cdot \frac{d(f^{-1}(x))}{dx} .$$

Alternatively, using $$z = f^{-1}(x)$$ and the inverse function theorem:

$$ \frac{d(f^{-1}(x))}{dx} = \frac{1}{\frac{df(z)}{dz}|_{z=f^{-1}(x)}} .$$

Therefore:

$$ p(x) = \pi(f^{-1}(x)) \left| \frac{d}{dx} f^{-1}(x) \right| .$$


## Case 2: Random vector

Given:

- A random vector $$\mathbf{z}$$ with joint PDF $$\pi(\mathbf{z})$$.
- A transformation $$\mathbf{x} = f(\mathbf{z})$$.
The inverse transformation can also be written as $$\mathbf{z} = f^{-1}(\mathbf{x})$$, and we want to get the joint PDF $$p(\mathbf{x})$$ of the new random vector $$\mathbf{x}$$.

**1. Jacobian Determinant:**

The transformation $$\mathbf{x} = f(\mathbf{z})$$ involves multiple variables, so we need to consider the Jacobian matrix of partial derivatives:

$$ J = \frac{\partial(x_1,x_2,\ldots,x_n)}{\partial(z_1,z_2,\ldots,z_n)} =
\begin{pmatrix}
\frac{\partial x_1}{\partial z_1} & \frac{\partial x_1}{\partial z_2} & \cdots & \frac{\partial x_1}{\partial z_n} \\
\frac{\partial x_2}{\partial z_1} & \frac{\partial x_2}{\partial z_2} & \cdots & \frac{\partial x_2}{\partial z_n} \\
\vdots & \vdots & \ddots & \vdots \\
\frac{\partial x_n}{\partial z_1} & \frac{\partial x_n}{\partial z_2} & \cdots & \frac{\partial x_n}{\partial z_n} \\
\end{pmatrix} .$$


**2. Change of Variables Formula:**

The joint PDF $$p(\mathbf{x})$$ is given by:

$$ p(\mathbf{x}) = \pi(\mathbf{z}) \cdot |\text{det}(J)| \text{ where } \mathbf{z} = f^{-1}(\mathbf{x}). $$

Here, $$|\text{det}(J)|$$ is the determinant of the Jacobian matrix.
 <!-- evaluated at $$\mathbf{z} = f^{-1}(\mathbf{x})$$. -->

**Special Case: Linear Transformation**

If $$\mathbf{z} \sim \mathcal{N}(\mu,\sigma^2)$$ and the transformation is linear, such as $$\mathbf{x} = a\mathbf{z} + b$$, the resulting distribution will also be normal due to the properties of the normal distribution.

For a linear transformation $$\mathbf{x} = a\mathbf{z} + b$$:

$$ \mathbf{x} \sim \mathcal{N}(a\mu + b,(a\sigma)^2). $$


