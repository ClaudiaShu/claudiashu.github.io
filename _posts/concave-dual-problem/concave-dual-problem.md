---
layout: post
title: "The concavity of dual function"
date: 2024-06-07
categories: tutorials
permalink: /posts/concave-dual-problem/
usemathjax: true
---

When I was reading the book "Mathematics for Machine learning", I :

Therefore $\min_{x \in \mathbb{R}^d} L(x, \lambda)$ is a pointwise minimum of affine functions of $\lambda$, and hence $D(\lambda)$ is concave even though $f(\cdot)$ and $g_i(\cdot)$ may be nonconvex.

I got a bit confused at first, so I decided to make a post on this to ().


# Concavity of the Dual Function in Lagrange Duality

## Background: Lagrangian and Dual Function

Given a primal optimization problem:

$$ \min_{x \in \mathbb{R}^d} f(x) \quad \text{subject to} \quad g_i(x) \leq 0, \quad i = 1, \ldots, m $$

We form the Lagrangian:

$$ L(x, \lambda) = f(x) + \sum_{i=1}^m \lambda_i g_i(x) $$

where $ \lambda_i \geq 0 $ are the Lagrange multipliers.

The dual function $ D(\lambda) $ is defined as:

$$ D(\lambda) = \inf_{x \in \mathbb{R}^d} L(x, \lambda) $$

## Key Points

1. **Affine Functions of $ \lambda $**:
   The Lagrangian $ L(x, \lambda) = f(x) + \sum_{i=1}^m \lambda_i g_i(x) $ is affine in $ \lambda $ for a fixed $ x $. This means that for each $ x $, $ L(x, \lambda) $ is a linear combination of the $ \lambda_i $ terms with coefficients $ g_i(x) $.

2. **Pointwise Infimum of Affine Functions**:
   The dual function $ D(\lambda) = \inf_{x} L(x, \lambda) $ is the pointwise infimum of the affine functions $ L(x, \lambda) $ over all $ x $.

## Proof of Concavity

- Consider $ \lambda^1, \lambda^2 \in \mathbb{R}^m $ and a scalar $ \theta \in [0, 1] $.

- Define $ \lambda^\theta = \theta \lambda^1 + (1 - \theta) \lambda^2 $.

- By definition of the dual function:

  $$ D(\lambda^\theta) = \inf_{x} \left[ f(x) + \sum_{i=1}^m (\theta \lambda_i^1 + (1 - \theta) \lambda_i^2) g_i(x) \right] $$

- We can rewrite this as:

  $$ D(\lambda^\theta) = \inf_{x} \left[ \theta \left( f(x) + \sum_{i=1}^m \lambda_i^1 g_i(x) \right) + (1 - \theta) \left( f(x) + \sum_{i=1}^m \lambda_i^2 g_i(x) \right) \right] $$

- Since the infimum of a sum is less than or equal to the sum of the infima (by the properties of infima):

  $$ D(\lambda^\theta) \leq \theta \inf_{x} \left[ f(x) + \sum_{i=1}^m \lambda_i^1 g_i(x) \right] + (1 - \theta) \inf_{x} \left[ f(x) + \sum_{i=1}^m \lambda_i^2 g_i(x) \right] $$

- Which simplifies to:

  $$ D(\lambda^\theta) \leq \theta D(\lambda^1) + (1 - \theta) D(\lambda^2) $$

- This inequality shows that $ D(\lambda) $ is concave, because $ D(\lambda^\theta) \leq \theta D(\lambda^1) + (1 - \theta) D(\lambda^2) $ holds for all $ \lambda^1, \lambda^2 \in \mathbb{R}^m $ and $ \theta \in [0, 1] $.

## Conclusion

The concavity of $ D(\lambda) $ arises because it is defined as the pointwise infimum of a family of affine functions in $ \lambda $. Even if the functions $ f(x) $ and $ g_i(x) $ are nonconvex in $ x $, the pointwise infimum operation over affine functions of $ \lambda $ results in a concave function.
