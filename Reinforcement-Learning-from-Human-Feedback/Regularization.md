# Regularization

Regularization in the context of Reinforcement Learning from Human Feedback (RLHF) refers to techniques used to prevent overfitting and over-optimization of the reward model, which can lead to unintended consequences in the behavior of the trained agent. Regularization helps ensure that the agent's learned policy remains aligned with human values and generalizes well to new situations, rather than exploiting weaknesses in the reward model.

**Key Regularization Techniques in RLHF:**
1. **KL Divergence Regularization:** This technique constrains the policy updates during reinforcement learning to prevent the new policy from diverging too much from the original pre-trained model (or a reference model). By adding a KL divergence penalty to the loss function, we can control how much the policy is allowed to change, which helps mitigate reward hacking.

2. **Reward Model Ensembles:** Using multiple reward models and taking the minimum score can help prevent the agent from exploiting a single model's inaccuracies. This ensemble approach provides a more robust signal for training.

3. **Behavior-Supported Policy Optimization (BSPO):** This method restricts the RL agent to taking actions within the "distribution" of the reward model's training data, reducing the likelihood of generating out-of-distribution (OOD) responses that the reward model might inaccurately score.

4. **Adversarial Policy Optimization (AdvPO):** This technique optimizes the policy based on the lower bound (pessimistic) confidence interval of the reward model's prediction, which can help prevent over-optimization by ensuring that the policy is not rewarded for actions that the reward model is uncertain about.

5. **Iterative RLHF:** Repeating the data collection and reward modeling process allows for continuous refinement of the reward function as the model changes, helping to address issues of over-optimization over time.

6. **Direct Alignment Algorithms (DAAs):** Methods like Direct Preference Optimization (DPO) can also be regularized by controlling the KL budget, which helps prevent overfitting to the training data and ensures better generalization. 

Regularization is crucial in RLHF to maintain the balance between optimizing for human preferences and ensuring that the agent's behavior remains safe, aligned, and generalizable. Without proper regularization, agents can easily exploit the reward model, leading to behaviors that are misaligned with human values or that fail to generalize to new situations.


`Papers:`

- [Over-optimization in Reinforcement Learning from Human Feedback: Causes, Consequences, and Mitigation Strategies](https://arxiv.org/pdf/2504.12329)
- [AComedyofEstimators: On KL Regularization in RLTraining of LLMs](https://arxiv.org/pdf/2512.21852v3)
- [Mitigating Over-optimization in Reinforcement Learning from Human Feedback](https://arxiv.org/pdf/2506.15422)
- [Regularization in Reinforcement Learning](https://rl-vs.github.io/rlvs2021/class-material/regularized_mdp/Regularization_RL_RLVS.pdf)
- [Over-optimization in RLHF Code](https://arxiv.org/pdf/2506.15423)
- [Over-optimization in RLHF Code](https://huggingface.co/papers/2506.15423)
