---
title: "Few-Shot Learning for Images"
permalink: /projects/fsl/
author_profile: false
---

Few-Shot Learning (FSL) aims at identifying unseen objects using only a few samples with supervised information available. The limited data in novel classes usually follow a biased distribution, which makes it difficult for the model to learn. We proposed **Weighted-distribution Calibration (WC)** to alleviate bias by generating more data from the calibrated distribution of novel classes. We used transferred statistics of all base classes with sufficient data to perform the calibration, weighted by the distance between the novel class and base classes. With a backbone of ResNet-12 and a logistic regression classifier, our method successfully improves the model's performance from a base accuracy of 57.33% to **63.87%**. Our weighted calibration method can be easily combined with any feature extractor and classifier, bringing a boost in Top-1 accuracy in FSL.

We extended the Distribution Calibration proposed by [Yang *et al.*](https://arxiv.org/abs/2101.06395) by computing the similarity scores between the novel class and the base classes measured by the Euclidean distance. For base class $$i$$, we calculate the distance vector $$\boldsymbol{D}_i$$:

$$
\boldsymbol{D}_i=-\|\boldsymbol{\mu}_i-\tilde{\boldsymbol{x}}\|^2,\quad i=1,\cdots,{N_\text{base}}
$$

where $$\tilde{\boldsymbol{x}}$$ is the feature vector of the support set. Then we calibrate the distribution of the novel class via:

$$
\boldsymbol{\mu}'=\frac{\sum_{i=1}^{N_\text{b a s e}}\boldsymbol{\mu}_iS_i+\tilde{\boldsymbol{x}}}{2},
$$

$$
\boldsymbol{\Sigma}'=\sum_{i}\boldsymbol{\Sigma}_iS_i+\alpha,
$$

$$
S_i=\text{softmax}(\boldsymbol{D}_i),\quad i=1,\cdots,{N_\text{base}}
$$

where $$S_i$$ is the normalized weight for each pair of $$(\boldsymbol{\mu}_i,\boldsymbol{\Sigma}_i)$$ and $$\alpha$$ is a chosen hyperparameter constant that gets added to each element of the estimated covariance matrix, which determines the degree of dispersion of features sampled from the calibrated distributions.

We further experimented with Vision Transformer (ViT) as a feature extractor and due to high complexity and insufficient pre-training, ViT does not produce more meaningful features.

### References

1. Shuo Yang, Lu Liu, and Min Xu. "Free Lunch for Few-shot Learning: Distribution Calibration". In: *International Conference on Learning Representations (ICLR)*. 2021.

---

- [GitHub](https://github.com/kapikantzari/11785-FSL)
- [Final Report](/files/11785_report.pdf)

[back](/misc/)
