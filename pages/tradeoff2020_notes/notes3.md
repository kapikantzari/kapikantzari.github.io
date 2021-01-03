---
layout: post
---

## [Understanding and Mitigating the Tradeoff Between Robustness and Accuracy](https://arxiv.org/abs/2002.10716)

#### Terminologies

- **Inductive bias** is the set of extra assumptions that the learning algorithm makes about the target function that enables it to generalize beyond the training data.

#### Challenge/Opportunity

- Adversarial training with perturbed training set improves robust error but often increases the standard error.

#### SOA Knowledge/Solution

- Previous explanations are based on an inherent accuracy-robustness tradeoff that persists even in the infinite data limit and show that standard and robust error are fundamentally at odds.
- **Robust self-training (RST)** is introduced as a robust variant of self-training that overcomes the sample complexity barrier of learning a model with low robust error by leveraging extra unlabeled data.
- **Interpolated Adversarial Training (IAT)** that considers a different training algorithm based on Mixup and **Neural Architecture Search (NAS)** that uses RL to search for more robust architectures were proposed to mitigate the tradeoff between standard and robust error empirically.

#### Approach

- Consider a linear model where the true linear function has zero standard and robust error with adversarial training augmenting with extra data where the perturbations are consistent: $P_y(\cdot\mid x_{\text{ext}})=P_y(\cdot\mid x)$.

- Compare the standard errors of the standard estimator and the augmented estimator in noiseless linear regression and characterize the general conditions for standard error not increasing in fitting augmented data.

- Examine the general RST estimator in the linear regression setting, study the effect of the labeled training set sizes on RST improving standard and robust error over vanilla adversarial training, and evaluate RST empirically.

  RST formulation:

  1. Perform standard training on labeled data 
     $$
     \{(x_i, y_i\}_{i=1}^n
     $$
      to obtain a standard estimator 
     $$
     \hat{\theta}_\text{std}=\arg\min_\theta\sum_{i=1}^n\ell(f_\theta(x_i), y_i)
     $$
     

  2. Perform robust training on both the labeled and unlabeled inputs 
     $$
     \{\hat{x}_i\}_{i=1}^m
     $$
      with pseudo-labels 
     $$
     \hat{y}_i=f_{\hat{\theta}_\text{std}}(\hat{x}_i)
     $$
      and combined loss to obtain 
     $$
     \hat{\theta}_\text{rst}=\arg\min_\theta(\alpha\hat{L}_\text{std-lab}(\theta)+\beta\hat{L}_\text{rob-lab}(\theta)+\gamma\hat{L}_\text{std-unlab}(\theta;\hat{\theta}_\text{std}))+\lambda\hat{L}_\text{rob-unlab}(\theta;\hat{\theta}_\text{std}))
     $$
      for fixed $\alpha,\beta,\gamma,\lambda\ge 0$

#### Results

- The  <mark>mismatch between the inductive bias of models and the underlying distribution of the inputs</mark> causes the standard error to increase even when the augmented data is perfectly labeled.

  - <mark>An augmentation that encourages the fitting local components can potentially increase the error along other global components with larger eigenvalue in $\Sigma$ results in larger standard error of $\hat{\theta}_\text{aug}$.</mark>
  - For a large increase in standard error upon augmentation, the true parameter needs to be sufficiently more complex than $\hat{\theta}_\text{std}$.
  - Conditions for the standard error not increasing when fitting augmented data:
    1. The population covariance $\Sigma=\mathbb{E}_{P_x}[xx^T]$ is identity.
    2. The augmented data $[X_\text{std};X_\text{ext}]$ spans the entire space.
    3. The extra data $x_\text{ext}\in\mathbb{R}^d$ is a single point such that it's an eigenvector of $\Sigma$.

- RST upperbounds the 
  $$
  L_\text{std}(\hat{\theta}_\text{rst})
  $$
   by 
  $$
  L_\text{std}(\hat{\theta}_\text{std})
  $$
   and makes 
  $$
  L_\text{rob}(\hat{\theta}_\text{rst})=L_\text{std}(\hat{\theta}_\text{rst})
  $$
   by incorporating a $\Sigma$-induced regularizer while satisfying the robust losses constraints.

- <mark>Empirical evidence shows that RST improves both the standard and robust error and mitigate the tradeoff between accuracy and robustness</mark>.

#### Thoughts

- This paper complements [[1]](./notes1.html) by explaining the tradeoff in another theoretical setting and showing the tradeoff doesn't exist in infinite data.

  What is the significance of the noiseless linear regression setting?

- <mark>Another reason for the accuracy-robustness tradeoff is **overparametrization**.</mark>

- A natural strategy to mitigate this tradeoff is to <mark>regularize the augmented estimator to be closer to the standard estimator</mark> (which incorporates information about $\Sigma$).

- <mark>RST captures the global structure and obtains low standard error by fitting extra inputs while enforcing invariance on local transformations on both labeled and unlabeled inputs for obtaining low robust error by capturing the local structure across the domain.</mark>