# Reasoning Training & Inference-Time Scaling

### **Papers:**

- [REVIEW OF INFERENCE-TIME SCALING STRATEGIES: REASONING, SEARCH AND RAG](https://arxiv.org/pdf/2510.10787)
- [Inference-Time Scaling for Complex Tasks: Where We Stand and What Lies Ahead](https://arxiv.org/html/2504.00294v1)
- [Towards Inference-time Scaling for Continuous Space Reasoning](https://arxiv.org/html/2510.12167v1)
- [A Survey of Frontiers in LLM Reasoning: Inference Scaling, Learning to Reason, and Agentic Systems](https://arxiv.org/html/2504.09037v1)
- [Inference-Time Scaling for Generalist Reward Modeling](https://arxiv.org/pdf/2504.02495)
- [Inference-Time Computations for LLM Reasoning and Planning: A Benchmark and Insights](https://arxiv.org/html/2502.12521v1)

- Blog post about Inference-Time Scaling https://introl.com/blog/inference-time-scaling-research-reasoning-models-december 



`Reasoning training` and `inference-time scaling` improve Large Language Model (LLM) performance on complex tasks by dedicating more computational resources during query processing rather than just during training. Techniques include training for `long Chain-of-Thought (CoT)` and using search algorithms like `MCTS (Monte Carlo Tree Search)` to evaluate multiple reasoning paths.


**Key Aspects of Reasoning & Inference-Time Scaling**

- `Definition & Purpose`: Inference-time scaling shifts focus from increasing model parameters (training-time) to using more compute at inference time—"thinking" longer before answering. This allows smaller models to achieve performance comparable to much larger ones.

- `Reasoning Techniques (Output-Focused):`
  - `Chain-of-Thought (CoT):` Encouraging the model to generate intermediate steps.
  - `Tree of Thoughts (ToT):` Exploring multiple reasoning paths via search algorithms like MCTS.
  - `Self-Reflection/Critique:` Enabling the model to evaluate its own draft responses.

- `Training for Inference Scaling:`
  - `Reinforcement Learning (RL):` Training models with methods like `RLVR` or `GRPO` to foster efficient "thinking" processes.
  - `Process Reward Models (PRMs):` Training models to assign scores to intermediate reasoning steps, rather than just the final answer, allowing the model to choose the best path.

- `Inference-Time Scaling Strategies:`
  - `Best-of-N Sampling:` Generating multiple responses and using a `verifier` (reward model) to pick the best one.
  - `Search/Decoding Methods:` Implementing beam search to explore high-likelihood paths.

- `Benefits & Trade-offs:` Increased accuracy on math, coding, and logical reasoning. However, this comes at the cost of higher latency and higher financial expenses for API usage and GPU time.

