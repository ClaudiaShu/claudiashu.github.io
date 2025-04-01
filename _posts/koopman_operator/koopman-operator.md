---
layout: post
title: "Introducing Koopman Operator for Time Series Forecasting"
date: 2025-02-13
categories: tutorials
tags: [math]
permalink: /posts/koopman-operator/
usemathjax: true
---


## **1. Introduction**
The **Koopman operator** provides a powerful lens for analyzing nonlinear dynamical systems by lifting them into an infinite-dimensional linear space. This tutorial explains its mathematical foundations, numerical implementation, and applications in **time series forecasting**, where it enables linear prediction of complex nonlinear trends.

---

## **2. Dynamical Systems & the Koopman Perspective**

### **2.1 Problem Setup**
Consider a discrete-time nonlinear system:  
$$ \mathbf{x}_{t+1} = \mathbf{F}(\mathbf{x}_t) $$  
where $$ \mathbf{x}_t \in \mathbb{R}^n $$ is the system state (e.g., stock prices, sensor readings). Directly modeling $$ \mathbf{F} $$ is challenging due to nonlinearity.

### **2.2 Koopman Operator Definition**
The Koopman operator $$ \mathcal{K} $$ acts on **observables** $$ g: \mathbb{R}^n \to \mathbb{C} $$:  
$$ \mathcal{K} g(\mathbf{x}_t) = g(\mathbf{F}(\mathbf{x}_t)) = g(\mathbf{x}_{t+1}) $$  
Critically, $$ \mathcal{K} $$ is **linear** even if $$ \mathbf{F} $$ is nonlinear. This allows spectral analysis and global linear prediction.


---

## **3. Spectral Decomposition & Forecasting**

### **3.1 Eigenfunctions and Modes**
Let $$ \phi_j(\mathbf{x}) $$ be eigenfunctions of $$ \mathcal{K} $$ with eigenvalues $$ \lambda_j $$:  
$$ \mathcal{K} \phi_j = \lambda_j \phi_j $$  
Any observable $$ g(\mathbf{x}) $$ can be expanded as:  
$$ g(\mathbf{x}) = \sum_{j=1}^\infty v_j \phi_j(\mathbf{x}) $$  
where $$ v_j $$ are Koopman modes. The time evolution becomes:  
$$ g(\mathbf{x}_t) = \sum_{j=1}^\infty v_j \lambda_j^t \phi_j(\mathbf{x}_0) $$  

### **3.2 Forecasting with Koopman Modes**
For the identity observable $$ \mathbf{g}(\mathbf{x}) = \mathbf{x} $$:  
$$ \mathbf{x}_t = \sum_{j=1}^\infty \mathbf{v}_j \lambda_j^t \phi_j(\mathbf{x}_0) $$  
This provides a **linear predictive model** in Koopman space, even for nonlinear systems.

---




---

## **7. Challenges in Time Series Forecasting**

1. **Observable Selection**: Poor observables lead to biased predictions.  
2. **Mode Truncation**: Finite-dimensional $$ \mathbf{K} $$ loses long-term accuracy.  
3. **Non-Stationarity**: $$ \mathcal{K} $$ may drift for time-varying systems.  

---

## **8. Applications**

1. **Finance**: Predict stock volatility with nonlinear Koopman modes.  
2. **Energy**: Forecast electricity demand from temperature/social trends.  
3. **Epidemiology**: Model disease spread with linearized dynamics.  

---

## **9. Further Reading**
- **Foundational Paper**: [Koopman (1931)](https://www.jstor.org/stable/1968242).  
- **EDMD Tutorial**: [Williams et al. (2015)](https://arxiv.org/abs/1408.4408).  
- **Code Examples**: [PyKoopman](https://github.com/dynamicslab/pykoopman).  

---

The Koopman operator transforms nonlinear forecasting into a linear problem in a learned latent space, blending dynamical systems theory with modern machine learning. By mastering its spectral properties and approximation techniques, you can tackle complex temporal patterns with rigor and efficiency.