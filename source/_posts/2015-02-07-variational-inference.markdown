---
layout: post
title: "[译]Variational Inference"
date: 2015-02-07 23:32
comments: true
categories: [variational inference, mean field theory, KL divergency]
---

本文翻译自David Blei的[课程讲义](http://www.cs.princeton.edu/courses/archive/fall11/cos597C/lectures/variational-inference-i.pdf)。

## 1 设定
* 我们假设观测值为$$x=x_{1:n}$$，隐变量为$$z=z_{1:m}$$。另外，假设其他参数$$\alpha$$为固定值。
* 我们的讨论适用于一般情况——在一个典型的推断问题中，隐变量中可能包含了模型参数。（在这种情况下，$\alpha$为超参数）
* 我们感兴趣的是*后验分布*:

$$
p(z \ | \ x, \alpha)=\frac{p(z, x \ \mid \ \alpha)}{\int_z p(z,x \ \vert \ \alpha)}
$$

* 后验分布结合了数据和模型。它将用在所有的下游分析中，例如predictive distribution。
* （注意：计算后验只是变分推断可以解决的一般性问题中的一种特例。）

## 2 动机
* 对于很多有意思的模型，我们无法计算后验。
* 考虑贝叶斯混合高斯分布，
    1. 对于$$k=1...K$$，采样$$\mu_k \~ \mathcal{N}(0, \tau^2)$$。
