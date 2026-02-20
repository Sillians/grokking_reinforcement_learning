# Introduction to Deep Reinforcement Learning

## Definition and Big Picture

Deep RL is a type of Machine Learning where an agent learns how to behave in an environment by performing actions and seeing the results.

The idea behind Reinforcement Learning is that an agent (an AI) will learn from the environment by interacting with it (through trial and error) and receiving rewards (negative or positive) as feedback for performing actions.

Learning from interactions with the environment comes from our natural experiences.

- For instance, imagine putting your little brother in front of a video game he never played, giving him a controller, and leaving him alone. 
- Your brother will interact with the environment (the video game) by pressing the right button (action). He got a coin, that’s a +1 reward. It’s positive, he just understood that in this game he must get the coins.
- But then, he presses the right button again and he touches an enemy. He just died, so that’s a -1 reward.
- By interacting with his environment through trial and error, your little brother understands that he needs to get coins in this environment but avoid the enemies.
- Without any supervision, the child will get better and better at playing the game.

That’s how humans and animals learn, through interaction. Reinforcement Learning is just a computational approach of learning from actions.

`Reinforcement learning is a framework for solving control tasks (also called decision problems) by building agents that learn from the environment by interacting with it through trial and error and receiving rewards (positive or negative) as unique feedback.`


### The RL Process

`The reward hypothesis: the central idea of Reinforcement Learning`
- Why is the goal of the agent to maximize the expected return?
  - Because RL is based on the `reward hypothesis`, which is that all goals can be described as the maximization of the expected return (expected cumulative reward).
  - That’s why in Reinforcement Learning, `to have the best behavior`, we aim to learn to take actions that `maximize the expected cumulative reward`.


### Markov Property

The Markov Property implies that our agent needs only the current state to decide what action to take and not the history of all the states and actions they took before.


### Observations/States Space

Observations/States are the information our agent gets from the environment. In the case of a video game, it can be a frame (a screenshot). In the case of the trading agent, it can be the value of a certain stock, etc.


### Action Space

The Action space is the set of all possible actions in an environment.

The actions can come from a discrete or continuous space:
- `Discrete space`: the number of possible actions is `finite`.
- `Continuous space`: the number of possible actions is `infinite`.


### Rewards and the discounting

The reward is fundamental in RL because it’s the only feedback for the agent. Thanks to it, our agent knows if the action taken was good or not.


## Tasks

A task is an instance of a Reinforcement Learning problem. We can have two types of tasks: `episodic` and `continuing`.

- Episodic task
  - In this case, we have a starting point and an ending point (a terminal state). This creates an episode: a list of States, Actions, Rewards, and new States.
  - For instance, think about Super Mario Bros: an episode begins at the launch of a new Mario Level and ends when you’re killed or you reached the end of the level.
  
- Continuing tasks
  - These are tasks that continue forever `(no terminal state)`. In this case, the agent must `learn how to choose the best actions and simultaneously interact with the environment`.
  - For instance, an agent that does automated stock trading. For this task, there is no starting point and terminal state. The agent keeps running until we decide to stop it.



## The Exploration/Exploitation trade-off

**the exploration/exploitation trade-off.**

- Exploration is exploring the environment by trying random actions in order to `find more information about the environment`.
- Exploitation is `exploiting known information to maximize the reward`.


This is what we call the exploration/exploitation trade-off. We need to balance how much we explore the environment and how much we exploit what we know about the environment.



## Two main approaches for solving RL problems

In other words, how do we build an RL agent that can select the actions that maximize its expected cumulative reward?

**The Policy π: the agent’s brain**

The `Policy π` is the brain of our Agent, it’s the function that tells us what action to take given the state we are in. So it defines the agent’s behavior at a given time.

This Policy is the function we want to learn, our goal is to find the optimal `policy π*`, the policy that maximizes expected return when the agent acts according to it. We find this `π* through training`.


There are two approaches to train our agent to find this optimal policy π*:

- Directly, by teaching the agent to learn which action to take, given the current state: `Policy-Based Methods`.
- Indirectly, teach the agent to learn which state is more valuable and then take the action that leads to the more valuable states: `Value-Based Methods`.


### **Policy-Based Methods**

In Policy-Based methods, we learn a policy function directly.

This function will define a mapping from each state to the best corresponding action. Alternatively, it could define `a probability distribution over the set of possible actions at that state`.

We have two types of policies:

