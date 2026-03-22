`Papers: `
- [Evaluating Reward Models for Language Modeling](https://arxiv.org/pdf/2403.13787)
- [RewardBench 2: Advancing Reward Model Evaluation](https://arxiv.org/pdf/2506.01937)

# RewardBench

`RewardBench` (specifically RewardBench 2) is a leading evaluation framework by Allen Institute for AI (AI2) designed to assess reward models (RMs) in language modeling, focusing on safety, reasoning, and instruction-following, with a leaderboard on Hugging Face. It evaluates models on unseen prompt-chosen-rejected trios, covering areas like math, factuality, and strict instruction adherence.


### Key aspects of RewardBench include:

- `Evaluation Focus`: It measures the ability of RMs to distinguish between better and worse model outputs, addressing challenges in RLHF (Reinforcement Learning from Human Feedback).

- `Data Composition`: The dataset includes, but is not limited to, chat, safety, and reasoning tasks, often using "hard" data where differences between choices are subtle.

- `RewardBench 2 (2025)`: The second version introduced more rigorous evaluation on "unseen" data, including new subsets for Factuality, Precise Instruction Following, and Ties, often showing significant performance drops for older models.

- `Methodology`: It evaluates models trained via various methods, including Direct Preference Optimization (DPO) and classifier-based approaches.

- `Multimodal/Other`: Variants like Multimodal RewardBench (for vision-language) and Agent-RewardBench have been developed to evaluate reward models in broader, specialized contexts. 