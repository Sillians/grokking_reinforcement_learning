## PPO in PyTorch from scratch

- Vector environment
- Layer initialization
- Adam's epsilon
- Learning rate annealing
- General Advantage Estimation
- Minibatch update
- Advantage normalization
- Clipped objective
- Value loss clipping
- Entropy loss
- Global gradient clipping
- Early stopping


### Refs

- [Navigating the RLHF Landscape: From Policy Gradients to PPO, GAE, and DPO for LLM Alignment](https://huggingface.co/blog/NormalUhr/rlhf-pipeline)




`CartPole` is a classic reinforcement learning environment and control theory problem where an agent moves a cart left or right to balance an inverted, un-actuated pole vertically. The goal is to maximize reward (+1 per step) by keeping the pole upright for as long as possible. It is widely used in AI training to test, develop, and benchmark control algorithms, often via OpenAI Gym/Gymnasium.

### **Key Aspects of CartPole**

- `System Components`: A pole attached to a cart, which moves along a frictionless track.
- `Goal`: Balance the pole and keep it from tipping over (±12 degrees).
- `Actions`: Discrete actions, usually 0 (push left) or 1 (push right).
- `Observation Space`: Continuous,
- `Termination`: An episode ends if the pole falls over (angle too large), the cart moves too far off-center, or a maximum time step limit is reached.



