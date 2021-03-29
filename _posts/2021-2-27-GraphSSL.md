---
layout: post
title: Review of Self-supervised learning of GNN
subtitle: unsupervised graph representation learning
cover-img: /assets/img/Review_SSL_GNN/.png
thumbnail-img: /assets/img/Review_SSL_GNN_model.png
share-img: /assets/img/Review_SSL_GNN_model.png
tags: [deep learning, graph representation, unsupervised learning]
---

## 分类

自监督的方法可以大致分为两类：1.contrastive method 2. perdictive method

![comparison](../assets/img/Review_SSL_GNN/comparison.png)

### 1. contrastive method

需要data-data pairs用作training
困难在于如何获得好的views，如何选择graph encoder
丛互信息的视角去统一contrastive objectives，从这个视角来看，不同的CL方法被认为执行三种不同的transformations以获得views

#### contrastive objectives

1. Mutual Information Estimation
假设我们存在一对随机变量$(\boldsymbol{x}, \boldsymbol{y})$,他们之间的互信息计算如下:
$$
\begin{aligned}
\mathcal{I}(\boldsymbol{x}, \boldsymbol{y}) &=D\_{K L}(p(\boldsymbol{x}, \boldsymbol{y}) \| p(\boldsymbol{x}) p(\boldsymbol{y})) \\
&=\mathbb{E}\_{p(\boldsymbol{x}, \boldsymbol{y})}\left[\log \frac{p(\boldsymbol{x}, \boldsymbol{y})}{p(\boldsymbol{x}) p(\boldsymbol{y})}\right]
\end{aligned}
$$
contrastive learning的目标是最大化两个view（视作两个随机变量）的互信息，正样本对的presentation来自联合分布$p\left(\boldsymbol{v}\_{i}, \boldsymbol{v}\_{j}\right)$，负样本对的representation来自边缘分布的乘积$p\left(\boldsymbol{v}\_{i}\right) p\left(\boldsymbol{v}\_{j}\right)$。

有三种典型的互信息的下界：Donsker-Varadhan representation $\widehat{\mathcal{I}}^{(D V)}$，Jensen-Shannon estimator $\widehat{\mathcal{I}}^{(J S)}$，noise-contrastive estimation $\widehat{\mathcal{I}}^{(N C E)}$。$\widehat{\mathcal{I}}^{(J S)}$和$\widehat{\mathcal{I}}^{(N C E)}$通常是用作目标函数。
互信息的估计一般是通过discriminator$\mathcal{D}: \mathbb{R}^{q} \times \mathbb{R}^{q} \rightarrow \mathbb{R}$计算得到，通过计算两个view的representation的agreement score，discriminator可以是带参的也可以是无参的。discriminator还可以使用projection heads对view represnetations进行映射变换$\boldsymbol{z}\_{i}=g\_{i}\left(\boldsymbol{h}\_{i}\right), i=1, \cdots, k$，然后再计算pairs的similarity score。

1.1 Donsker-Varadhan Estimator
DV estimator通常也叫KL散度的DV representation。
给定$\boldsymbol{h}\_{i}$ and $\boldsymbol{h}\_{j}$，他们互信息的下界：
$$
\begin{aligned}
\widehat{\mathcal{I}}^{(D V)}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}\right)=\mathbb{E}\_{p\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}\right)} &\left[\mathcal{D}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}\right)\right] \\
-& \log \mathbb{E}\_{p\left(\boldsymbol{h}\_{i}\right) p\left(\boldsymbol{h}\_{j}\right)}\left[e^{\mathcal{D}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}\right)}\right]
\end{aligned}
$$
该公式可以重写为：
$$
\begin{aligned}
\widehat{\mathcal{I}}^{(D V)}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}\right) &=\mathbb{E}\_{(\boldsymbol{A}, \boldsymbol{X}) \sim \mathcal{P}}\left[\mathcal{D}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}\right)\right] \\
-& \log \mathbb{E}\_{\left[(\boldsymbol{A}, \boldsymbol{X}),\left(\boldsymbol{A}^{\prime}, \boldsymbol{X}^{\prime}\right)\right] \sim \mathcal{P} \times \mathcal{P}}\left[e^{\mathcal{D}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}^{\prime}\right)}\right]
\end{aligned}
$$

