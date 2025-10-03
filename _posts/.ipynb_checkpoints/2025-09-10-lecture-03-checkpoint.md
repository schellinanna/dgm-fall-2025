---
layout: distill
title: Lecture 03
description: Statistics / linear algebra / calculus review
date: 2025-09-10

lecturers:
  - name: Ben Lengerich
    url: "https://adaptinfer.org"

authors:
  - name: Jeff Zhang # author's full name
    url: "#" # optional URL to the author's homepage
  - name: Youngwoo Kim
    url: "#"
  - name: Yongqi Zhang
    url: "#"

editors:
  - name: Eva Song # editor's full name
    url: "#" # optional URL to the editor's homepage

abstract: >
  This lecture provides a brief review of fundamental math skills for deep learning. It provides some basic programming techniques about tensors and PyTorch. This lecture also reviews some important math and statistics topics in deep learning, including linear algebra, probability, estimation, and linear regression.
---

## Today's Topics:
- [Today's Topics:](#todays-topics)
- [1. Tensors in Deep Learning](#1-tensors-in-deep-learning)
- [2. Tensors and PyTorch](#2-tensors-and-pytorch)
- [3. Vectors, Matrices, and Broadcasting](#3-vectors-matrices-and-broadcasting)
- [4. Probability Basics](#4-probability-basics)
- [5. Estimation Methods](#5-estimation-methods)
- [6. Linear Regression](#6-linear-regression)


## 1. Tensors in Deep Learning
- A **tensor** is a multidimensional array
- Dimensionality (order) = number of indices
- It is generalization of scalars(order-0 tensor), vectors(order-1 tensor), matrices(order-2 tensor).
- Images are common examples of tensor used as input in deep learning.
- 3D tensor : a single color image (height, width, channels)
- 4D tensor : a batch of images (batch_size, height, width, channels)


## 2. Tensors and PyTorch
- NumPy vs PyTorch: They have similar syntax but PyTorch adds:
  - GPU support,
  - automatic differentiation,
  - deep learning convenience functions.  
- Data types: mappings exist (e.g., NumPy `float64` → PyTorch `DoubleTensor`).   
- Matrix multiplication in Pytorch: `b.matmul(b)`, `b.dot(b)`, `b @ b`  .  
- Check GPU availability in PyTorch and move tensors between CPU and GPU using `b.to(torch.device('cpu'))` or `b.to(torch.device('cuda:0'))`


## 3. Vectors, Matrices, and Broadcasting
- **Notations:** 
  - x (lower case): vector
  - X (upper case): matrix
  - w: weights **(also a vector)**
  - b: bias **(a constant)**
  - z: pre-activation value
- **Vectors:**
  - A vector is an **n-by-1** matrix, where **n** is the number of **row**, and **1** is the number of **column**. 
  - In deep learning, we often represent vectors as **column vectors**.
  - The linear combination (pre-activation value) is written: $z = \mathbf{w}^\top \mathbf{x} + b$, where
  - 
$$
  \mathbf{x} =
  \begin{bmatrix}
  x_1 \\
  x_2 \\
  \vdots \\
  x_m
  \end{bmatrix}
  \in \mathbb{R}^{m \times 1},
  \quad
  \mathbf{w} =
  \begin{bmatrix}
  w_1 \\
  w_2 \\
  \vdots \\
  w_m
  \end{bmatrix}
  \in \mathbb{R}^{m \times 1},
  \quad
  z \in \mathbb{R}
$$

- **Matrices:**
  - A matrix is an **n-by-m arrays** of numbers, where **n** is the number of **row**, and **m** is the number of **column**.
  - The linear combination is written: $\mathbf{z} = \mathbf{Xw} + b$, where

$$
  \mathbf{X} =
  \begin{bmatrix}
  x^{[1]}_1 & x^{[1]}_2 & \cdots & x^{[1]}_m \\
  x^{[2]}_1 & x^{[2]}_2 & \cdots & x^{[2]}_m \\
  \vdots & \vdots & \ddots & \vdots \\
  x^{[n]}_1 & x^{[n]}_2 & \cdots & x^{[n]}_m
  \end{bmatrix}
  \in \mathbb{R}^{n \times m},
  \quad
  \mathbf{w} =
  \begin{bmatrix}
  w_1 \\
  w_2 \\
  \vdots \\
  w_m
  \end{bmatrix}
  \in \mathbb{R}^{m \times 1},
  \quad
  \mathbf{z} =
  \begin{bmatrix}
  z^{[1]} \\
  z^{[2]} \\
  \vdots \\
  z^{[n]}
  \end{bmatrix}
  \in \mathbb{R}^{n \times 1}
$$
  - Time Complexity of N-by-N matrices multiplication by naive algorithms: $O(n^3)$.
- **Broadcasting:** 
  - The rigorous math formula of linear combination is: $\mathbf{z} = \mathbf{Xw} + \mathbf{1}_n b$.
  - Using NumPy / PyTorch / TensorFlow, **broadcasting** helps to automatically expands the scalar $b$ into an $n \times 1$ vector.
  - Thus, in deep learning notation, we usually drop $\mathbf{1}_n$ and simply write $\mathbf{z} = \mathbf{Xw} + b$.
- **Broadcasting Usage:** 
  - Be cautious when debugging, since **broadcasting expands tensors automatically**.
  - Example:
    - `# 1 is broadcast to [1, 1, 1] to match the vector`  
    `torch.tensor([1, 2, 3]) + 1`  
    → `tensor([2, 3, 4])`

    - `# [1, 2, 3] is broadcast to [[ 1,  2,  3], [ 1, 2, 3]] to match the matrix`  
    `t = torch.tensor([[4, 5, 6], [7, 8, 9]])`  
    `t + torch.tensor([1, 2, 3])`  
    → `tensor([[ 5,  7,  9], [ 8, 10, 12]])`


## 4. Probability Basics
- **Random Variables and Distributions:**
  - **Discrete Random Variables:** Values from a **countable** set (e.g. coin flip).
    - Described by: **Probability Mass Function (PMF)**: $P(X = x)$.
    - Example: **Bernoulli Distribution**
      $P(X = x) = \theta^x (1-\theta)^{1-x}, \; x \in \{0,1\}$.
      - Example: fair coin flip ($\theta = 0.5$).
  - **Continuous Random Variables:**  Values from an interval (e.g. a height).
    - Described by **Probability Density Function (PDF)**:  $f(x)$.
    - Example: **Gaussian (Normal) Distribution**
      $f(x) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
      - Called **"Normal"** because of the **Central Limit Theorem**.
      - **Standard Normal:** when $\mu= 0, \sigma= 1$.
- **Central Limit Theorem (CLT):**
  - Let $X_1, X_2, \ldots, X_n$ be independent and identically distributed (i.i.d.) random variables with mean $\mu$ and finite variance $\sigma^2$.
  - Define the **sample mean**: $\bar{X}_n = \frac{1}{n}(X_1 + X_2 + \cdots + X_n)$
  - Then we have: $\frac{\bar{X}_n - \mu}{\frac{\sigma}{\sqrt{n}}} \to N(0,1)$ as $n \to \infty$
- **Joint, Marginal, and Conditional Probabilities:**
  - **Joint**: $P(A,B)$, probability of two events occurring together.
  - **Marginal**: $P(A) = \sum_B P(A, B)$, sum of joint probabilities over one variable.
  - **Conditional**: $P(A \mid B) = \frac{P(A, B)}{P(B)}$, probability of A given B.
- **Expectation:**
  - Formula:
    - Discrete: $E[X] = \sum_x x P(X = x)$ 
    - Continuous: $E[X] = \int_{-\infty}^{\infty} x f(x)\, dx$
  - Linearity:
    - $E[aX + b] = aE[X] + b$
    - $E[X_1 + X_2] = E[X_1] + E[X_2]$
  - Expectation of Functions:
    - **Discrete:**
      - $E[g(X)] = \sum_x g(x) P(X = x)$
      - Example: $X \sim \text{Bernoulli}(\theta), g(X) = X^2$, then $E[g(X)] = 1^2 \theta + 0^2 (1-\theta) = \theta$
    - **Continuous:**
      - $E[g(X)] = \int_x g(x) f(x)\, dx$
      - Example: $X \sim \text{Uniform}(0,1), \, g(X) = X^2$, then $E[g(X)] = \int_0^1 x^2 dx = \tfrac{1}{3}$
- **Variance:**
  - Formula: $Var(X) = E[(X - E[X])^2] \equiv E[X^2] - (E[X])^2$ 
  - Variance of Functions: $Var(g(X)) = E[(g(X) - E[g(X)])^2] \equiv E[g(X)^2] - E[g(X)]^2$
- **Covariance:**
  - $Cov(X,Y) = E[(X - E[X])(Y - E[Y])]$
  - If $X, Y$ are independent, then $Cov(X,Y) = 0$
  - $Cov(X,X) = Var(X)$
- **Correlation:**
  - $\rho(X, Y) = \dfrac{\mathrm{Cov}(X,Y)}{\sqrt{Var(X)Var(Y)}}$
    - $\rho = 1$: Perfect positive linear relationship.
    - $\rho = 0$: No linear relationship. 
    - $\rho = -1$: Perfect negative linear relationship.
    - Always in the range $[-1, 1]$.
- **Bayes'Rule:**
  - $P(A \mid B) = \frac{P(B \mid A) P(A)}{P(B)}$

  - Example: Medical Test:
    - $P(\text{disease} \mid \text{positive test}) = \frac{P(\text{positive test} \mid \text{disease}) \, P(\text{disease})} {P(\text{positive test})} $


  



## 5. Estimation Methods
The goal of estimation is to **infer unknown parameters** $\theta$ from observed data.

- #### Types of Estimation
  - **Point Estimation**: Provides a single best guess of the parameter.  
  - **Interval Estimation**: Provides a range of plausible values (e.g., confidence intervals, Bayesian credible intervals).  


- #### Maximum Likelihood Estimation (MLE)

  - **Definition:**

$$
\hat{\theta}_{\text{MAP}}
= \arg\max_{\theta} P(\theta \mid \text{data})
= \arg\max_{\theta} \big[ P(\text{data} \mid \theta)\, P(\theta) \big].
$$

   - **Interpretation:**  
MLE chooses $\theta$ that makes the observed data most "likely."
   - **Log-likelihood:**
     
$$
   \ell(\theta)
   = \log L(\theta)
   = \sum_i \Big[ x_i \log \theta + (1-x_i)\log(1-\theta) \Big].
   </d-math>
$$

   - **Example (Bernoulli):**  
Suppose we observe $k$ successes in $n$ Bernoulli trials. Then

$$
   \hat{\theta}_{\text{MLE}} = \frac{k}{n}.
$$

  - **Notes:**
     - MLE does not always exist.  
     - MLE may not be unique.  
     - MLE may not always be admissible.  



- #### Maximum A Posteriori (MAP)

  - **Definition:**

$$
  \hat{\theta}{\text{MAP}}
  = \arg\max{\theta} P(\theta \mid \text{data})
  = \arg\max_{\theta} P(\text{data} \mid \theta),P(\theta).
$$

  - MAP incorporates a **prior distribution** $P(\theta)$.  
  - MLE ignores the prior.  


- #### Regularization as MAP
  - Adding a regularization term to MLE is equivalent to doing MAP estimation with a prior:
    - **L2 penalty (ridge regression):** corresponds to a Gaussian prior on parameters.  
    - **L1 penalty (lasso regression):** corresponds to a Laplace prior on parameters.  

Formally:

$$
  \hat{\theta}_{\text{reg}}
  = \arg\max_{\theta} \Big[ \log L(\theta) - \lambda R(\theta) \Big]
  \quad\Longleftrightarrow\quad
  \hat{\theta}_{\text{MAP}}
$$



## 6. Linear Regression
Linear regression models the relationship between inputs (features) and outputs (responses).

- #### Model Definition
$$
  y = X\beta + \epsilon
$$

   - $y$: response variable (dependent variable).  
   - $X$: design matrix (independent variables or features).  
   - $\beta$: coefficients (parameters to estimate).  
   - $\epsilon$: error term, often assumed $\epsilon \sim N(0, \sigma^2)$.

   - **Goal:** Estimate $\beta$.


- #### Evaluation Metrics
   - **Coefficient of Determination ($R^2$):**

$$
  R^2 = 1 - \frac{SS_{\text{res}}}{SS_{\text{tot}}}
$$

  - Measures the proportion of variance in $y$ explained by the model.

   - **Mean Squared Error (MSE):**

$$
  MSE = \frac{1}{n} \sum_i (y_i - \hat{y}_i)^2
$$
  
   - **Mean Absolute Error (MAE):**

$$
  MAE = \frac{1}{n} \sum_i |y_i - \hat{y}_i|
$$


- #### Ordinary Least Squares (OLS)

   - **Objective:**

$$
  \hat{\beta}_{\text{OLS}}
  = \arg\min_{\beta} \|y - X\beta\|^2
$$

   - **Residuals:** $e_i = y_i - \hat{y}_i$.  

   - **Closed-form solution:**

$$
  \hat{\beta}_{\text{OLS}} = (X^TX)^{-1}X^Ty
$$


- #### Regularization in Linear Regression

   - **Ridge Regression (L2):**

$$
  \hat{\beta}_{\text{ridge}}
  = \arg\min_{\beta} \|y - X\beta\|^2 + \lambda \|\beta\|_2^2
$$
  
  Equivalent MAP interpretation: Gaussian prior $\beta \sim N(0,  \frac{\sigma^2}{\lambda})$.


   - **Lasso Regression (L1):**

$$
   \hat{\beta}_{\text{lasso}}
   = \arg\min_{\beta} \|y - X\beta\|^2 + \lambda \|\beta\|_1
$$

  Equivalent MAP interpretation: Laplace prior $\beta \sim \text{Laplace}(0, \frac{\sigma}{\lambda})$.  
  Encourages **sparsity** (many coefficients shrink to 0).
