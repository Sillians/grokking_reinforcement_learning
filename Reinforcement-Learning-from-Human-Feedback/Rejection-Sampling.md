# Rejection Sampling

Papers: 

- [A Minimalist Approach to LLM Reasoning: from Rejection Sampling to Reinforce](https://arxiv.org/abs/2504.11343)
- [Rejection Sampling🔗](https://rlhfbook.com/c/09-rejection-sampling)
- [A Minimalist Approach to LLM Reasoning: from Rejection Sampling to Reinforce](https://huggingface.co/papers/2504.11343)
- [Demystifying Reasoning Models](https://cameronrwolfe.substack.com/p/demystifying-reasoning-models)
- [Rejection Fine-Tuning (RFT)](https://www.emergentmind.com/topics/rejection-fine-tuning-rft)


Rejection sampling in reinforcement learning (RL), particularly in the context of Large Language Models (LLMs) and robotics, is a technique used to improve policy performance by filtering out low-quality trajectories and keeping only those with high rewards. It serves as a simplified alternative or precursor to complex policy gradient methods like PPO, allowing models to learn from superior, verified examples.


**Key Aspects of Rejection Sampling in RL:**

- `Process`: The model generates multiple candidate actions or complete trajectories for a given prompt or state. A reward model or verifier (e.g., code interpreter, math solver) evaluates these candidates. Only samples exceeding a certain threshold are accepted, and the model is then fine-tuned on this high-quality dataset.

- `RAFT (Rejection-Sampling Fine-Tuning)`: This is a popular application where only positively rewarded samples are used to fine-tune the model, acting as a "minimum" RL algorithm.

- `Benefits`: It provides stable training, avoids the instability of high-dimensional policy gradients, and often converges faster than PPO.

- `Limitations`: Rejection sampling relies on high-quality generation, and if not managed, can lead to a drop in policy entropy, limiting exploration.

- `Application in R1 (DeepSeek)`: The DeepSeek-R1 model uses a multi-phase training process that includes a Rejection Sampling phase, where the model generates samples filtered by a judge, creating a high-quality dataset for SFT.

- `Comparison to PPO`: While PPO uses all generated samples with calculated rewards to update the policy, rejection sampling acts as an "offline" filtering mechanism, focusing only on the best behaviors.



**Rejection Sampling vs. GRPO/PPO:**

While methods like `Group Relative Policy Optimization (GRPO)` use relative rewards to guide learning, rejection sampling is purely extractive, choosing only the best results. However, research suggests that simple rejection-based methods (like RAFT) are surprisingly competitive with complex RL methods like GRPO for improving reasoning skills.


