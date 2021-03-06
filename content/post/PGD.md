---
title: 'Towards Deep Learning Models Resistant to Adversarial Attacks'
author: Bolor-Erdene Zolbayar
date: '2019-01-14'
slug:
categories: ["adversarial machine learning", "research"]
tags: ["review"]
math: true
header:
  caption: ''
  image: ''
---

The goal of this paper is to train a machine learning model such that the ML system becomes resistance to adversarial examples. It is a well known fact that neural networks are vulnerable to adversarial examples. Some of the recent works explain that vulnerability of the neural networks is intrinsic. Madry et al. 2017 at MIT investigated how to create robust neural networks with robust optimization. As a result, they found a way to create successful attack and defense techniques based on robust optimization. In this article, I will explain why they used robust optimization, how it worked, and what important techniques they used to create the robust neural networks.

## Table of Contents

1. [Why do they want to do robust optimization?](#why)
2. [How were they able to achieve training that resulted high resistance to adversarial examples?](#how)
3. [What are the some math behind this magic?](#what)

## ***Why do they want to do robust optimization?*** <a id="why"></a>

1. Growth in Machine Learning
Application of machine learning has been exponentially increasing in recent years following the development in high computational power and data availability. Recent breakthroughs in computer vision and speech recognition enable us to ride on a self-driving car, make a restaurant or haircut order without touching phone screen, and  search what you need on youtube or google simply telling a few words. A new research study ["Machine Learning Market"](https://www.marketwatch.com/press-release/global-machine-learning-market-2018-expected-to-reach-3998-billion-by-2025-and-research-analysis-done-by-technologies-types-2018-08-20) states that the machine learning market is expected to grow from 1.3 billion USD in 2016 to 40 billion USD in 2025. For these reasons, security of machine learning has become one of the most important fields that needs to be studied.

2. Robust neural networks
Adversaries often create adversarial exmaples by modifying features of training examples that are close to decision boundary. To create a robust neural networks resistant to these adversarial examples, one technique is to create adversarial examples using training examples close to decision boundary and train the neural networks with them. As a result, the neural networks become more robust.

3. Why adversarial robust through robust optimization?
Previously, there were many defense techniques including defensive distillation, feature squeezing, and several others. They claim that these techniques don't offer a good understanding of the guarantees they provided. Their a natural saddle point formulation technique guarantees security to broad range of attacks. They were able to create attack and defense mechanisms with this technique. The adversarial training directly relates with optimizing the saddle point problem.  


## ***How were they able to achieve training that resulted high resistance to adversarial examples?*** <a id="how"></a>

They make the following contributions:

1. Optimization landscape study corresponding to saddle point formulation. Despite non-convexity and non-cavity of its constituent parts, they were able to track and solve the optimization problem using first-order methods. They created a projected gradient attack with the optimization technique using the first-order methods.

2. They found the model architecture capacity on adversarial robustness is important. To reliably withstand strong adversarial attacks, networks
require a significantly larger capacity than for correctly classifying benign examples only. This shows that a robust decision boundary of the saddle point problem can be significantly more complicated than a decision boundary that simply separates the benign data points.

3. Building on the above insights, we train networks on MNIST and CIFAR10 that are robust to
a wide range of adversarial attacks. Our approach is based on optimizing the aforementioned
saddle point formulation and uses our optimal “first-order adversary”. Our best MNIST model
achieves an accuracy of more than 89% against the strongest adversaries in our test suite. In
particular, our MNIST network is even robust against white box attacks of an iterative adversary.
Our CIFAR10 model achieves an accuracy of 46% against the same adversary. Furthermore,
in case of the weaker black box/transfer attacks, our MNIST and CIFAR10 networks achieve
the accuracy of more than 95% and 64%, respectively.

## ***What are the some math behind this magic?*** <a id="what"></a>

$ \theta \in \mathbb{R}^{d}$; D - training data distribution; $x \in \mathbb{R}^{d}$ training examples; $y \in [k]$ labels for corresponding
examples; $L(\theta,x,y)$ - loss function

### *The goal is to minimize the risk: $E_{(x,y)} \sim D[L(x,y,\theta)]$*
This ERM is great for classifiers. But, it doesn't provide resistance to adversarial examples

### *How to make it robust?*
To make the model resistant, they augmented the ERM by following steps.

1. They specify the attack model and make the classifier more robust to this specific attack.
2. For each training example x, they introduce set of perturbations $S \in \mathbb{R}^{d}$ that is  represented by the manipulative power of adversary
3. They modified the population risk $E\_{d}[L]$ instead of feeding loss L with samples from original D distribution, they perturb the inputs. In this paper, they only focused on $l\_{\infty}$ bounded attacks.

They introduced the saddle point optimization problem. Inside is a maximization and outside is a minimization problem.

### *Challenges and solutions to them*
- The goal is to obtain a solution for $\mathop{min}\_{\theta \in \mathbb{R}^{d}}E\_{(x,y) \sim D} [\mathop{max}\_{\delta \in S} L(\theta,x+\delta,y)]$

- The inner part finding adversarial example is equivalent as maximizing highly non-concave function. Tool to solve this problem is PGD. PGD is a standard method for large-scale constrained optimization.

- Important challenge is to obtain a good solution in a reasonable time.

- Solving this problem requires non-convex outer minimization and non-concave inner maximization.

- Their important contribution is to use PGD technique that solves the saddle point. They argue that loss landscape of the optimization problem is surprisingly tractable. The problem points towards projected gradient descent as the ultimate first order adversary.

- Finding significantly low loss function after solving the optimization problem guarantees the classifier is resistant to adversarial examples.

- They argue that the minimzation of the outer function in terms of \theta parameters is equivalent as solving the problem with stochastic gradient descent using the gradients solved by the inner maximization problem.

- Basically, they stated that if the network is trained to be robust against PGD adversaries, it will be robust against a wide range of attacks that encompasses all current approaches.

### *They found following phenomena during their experiments*

- the loss achieved by the adversary increases in a fairly consistent way and plateaus rapidly when performing projected $l\_{\infty}$ gradient descent for randomly chosen starting points inside $x + S$

- Investigating the concentration of maxima further, they observed that over a large number of random restarts, the loss of the final iterate follows a well-concentrated distribution without extreme outliers.

- By applying SGD using the gradient descent of the loss at adversarial examples they can consistently reduced the loss of the saddle point problem during training.
