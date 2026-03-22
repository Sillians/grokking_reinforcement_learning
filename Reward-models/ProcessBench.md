`Papers:`

- [ProcessBench: Identifying Process Errors in Mathematical Reasoning](https://arxiv.org/pdf/2412.06559)
- [PRMBench: A Fine-grained and Challenging Benchmark for Process-Level Reward Models](https://arxiv.org/pdf/2501.03124)


# ProcessBench

`ProcessBench` is a specialized benchmark introduced in late 2024 (by Qwen/Alibaba researchers) designed to evaluate the capability of Large Language Models (LLMs) to identify errors in mathematical reasoning steps. It focuses on high-difficulty math problems (competition and Olympiad level) and serves as a tool for evaluating Process Reward Models (PRMs) and general critic models.


**Key Details of ProcessBench:**

- `Total Test Cases`: 3,400 problems.

- `Problem Domains`: Covers GSM8K, MATH, OlympiadBench, and Omni-MATH, ranging from grade-school to Olympiad difficulty.

- `Task Definition`: Models are provided with a step-by-step solution to a math problem and are required to identify the earliest step containing an error, or confirm if the entire solution is correct.

- `Annotation`: All solutions are annotated with step-by-step error locations by human experts.

- `Best-Performing Models`: As of the initial release, the QwQ-32B-Preview model demonstrated critique capability competitive with GPT-4o, while the reasoning-specialized o1-mini performed best.

- `Significance`: It addresses the limitation of existing PRMs (like PRM800K) that often fail to generalize to more challenging, advanced math problems. 


