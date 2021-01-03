---
layout: post

---

## [Robustness May Be at Odds with Accuracy](https://arxiv.org/abs/1805.12152)

#### Terminologies

- **Adversarial examples** synthesize small, imperceptible perturbations of the input data and cause the model to make highly-confident but erroneous predictions.
- **Adversarial training** repeatedly finds the worst perturbations and updates the model to reduce the loss on perturbed inputs, which requires more training time/data.
- **Transferability** is the phenomenon that one set of adversarial examples is likely to fool other classifiers learned from samples of the same distribution.

#### Challenge/Opportunity

- Many proposed robust training methods are ineffective.
- Computational complexity and training data may not be the only costs of adversarial robustness, and it remains a question if robust model is always preferable than a standard model.
- Empirical evidence shows that there exists a trade-off between standard and adversarially robust accuracy.

#### SOA Knowledge/Solution

- It is already known that robustness comes at a cost of computational complexity and more training data.
- The most successful robust training approach so far is adversarial training.
- Adversarial perturbation is commonly mistaken as a form of data augmentation that benefits standard accuracy.
- Previous works prove upper bounds on the robustness of classifiers and show regularized gradients are more human-aligned and give better adversarial examples.

#### Approach

- The trade-off between standard accuracy and robustness is demonstrated in a simple binary classification task.

#### Results

- The paper concludes that <mark>the trade-off is caused by the fundamental difference in the feature representations learned by standard and robust models</mark>.
- Robustness is unlikely resulted from standard training, so robust training methods are important.
- <mark>Robust models learn more human-aligned feature representations</mark>.
- Robust models yield clean feature interpolations similar to those obtained from generative models.

#### Thoughts

- This trade-off between standard and adversarially robust accuracy is significant since it contradicts a misconception that sufficient amount of training data will build a robust model. There are two aspects in terms of training a model: feature selection and feature learning. The paper shows that robustness is about the former while the amount of training data affects the latter.

- Robust training methods are more connected to generative networks like GAN since both focus on selecting and twisting the most correlated feature to confuse the model/discriminator.

- Humans can have lower accuracy in certain benchmarks than ML models potentially because ML models rely on brittle features that humans are naturally invariant to. So is higher standard accuracy really an indicator for better model?

- Robustness may be a better indicator because of cross-model adversarial transferability.

- <mark>Adversarial training may be sufficient for attaining more human-aligned gradients.</mark>