---
title: 'What is GANs?'
author: Bolor-Erdene Zolbayar
date: '2019-01-12'
slug:
categories: ["adversarial machine learning", "research"]
tags: ["GANs"]
math: true
header:
  caption: ''
  image: ''
---

The goal of one of my research is to generate adversarial network traffic that fools the network detection system. I used deep generative model GANs to create the adversarial samples. GANs are known to be best for learning finite amount of training data, interpolating the training data's distribution, and creating samples that are indistinguishable from the original training data. Basically, with GANs, I was able to generate huge amount adversarial traffic by keeping the features that are intrinsic to the attacks and changing the unimportant features so that the traffic bypasses the detection system. In this article, I will explain why you need GANs, how it works, and what challenges you will encounter applying GANs.

## Table of Contents

1. [What is generative model?](#generative-model)
2. [Why do you need to study GANs?](#why)
3. [How does GANs work?](#how)
4. [What are the main challenges of GANs?](#what)
5. [Wasserstain GANs](#paper1)
6. [Improved techniques for training GANs](#paper2)

## ***What is generative model?*** <a id="generative-model"></a>

Some generative models perform density estimation. The model learns the finite number of samples from the given training data distribution $P\_{data}$ and returns an estimate of that distribution $P\_{model}$. The end goal is to represent the $P\_{data}$ distribution as $P\_{model}$ probability distribution.

## ***Why do you need to study GANs?*** <a id="why"></a>

1. *Study high-dimensional probability*:
Training and generating from generative model is an excellent way to test our ability to manipulate high-dimensional probability distributions. High-dimensional probability distributions are important in applied math and engineering domains.

2. *Reinforcement learning*:
Reinforcement learning algorithms are divided into model-based and model-free categories. Model-based algorithms can be incorporated with generative models. Generative models of time-series are able to simulate possible futures for the problem. A generative model can learn a conditional probability distributions over a future states of the world, given with current state and actions.

3. *Generating data*:
Generative models can be trained with missing data and can provide prediction on inputs that are missing data. The main idea is to add one more class corresponding to the fake images to the original n classes and train the model. The real-and-fake model also can be trained with known real unlabeled dataset and generated fake dataset.


## ***How does GANs work?*** <a id="how"></a>

The GANs model consists of generator and discriminator models. Generator and discriminator can be seen as counterfeiter and policeman, respectively. The counterfeiter generates fake currency and inject it to market while the policeman evaluates the currency and tells if the currency is real or fake. Due to competition between counterfeiter and policeman, their craftsmanship and evaluation get better and better. At the end, the counterfeiter learns to generate fake currency that is indistinguishable from the real currency.

## ***What are the main challenges of GANs?*** <a id="what"></a>

1. *Mode Collapse*: Mode collapse occurs when the generator is generating samples from only specific type of class. According to Goodfellow et al., mode collapse does not seem to be caused by any particular cost function. In some cases, the Jensen-Shannon divergence caused the mode collapse, however, this does not seem to be the case, because GANs that minimize approximations of $D\_{KL}(P\_{data}||P\_{model})$ face the same same issues, and because the generator often collapses to even fewer modes than would be preferred by the Jensen-Shannon divergence. The mode collapse might be the most important problem with GANs.

2. *Non-convergence*: The GANs optimize two deep models: Generator and Discriminator. Optimizing interrelated two different losses due to generator and discriminator is much more complicated than optimizing loss of conventional deep neural network models. In some cases, although each player might go downhill at each update, it might be the case that one player might make the other one to go uphill on its turn. Eventually, this leads to non-convergence.

In the minimax game of GANs, Goodfellow et al. (2014b) showed that the simultaneous gradient descent converges if the updates are made in function space. However, the updates are made in parameter space, so the convexity property needed for the proof doesn't apply. There has not been any theoretical proof that the GAN games should converge or not converge came out yet.

To solve these problems, certain methods are introduced such as Wasserstain GAN optimization.

## ***Wasserstain GANS*** <a id="paper1"></a>

Learning probability distribution means learning a probability density, which is done by defining a parametric family that maximizes the likelihood on our real data examples $\{x^{(i)}\}_{i=1}^m$ shown below


$\mathop{max}\_{\theta \in R^{d}}\frac{1}{m} \sum\_{i=1}^{m} log P\_{\theta} (x^{(i)})$


The WGAN paper studied various ways to measure how close the model and real distributions are meaning that various ways to define a distance of divergence $\rho(P\_{\theta},P\_{r})$.

The most fundamental difference between such distance is their impact on the convergence of sequences of probability distributions. To optimize the parameter $\theta$, they define model distribution $P_{\theta}$ in a way that results the mapping $\theta \rightarrow P\_{\theta}$ continuous.

In short contributions of the paper:

Theoretical Analysis

They provide a comprehensive theoretical analysis of how the Earth Mover (EM) distance behaves in comparison to popular probability distances and divergences used in the context of learning distributions.

- *A form of GAN called WGAN*: They define the WGAN that minimizes the reasonable and efficient approximation of the EM distance, and they theoretically show that the corresponding problem is sound.

- *WGANs' advantages*: WGANs does not require maintaining a careful balance in training of the discriminator and the generator, and does not require a careful design of the network architecture either. The mode dropping phenomenon that is typical in GANs is also drastically reduced. The most compelling benefit is the ability to continuously estimate the EM distance by training the discriminator to optimality. Plotting these learning curves is not only useful for debugging and hyperparameter searches, but also correlate remarkably well with the observed same quality.



If the real data distribution $P^{r}$ has a density and $P\_{\theta}$ is the distribution of the density defined by the parametric family, then, the problem converts into minimizing the Kullback-Leibler divergence $KL(P\_{r}||P\_{\theta})$. For this new problem, we must have the density $P\_{\theta}$ to exist. However, in practice, it is common that we deal with distributions supported by low dimensional manifolds, which leads to negligible intersection between true and model distribution resulting infinite KL divergence. In this case, adding noise to the model distribution fixes the problem, which explains why generative models include a noise component. However, this noise reduces the quality of the samples and makes them poor or blurry. In short, adding noise term is wrong, but is needed to make the maximum likelihood approach work.

To solve this problem, without estimating the density $P\_{r}$ which may not even exist, we can create a random variable $Z$ with a fixed distribution p(z) and pass that through a parametric function $g\_{\theta}:Z \rightarrow X$ (a neural network in our case)
 that directly generates samples following a certain distribution $P\_{\theta}$. By changing $\theta$, we can change this distribution and make it close to the real data distribution $P\_{r}$.

 This is useful for two reasons. First, unlike densitites, this approach can represent distributions confined to a low dimensional manifold. Second, the ability to easily generate samples is often more useful than knowing the numerical value of the density (for example in image superresolution or semantic segmentation when considering the conditional distribution of the output image given the input image). In general, it is computationally difficult to generate samples given an arbitrary high dimensional density.

 Variational Auto-Encoders (VAEs) and GANs are well known examples of this approach. VAE focus on the approximate likelihood of the examples, they share the limitations of the standard models and need to fiddle with additional noise terms. GANs offer much more flexibility in the definition of the objective function, including Jensen-Shannon, and all f-divergence as well as some exotic combinations. On the other hand, training GANs is well known for being delicate and unstable, for reasons theoretically investigated in.
