# Elicitation Theory of Post-training

The Elicitation Theory of post-training posits that alignment techniques (RLHF, SFT) primarily extract, amplify, and structure latent capabilities already acquired by base models during pretraining, rather than teaching new skills from scratch. It suggests that post-training makes existing, hidden, or disorganized knowledge accessible and controllable for specific tasks.

This theory folds in with the reality that the majority of gains users are seeing are from post-training because it
implies that there is more latent potential in a model pretraining on the internet than we can simply teach the model — such as by passing certain narrow samples in repeatedly during early types of post-training (i.e. only instruction tuning). The challenge of post-training is to reshape models from next-token prediction to conversation question-answering, while extracting all of this knowledge and intelligence from pretraining.


**Key Principles of Elicitation Theory:**

- `Surface Latent Capabilities`: Post-training acts as a filter that brings out dormant reasoning or skills hidden within the model’s weights.

- `Efficient Parameter Usage`: Minimal training—sometimes just 10–100 specific parameters—can recover over 50% of the performance gap between base models and fully fine-tuned models.

- `Distinction from Teaching`: Elicitation differs from teaching (learning new, complex skills) because it requires significantly fewer parameters to achieve high performance.

- `Information-Constrained`: Elicitation can be modeled as a data-efficient process, where only a few examples are needed to teach the model how to use what it already knows.

- `Sample-Efficient Reinforcement`: Reinforcement learning (RL) in post-training acts as a tool to reinforce behaviors already implicitly present, making it highly effective.


Supervised fine-tuning (SFT) and RL are used to "un-lock" these latent capabilities, such as overcoming sandbagging (deliberate underperformance) or accessing knowledge through prompting.