1.2 Jensen-Shannon Estimator
相比于DV estimator，JS estimator通过计算联合分布和边缘分布之积的JS散度可以更有效地估计和优化互信息
$$
\begin{array}{c}
\widehat{\mathcal{I}}^{(J S)}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}\right)=\mathbb{E}\_{(\boldsymbol{A}, \boldsymbol{X}) \sim \mathcal{P}}\left[\log \left(\mathcal{D}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}\right)\right)\right]+ \\
\mathbb{E}\_{\left[(\boldsymbol{A}, \boldsymbol{X}),\left(\boldsymbol{A}^{\prime}, \boldsymbol{X}^{\prime}\right)\right] \sim \mathcal{P} \times \mathcal{P}}\left[\log \left(1-\mathcal{D}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}^{\prime}\right)\right)\right]
\end{array}
$$

1.3 InfoNCE
InfoNCE是另一种测量互信息的下界的方法，假设$\boldsymbol{h}\_{i} \text { and } \boldsymbol{h}\_{i}$是随机变量$(\boldsymbol{A}, \boldsymbol{X})$的两个view，并存在N个negative samples，InfoNCE的公式为：
![InfoNCE](https://cdn.mathpix.com/snip/images/IUNJZLySuknJ-DIk7JfcAtyQN1pX6umTPKFnNxG_10c.original.fullsize.png)
$\boldsymbol{K}$是有N个随机变量组成，这些随机变量独立采样于graph data distribution$\mathcal{P}$。$h\_i$和$h\_j$是$(\boldsymbol{A}, \boldsymbol{X})$这个样本的i-th和j-th个view的representation，$h\_j^{'}$是$(\boldsymbol{A^{'}}, \boldsymbol{X^{'}})$样本的j-th个view的representation。

Intuitively, the optimization of InfoNCE loss aims to score the agreement between $h\_i$ and $h\_j$ of views from the same
instance $x$ higher than between $h\_i$ and $h\_j^{'}$ from the rest N negative samples B \ {x}.

2. Non-Bound Mutual Information Estimators
还存在一些目标函数，它们能够增加互信息，但是它们没有被有力地证实为是互信息的下界，甚至有些方法没有被证实为在最大化互信息。
举个例子，Jiao et al[]提出了最小化triplet margin loss：
$$
\begin{aligned}
\mathcal{L}\_{\text {triplet }}=\mathbb{E}\_{\left[(\boldsymbol{A}, \boldsymbol{X}),\left(\boldsymbol{A}^{\prime}, \boldsymbol{X}^{\prime}\right)\right] \sim \mathcal{P} \times \mathcal{P}} &\left[\max \left\{\mathcal{D}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}\right)\right.\right.\\
&\left.\left.-\mathcal{D}\left(\boldsymbol{h}\_{i}, \boldsymbol{h}\_{j}^{\prime}\right)+\epsilon, 0\right\}\right]
\end{aligned}
$$

3. Projection Head: Parametric Estimation
The projection head为证实能显著地增强CL的性能，我们认为包含projection head的MI estimator是parametric estimator，而不包含projection head的estimator是non-parametric estimator，为什么使用projection head的效果会更好，因为parametric estimator能得到更好的MI估计。

4. Comparison and Discussion of Different Objectives
Hjelm et al[]通过实验证明InfoNCE在大多数情况下性能会优于JS estimator，但是InfoNCE estimator要求有大量的负样本，否则它的效果就不太好，如果batch size比较小的化，JS estimator会是一个更好的选择。

#### Graph View Generation
可以大致分为feature transformation，structure transformation，sampling-based transformation。

1. Feature Transformation
可以公式化为:
$$
\mathcal{T}\_{\text {feat }}(\boldsymbol{A}, \boldsymbol{X})=\left(\boldsymbol{A}, \mathcal{T}\_{X}(\boldsymbol{X})\right)
$$
, where $\mathcal{T}\_{X}: \mathbb{R}^{|V| \times d} \rightarrow \mathbb{R}^{|V| \times d}$
目前最常见的是node attribute masking，所有节点的部分attribute会被随机mask掉，公式化为:
$$
\mathcal{T}\_{X}^{(m a s k)}(\boldsymbol{X})=\boldsymbol{X} \*\left(\mathbf{1}-\mathbf{1}\_{m}\right)+\boldsymbol{M} \* \mathbf{1}\_{m}
$$, 其中$\boldsymbol{M}$是噪声矩阵，$\mathbf{1}\_{m}$是masking location indicator matrix。attribute masking不仅只用在contrastive method中，也用在predictive method中。节点属性掩蔽迫使编码器捕获掩蔽属性和未掩蔽上下文属性之间更好的依赖关系，并在编码期间从其上下文恢复掩蔽值。

2. Structure Transformation
可以公式化为:
$$
\mathcal{T}\_{\text {struct }}(\boldsymbol{A}, \boldsymbol{X})=\left(\mathcal{T}\_{A}(\boldsymbol{A}), \boldsymbol{X}\right)
$$
, where $\mathcal{T}\_{A}: \mathbb{R}^{|V| \times|V|} \rightarrow \mathbb{R}|V| \times|V|$
结构的变化主要是对邻接矩阵进行变化，1.在节点对之间随机添加或删除边的边扰动和2.基于从一个节点到另一个节点的可访问性创建新边的图扩散。
**Edge perturbation**:
$$
\mathcal{T}_{A}^{(p e r t)}(\boldsymbol{A})=\boldsymbol{A} *\left(\mathbf{1}-\mathbf{1}_{p}\right)+(\mathbf{1}-\boldsymbol{A}) * \mathbf{1}_{p}
$$
**Diffusion**:
基于random walk构建节点之间新的链接，旨在产生graph的全局的view $(\boldsymbol{S}, \boldsymbol{X})$以对比局部的view $(\boldsymbol{A}, \boldsymbol{X})$
Graph diffusion的两种具体的方法有heat kernel $\mathcal{T}\_{A}^{\text {(heat) }}$和Personalized PageRank $\mathcal{T}\_{A}^{\text {(PPR) }}$
$$
\begin{array}{c}
\mathcal{T}_{A}^{(h e a t)}(\boldsymbol{A})=\exp \left(t \boldsymbol{A} \boldsymbol{D}^{-1}-t\right) \\
\mathcal{T}_{A}^{(P P R)}(\boldsymbol{A})=\alpha\left(\boldsymbol{I}_{n}-(1-\alpha) \boldsymbol{D}^{-1 / 2} \boldsymbol{A} \boldsymbol{D}^{-1 / 2}\right)^{-1}
\end{array}
$$
α denotes the teleport probability in a random walk and t denotes the diffusion time.

3. Sampling-based transformation
可以公式化为:
$$
\mathcal{T}\_{\text {sample }}(\boldsymbol{A}, \boldsymbol{X})=(\boldsymbol{A}[S ; S], \boldsymbol{X}[S])
$$
, where $S \subseteq V$
获得node subset的方式主要有三种：1. uniform sampling 2. random walk sampling 3. ego-nets sampling
前两种方法都容易理解，ego-nets sampling是针对每个节点的，采样出每个节点的L-hop的所有邻居节点。以及这些节点构成的邻接矩阵
$$
\begin{array}{c}
\boldsymbol{w}\_{i}=\mathcal{T}\_{i}(\boldsymbol{A}, \boldsymbol{X})=\left(\boldsymbol{A}\left[\mathcal{N}\_{L}\left(v\_{i}\right) ; \mathcal{N}\_{L}\left(v\_{i}\right)\right], \boldsymbol{X}\left[\mathcal{N}\_{L}\left(v\_{i}\right)\right]\right) \\
\mathcal{N}\_{L}\left(v\_{i}\right)=\left\{v: d\left(v, v\_{i}\right) \leq L\right\}
\end{array}
$$
, L代表node-level encoder的层数


### 2. predictive method

需要data-label pairs用作training，这些labels是self-generated，也就是伪标签，predictive method实际上也是使用有监督的方式进行学习的，但是标签是不需要人工标注的，由数据自己产生。而contrastive method是判断正样本对和负样本对进行训练的。
predictive model通常是由一个encoder以及prediction head组成。当预训练结束后，我们只使用encoder部分。
困难在于如何选择labels，什么样的labels更有助于捕获node attributes和graph structures的信息的任务，

### 展望

在图上做unsupervised domain adaptation，encoder先在有监督的数据集上training，然后再在unlabeled dataset上self-training。

### 术语

auxiliary learning（joint learning）:
aims to improve the performance of a supervised primary learning task by including an auxiliary task under self- supervision.