- `Deterministic`: a policy at a given state will always return the same action.
- `Stochastic`: outputs a probability distribution over actions.


### **Value-based methods**

In value-based methods, instead of learning a policy function, we `learn a value function` that maps a state to the expected value `of being at that state.`

The value of a state is the `expected discounted return` the agent can get if it `starts in that state, and then acts according to our policy`.

“Act according to our policy” just means that our policy is `“going to the state with the highest value”`.

Thanks to our value function, at each step our policy will select the state with the biggest value defined by the value function: `-7`, then `-6`, then `-5` (and so on) to attain the goal.



## **The “Deep” in Reinforcement Learning**

Deep Reinforcement Learning introduces `deep neural networks` to solve `Reinforcement Learning problems` — hence the name “deep”.

For instance, in the next unit, we’ll learn about `two value-based algorithms`: `Q-Learning` (classic Reinforcement Learning) and then `Deep Q-Learning`.

- You’ll see the difference is that, in the first approach, we use a traditional algorithm to create a `Q table` that helps us find what action to take for each state.

- In the second approach, we will use a Neural Network (to approximate the Q value).


## Summary

- Reinforcement Learning is a computational approach of learning from actions. We build an agent that learns from the environment by interacting with it through trial and error and receiving rewards (negative or positive) as feedback.

- The goal of any RL agent is to maximize its expected cumulative reward (also called expected return) because RL is based on the reward hypothesis, which is that all goals can be described as the maximization of the expected cumulative reward.

- The RL process is a loop that outputs a sequence of state, action, reward and next state.

- To calculate the expected cumulative reward (expected return), we discount the rewards: the rewards that come sooner (at the beginning of the game) are more probable to happen since they are more predictable than the long term future reward.

- To solve an RL problem, you want to find an optimal policy. The policy is the “brain” of your agent, which will tell us what action to take given a state. The optimal policy is the one which gives you the actions that maximize the expected return.

- There are two ways to find your optimal policy:
  - By training your policy directly: policy-based methods.
  - By training a value function that tells us the expected return the agent will get at each state and use this function to define our policy: value-based methods.

- Finally, we speak about Deep RL because we introduce deep neural networks to estimate the action to take (policy-based) or to estimate the value of a state (value-based) hence the name “deep”.




- **Agent**: An agent learns to make decisions by trial and error, with rewards and punishments from the surroundings.

- **Environment**: An environment is a simulated world where an agent can learn by interacting with it.

- **Markov Property**: It implies that the action taken by our agent is conditional solely on the present state and independent of the past states and actions.

- **Observations/State**: 
  - State: Complete description of the state of the world.
  - Observation: Partial description of the state of the environment/world.

- **Actions**:
  - Discrete Actions: Finite number of actions, such as left, right, up, and down.
  - Continuous Actions: Infinite possibility of actions; for example, in the case of self-driving cars, the driving scenario has an infinite possibility of actions occurring.

- **Rewards and Discounting**: 
  - Rewards: Fundamental factor in RL. Tells the agent whether the action taken is good/bad.
  - RL algorithms are focused on maximizing the cumulative reward.
  - Reward Hypothesis: RL problems can be formulated as a maximisation of (cumulative) return.
  - Discounting is performed because rewards obtained at the start are more likely to happen as they are more predictable than long-term rewards.

- **Tasks**: 
  - Episodic: Has a starting point and an ending point.
  - Continuous: Has a starting point but no ending point.

- **Exploration v/s Exploitation Trade-Off**:
  - Exploration: It’s all about exploring the environment by trying random actions and receiving feedback/returns/rewards from the environment.
  - Exploitation: It’s about exploiting what we know about the environment to gain maximum rewards.
  - Exploration-Exploitation Trade-Off: It balances how much we want to explore the environment and how much we want to exploit what we know about the environment.


- **Policy**:
  - Policy: It is called the agent’s brain. It tells us what action to take, given the state.
  - Optimal Policy: Policy that maximizes the expected return when an agent acts according to it. It is learned through training.


- **Policy-based Methods:**
  - An approach to solving RL problems.
  - In this method, the Policy is learned directly.
  - Will map each state to the best corresponding action at that state. Or a probability distribution over the set of possible actions at that state.


- **Value-based Methods:**
  - Another approach to solving RL problems.
  - Here, instead of training a policy, we train a value function that maps each state to the expected value of being in that state.



























