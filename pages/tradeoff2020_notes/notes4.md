---
layout: post
---

## [Learning Representations from Imperfect Time Series Data via Tensor Rank Regularization](https://arxiv.org/abs/1907.01011)

#### Terminologies

- **Canonical Polyadic (CP)-decomposition**: for an order-$M$ tensor $\mathcal{X}\in\mathbb{R}^{d_1\times\dots\times d_M}$, there exists an exact decomposition into vectors $\mathbf{w}$:

  
  $$
  \mathcal{X}=\sum_{i=1}^r\bigotimes_{m=1}^M\mathbf{w}_m^i
  $$
  

  The minimal $r$ for exact decomposition is the **rank** of the tensor, which measures how many vectors are required to reconstruct the tensor. The vectors $\\{\\{\mathbf{w}\_m^i\\}\_{m=1}^M\\}_{i=1}^r$ are called the **rank $r$ decomposition factors** of $\mathcal{X}$.

- **Nuclear norm** of an order-$M$ tensor is defined as

  
  $$
  \|\mathcal{X}\|_*=\inf\left\{\sum_{i=1}^r|\lambda_i|:\mathcal{X}=\sum_{i=1}^r\lambda_i\left(\bigotimes_{m=1}^M\mathbf{w}_m^i\right),\|\mathbf{w}_m^i\|=1,r\in\mathbb{N}\right\}
  $$
  

  The tensor **Frobenius norm scaled by the tensor dimensions** upper bounds the nuclear norm and can be efficiently computed, defined as

  
  $$
  \|\mathcal{M}\|_*\le\sqrt{\frac{\prod_{i=1}^Md_i}{\max\{d_1,\dots,d_M\}}}\|\mathcal{M}\|_F
  $$

#### Challenge/Opportunity

- Natural multimodal data are often imperfect (incomplete due to mismatched modalities or sensor failure, or corrupted with random/structured noise), which breaks the temporal correlations and gives tensor representations of higher rank.
- Computing the rank of a tensor or its nuclear norm is $\mathsf{NP}$-hard for tensors of order$\ge 3$.

#### SOA Knowledge/Solution

- Recent research investigated the use of tensors for representation learning: given representations $\mathbf{h}_1,\dots,\mathbf{h}_M$ from $M$ modalities, the order-$M$ outer product tensor $\mathcal{T}=\mathbf{h}_1\otimes\mathbf{h}_2\otimes\dots\otimes\mathbf{h}_M$ represents all possible interactions between the modality dimensions.
- Generative approaches as neural models (cascaded residual autoencoders), deep adversarial learning, and translation-based learning have been proposed to account for imperfect data, but these methods often require prior knowledge of which entries or modalities are imperfect.

#### Approach (T2FN)

- Given time series data from the language, visual, and acoustic modalities, **Temporal Tensor Fusion Network (T2FN)** uses LSTM networks to encode the temporal information from each modality, which results in a sequence of hidden representations.

- Form tensors via outer products of the individual representation through time and append a 1 to capture unimodal, bimodal, and trimodal interactions

  
  $$
  \mathcal{M}=\sum_{t=1}^T\begin{bmatrix}\mathbf{h}_\ell^t\\1\end{bmatrix}\otimes\begin{bmatrix}\mathbf{h}_v^t\\1\end{bmatrix}\otimes\begin{bmatrix}\mathbf{h}_a^t\\1\end{bmatrix}
  $$

- Since the rank of a tensor or its nuclear norm is $\mathsf{NP}$-hard to compute, minimize the tensor Frobenius norm scaled by the tensor dimensions as the upper bound.

- Study the effect of imperfect data on the rank of tensor $\mathcal{M}$ with clear, random drop, and structured drop noises by performing CP decomposition on the tensor representations under different rank settings $r$ and measure the reconstruction error

  
  $$
  \epsilon=\min_{\mathbf{w}_m^i}\left\|\left(\sum_{i=1}^r\bigotimes_{m=1}^M\mathbf{w}_m^i\right)-\mathcal{X}\right\|_F
  $$
  

  Given the true rank $r^\*$, $\epsilon$ will be high at ranks $r<r^\*$ and approximately zero at ranks $r\ge r^\*$.

#### Results

- The rank of $\mathcal{M}$ is maximized when data entries are sampled from i.i.d. noise; clean real-world data is often generated from lower dimensional latent structures and multimodal time series data exhibits correlations across time and modalities, giving a low-rank tensor representations; $r_\text{clean}<r_\text{imperfect}<r_\text{noisy}$.
- <mark>Imperfection increases tensor rank.</mark>
- <mark>T2FN as a regularization method based on **tensor rank minimization** learns robust representations when imperfections that increase tensor rank are introduced.</mark>

#### Thoughts

- Tensor rank regularization is a method for learning robust model in the setting of imperfect datasets.