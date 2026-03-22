**Papers**

- [A Comprehensive Survey of Reward Models: Taxonomy, Applications, Challenges, and Future](https://arxiv.org/pdf/2504.12328)

- [Reward Models in Deep Reinforcement Learning: A Survey](https://arxiv.org/pdf/2506.15421)

# Reward Models

Think of a **Reward Model (RM)** as a "digital judge." Its entire purpose is to look at a prompt and a response, then output a single number (a scalar score) that represents how `"good"` that response is based on human preferences.

In the standard RLHF pipeline, we can’t have a human sit there 24/7 to grade every single thing the AI says during training. Instead, we train the Reward Model to **mimic** human judgment so it can grade the AI millions of times per second.


## 1. How a Reward Model is Built

A Reward Model isn't born knowing what "good" looks like; it has to be taught through a specific process:

1. **Data Collection:** Humans are shown two or more responses to the same prompt (e.g., Response A and Response B).
2. **Ranking:** The human ranks them (e.g., "A is more helpful than B").
3. **Preference Learning:** The RM is trained using a **Bradley-Terry model**. The goal is to ensure the model assigns a higher scalar value to the preferred response.

### The Math Behind the Judge

To train the RM, we minimize a loss function that looks like this:


$$\mathcal{L}(\theta) = -\mathbb{E}_{(x, y_w, y_l) \sim D} \left[ \log \sigma (r_\theta(x, y_w) - r_\theta(x, y_l)) \right]$$

* $x$: The prompt.
* $y_w$: The "winning" (preferred) response.
* $y_l$: The "losing" response.
* $r_\theta$: The Reward Model's predicted score.
* $\sigma$: The sigmoid function (to turn the difference into a probability).



## 2. Types of Reward Models

Not all judges look for the same things. Depending on the goal, researchers use different types of RMs:

* **Outcome Reward Models (ORM):** These only grade the final answer. If the answer is right, it gets a +1; if wrong, a 0. This is common in basic RLHF.
* **Process Reward Models (PRM):** These are the "new hotness" in AI reasoning (like in OpenAI’s o1 or DeepSeek). They grade **every single step** of a thought process. This helps prevent the model from getting the right answer for the wrong reasons (hallucinated logic).
* **Safety Reward Models:** These are specifically tuned to give very low scores to toxic, biased, or dangerous content, even if that content is technically "helpful" to the user's request.



## 3. The "Reward Hacking" Problem

Reward models aren't perfect. Because they are just neural networks, the Policy Model (the AI being trained) often finds "loopholes" to get high scores without actually being better.

> **Example:** An AI might learn that the Reward Model loves long, flowery responses. To get a high score, the AI starts writing 5-page essays for simple "Yes/No" questions. This is called **Reward Hacking**, and it’s why we use a "Reference Model" to keep the AI from changing its behavior too drastically.



## Comparison: Reward Model vs. Policy Model

| Feature | Policy Model (The Actor) | Reward Model (The Judge) |
| --- | --- | --- |
| **Output** | Text (Tokens) | A single number (Scalar) |
| **Goal** | Generate the best response | Predict how a human would rank the response |
| **Size** | Usually the full-size model | Often a smaller, "distilled" version |




---

# Models for RLHF and RLVR

Reinforcement Learning from Human Feedback (**RLHF**) and Reinforcement Learning from Verifiable Rewards (**RLVR**) are the two primary frameworks used to align Large Language Models (LLMs) with human intent and factual correctness.

While they share a common goal, they rely on different model architectures and training setups.


## 1. Models Used in RLHF

`RLHF` is the standard method for making AI helpful, honest, and harmless. It typically involves a "triad" of models working in tandem:

* **The Policy Model (Actor):** This is the LLM being trained (e.g., `Llama 3`, `GPT-4`, or `Claude`). It takes a prompt and generates a response. Its weights are updated during the RL process to maximize reward.

* **The Reward Model (RM):** A separate, usually smaller model trained on a dataset of human rankings (e.g., "Response A is better than Response B"). It acts as a proxy for human judgment, assigning a scalar score to the Policy Model's outputs.

* **The Reference Model:** A frozen copy of the Policy Model before RL began. It is used to calculate **KL Divergence**, ensuring the RL-tuned model doesn't drift too far from its original language capabilities or collapse into gibberish.



## 2. Models Used in RLVR

`RLVR` is a more recent evolution, popularized by models like **DeepSeek-R1**. It focuses on domains where "truth" can be verified by a system rather than a human (like Math or Coding).

* **The Reasoning Model (Policy):** Similar to RLHF, this is the main LLM. However, in RLVR, the model is often encouraged to produce a "Chain of Thought" (CoT) before reaching a final answer.

* **The Verifier (Environment):** Unlike RLHF, there is often **no neural Reward Model**. Instead, the "model" here is a **Rule-Based Verifier** or a Compiler.
  * **In Coding:** A unit test or compiler checks if the code runs.
  * **In Math:** A symbolic solver checks if the final answer matches the ground truth.

* **The Outcome Reward Model (ORM) / Process Reward Model (PRM):** In some RLVR setups, a PRM is used to grade each individual step of a reasoning chain, rather than just the final result.



## Key Differences at a Glance

| Feature | RLHF | RLVR |
| --- | --- | --- |
| **Primary Reward Source** | Human Preference (via Reward Model) | Verifiable Outcomes (Code/Math) |
| **Model Focus** | Style, Safety, and Tone | Logic, Accuracy, and Reasoning |
| **Typical Models** | GPT-4, Claude, Llama-3-Chat | DeepSeek-R1, OpenAI o1, AlphaCode |
| **Main Challenge** | Reward Hacking (tricking the RM) | Sparse Rewards (getting it 100% right) |

