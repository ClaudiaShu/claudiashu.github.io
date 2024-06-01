---
layout: post
title: "How to properly code latex notations"
date: 2024-02-07
categories: tutorials
permalink: /posts/2024-02-07-latex-notations/
usemathjax: true
---

In this guide, I'll walk you through the basics of coding LaTeX notations for academic writing. Let's get started! 🎉

<!-- # Proper latex notations -->
 <!-- -- continuous updating -->

## Mathematical Notations

**Matrix $$\mathbf{X}$$**: Matrices are represented by bold uppercase letters, such as `$\mathbf{X}$`, in LaTeX notation. Matrices are fundamental mathematical objects used to represent linear transformations and solve systems of linear equations.

**Vector $$\mathbf{x}$$**: Vectors are represented by bold lowercase letters, such as `$\mathbf{x}$`, in LaTeX notation. Vectors are commonly used to represent quantities with magnitude and direction, appearing in various fields like physics, engineering, and machine learning.

**Scalar $$x$$**: Scalars are represented by regular letters, such as `$x$`, in LaTeX notation. Scalars are single numerical values representing quantities like mass, distance, or temperature. Unlike vectors and matrices, scalars have magnitude but no direction.

**display-math expressions**: LaTeX doesn't officially support `$$`. Use `amsmath` package when using `\[`. Check for more details in: [post 1](https://tex.stackexchange.com/questions/503/why-is-preferable-to) and [post 2](https://tex.stackexchange.com/questions/40492/what-are-the-differences-between-align-equation-and-displaymath).

**Commas (`,`) for Horizontal Concatenation**: In mathematical notation, commas are used to separate vectors when concatenating them horizontally. This means that vectors are aligned side by side to form a row or a single vector. 

<!-- - - In latex: ```$\mathbf{X}=\left[\mathbf{x}_{t-L}, \mathbf{x}_{t-L+1}, \dots, \mathbf{x}_t \mid \mathbf{x}_i\!\in\! \mathbb{R}^{m \times 1} \right]$```  -->

**Semicolons (`;`) for Vertical Concatenation**: Similarly, semicolons are used to separate vectors when concatenating them vertically. This notation stacks vectors on top of each other to form a column or a matrix.

<!-- - - In latex: ```$\mathbf{X}=\left[\mathbf{x}_{t-L}; \mathbf{x}_{t-L+1}; \dots; \mathbf{x}_t \mid \mathbf{x}_i\!\in\! \mathbb{R}^{1 \times m} \right]$```  -->

## Spacing Notations

**Comma spacing $$1{,}000$$**: In LaTeX notation, comma spaces within numerical values can be properly formatted by enclosing the comma in curly braces, like in `$1{,}000$`. This notation ensures consistent and visually appealing spacing in mathematical expressions or numerical values.

**Spacing \~**: In LaTeX, the tilde symbol `~` is used to create a non-breaking space. This type of space prevents LaTeX from breaking the line at that point, ensuring that the elements separated by the tilde remain together on the same line. It is commonly used to create fixed spaces between words or symbols that should not be separated, for example `~\cite{}`.

# Others

In LaTeX, the default font for text is often Times, while the default math font is Latin Modern Math. When including figures in a document, it's essential to maintain consistency with the font choice and size used in the paper.

**Function $$f(\cdot)$$**: In LaTeX, when denoting a function, it's convention to use the function name in italic format as `$f(\cdot)$`.