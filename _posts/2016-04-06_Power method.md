---
layout: post
title: Power methods
date: 2016-04-06
categories: blog
tags: [Notes]
description: Eigenvalue problems
---


## Power method

The eigenvalues of an $n \times n$ matrix A are obtained by solving its characteristic equation

$$ \lambda^{n} + c_{n_1} \lambda^{n-1} + c_{n_2} \lambda^{n-2} + ...+ c_0 = 0 $$

**Dominant Eigenvalue and Dominant Eigenvector**

Let $\lambda_{1}, \lambda_{2}, ....,\lambda_{n}$ be the eigenvalues of an $n \times n$ matrix, $\lambda_{1}$ is called the **Dominant Eigenvalue** of A if
$$\lambda_{1} > \lambda_{i}, \quad i =2,...n$$

**Initial** approximation: choose $x_0$, given the form by iteration :
$$x_1 = A x_0$$
$$x_2 = A^2 x_0$$

$$x_k = A^k x_0$$

**Rayleigh quotient**
If x is an eigenvector of a matrix A, then its correponding eigenvalue of given by 

$$ \lambda = \frac{Ax\cdot x}{x \cdot x} $$

**Convergence of Power Method**
If A is an $n \times n$ diagonalizable matrix with a dominant eigenvalue, then there exists a nonzero vector $x_0$ such that the sequence of vectors given by 

$$ A x_0,\quad A^2 x_0,\quad A^3 x_0,\quad....,A^k x_0 $$ 

## QR methods

### Similar Matrices

A and B are **similar** matrices if and ony if there exists a nonsingular matrix so that
$$B = P^{-1} A P $$

**similar** matrices have same sets of eigenvalues

### QR Algorithm
Iteration:
Given $A_k$

Factor: $A_k = Q_k R_k$ and then  $A_{k+1} = R_k Q_k $
