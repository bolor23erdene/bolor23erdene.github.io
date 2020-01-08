---
title: 'Support Vector Machine and Karush-Kuhn-Tucker'
author: Bolor-Erdene Zolbayar
date: '2019-01-15'
slug:
categories: ["optimization", "coursework"]
tags: ["LSML"]
math: true
header:
  caption: ''
  image: ''
---

This article is a brief introduction about SVM and KKT.

## Table of Contents

1. [SVM](#svm)
2. [KKT](#kkt)

## ***Support Vector Machine*** <a id="svm"></a>

Support Vector Machine is one of the most influential techniques in supervised machine learning (Boser et al. 1992; Cortes and Vapnik et al. 1995). It is very similar to logistic regression. The main difference is it does not output probability, but, a class identity instead. SVM predicts a positive class is present when $w^Tx+b$ is positive. Similarly, SVM predicts a negative class is present when $w^Tx+b$ is negative.

The key innovation with SVM is kernel trick. Kernel trick is a technique that many ML algorithms can be written in terms of dot products between examples. In fact, one does not need to map the training examples for optimization. Kernel trick is powerful for 2 reasons.
> It enables to learn nonlinear models as a function of x using convex optimization techniques that provides efficient convergence.

> Kernel function k can be implemented computationally efficient instead of naively creating $\phi(x)$ vectors and explicitly computing their dot product.

The most commonly used kernel is the Gaussian kernel. The Gaussian kernel is similar to template matching. A training example x with training label y becomes a template for class y. If a test point x' is near x by Euclidean distance, the Gaussian kernel has a large response, which indicates that x' is very similar to x template. The model assigns a large weight on the associated training label y. Overall, the prediction will combine many such training labels weighted by the similarity of the corresponding training examples.

## ***Karush-Kuhn-Tucker*** <a id="kkt"></a>
Sometimes we need not only to minimize or maximize a function $f(x)$ over all possible values of x but also to find max or min value of $f(x)$ for value of x in some set $\mathbb{S}$. There are two possible ways to solve this problem.

- The simple way is to make gradient descent steps and project the result back into $\mathbb{S}$.

- The sophisticated way is to design a different, unconstrained optimization problem whose solution can be converted into solution of our constrained optimization problem.  

KKT provides a very general solution to constrained optimization. With KKT, we introduce a new function called generalized Lagrangian function.

- We describe $\mathbb{S}$ in terms of equations and inequalities.

- We want a description of $\mathbb{S}$ in terms of m functions $g(i)$ and n functions $h(j)$.

- $g(i)$ are equality constraints and $h(j)$ are inequality constraints.

- We introduce KKT multipliers $\lambda\_{i}$ and $\alpha\_{i}$ for each constraint.

The generalized Lagrangian is:
$L(x,\lambda,\alpha) = f(x) + \sum\limits\_{i} \lambda\_{i}g^{(i)}(x) + \sum\limits\_{j}\alpha\_{j}h^{(j)}(x)$

## From Convex Optimization book slides:


![This is an image](/static/KKT/KKT1.png)
![This is an image](/static/KKT/KKT2.png)

