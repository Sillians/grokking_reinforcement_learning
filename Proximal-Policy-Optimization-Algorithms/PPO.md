# Proximal Policy Optimization (PPO): Deep Dive
PPO (baseline RL algorithm used in many RLHF setups).

Paper: **Proximal Policy Optimization Algorithms**  
Authors: **John Schulman, Filip Wolski, Prafulla Dhariwal, Alec Radford, Oleg Klimov (OpenAI)**  
arXiv: [1707.06347](https://arxiv.org/abs/1707.06347)

## 1) Problem Setting and Motivation

PPO is a policy-gradient family designed to keep updates stable while remaining simple and first-order.

- Vanilla policy gradients can take destructively large steps.
- TRPO stabilizes updates with a KL trust region constraint, but adds implementation complexity.
- PPO approximates trust-region behavior with easier objectives that work with minibatch SGD/Adam.

The core idea is to alternate:
1. collect fresh on-policy data,

2. optimize a surrogate objective for multiple epochs on that batch.

## 2) Background Equations (from the paper)

### 2.1 Policy gradient estimator (Eq. 1)


$$\hat g = \hat{\mathbb E}_t\left[\nabla_\theta \log \pi_\theta(a_t|s_t)\hat A_t\right]$$

Meaning:
- $(\pi_\theta(a_t|s_t))$: policy probability of taken action $(a_t)$ at state $(s_t)$.

- $(\hat A_t)$: estimated advantage (how much better/worse action $(a_t)$ was than baseline).

- $(\hat{\mathbb E}_t[\cdot])$: empirical average over sampled timesteps.


### 2.2 Objective whose gradient gives Eq. 1 (Eq. 2)

$$L^{PG}(\theta)=\hat{\mathbb E}_t\left[\log \pi_\theta(a_t|s_t)\hat A_t\right]$$

Meaning: maximize this to perform policy gradient ascent.  
Issue: repeatedly optimizing this objective on the same batch can move policy too far.

### 2.3 TRPO constrained surrogate (Eq. 3-4)


$$\max_\theta \ \hat{\mathbb E}_t\left[\frac{\pi_\theta(a_t|s_t)}{\pi_{\theta_{old}}(a_t|s_t)}\hat A_t\right]$$


$$\text{s.t.}\ \hat{\mathbb E}_t\left[KL\left(\pi_{\theta_{old}}(\cdot|s_t),\pi_\theta(\cdot|s_t)\right)\right]\le \delta$$


Meaning:
- ratio term says how action probability changed under new vs old policy.

- KL constraint limits policy drift per update.

### 2.4 TRPO penalty form (Eq. 5)


$$\max_\theta\ \hat{\mathbb E}_t\left[\frac{\pi_\theta(a_t|s_t)}{\pi_{\theta_{old}}(a_t|s_t)}\hat A_t - \beta\,KL\left(\pi_{\theta_{old}}(\cdot|s_t),\pi_\theta(\cdot|s_t)\right)\right]$$


Meaning: replace hard trust-region constraint with a soft KL penalty weighted by $(\beta)$.



## 3) PPO Core Surrogate Objectives

Define probability ratio:


$$r_t(\theta)=\frac{\pi_\theta(a_t|s_t)}{\pi_{\theta_{old}}(a_t|s_t)}$$

At $(\theta=\theta_{old})$, $(r_t(\theta)=1)$.

### 3.1 CPI surrogate (Eq. 6)


$$L^{CPI}(\theta)=\hat{\mathbb E}_t\left[r_t(\theta)\hat A_t\right]$$


`Meaning`: first-order improvement proxy used in conservative policy iteration.  
`Issue`: unconstrained maximization can make policy changes too large.


### 3.2 PPO-Clip objective (Eq. 7, main contribution)


$$L^{CLIP}(\theta)=\hat{\mathbb E}_t\left[\min\left(r_t(\theta)\hat A_t,\ \text{clip}(r_t(\theta),1-\epsilon,1+\epsilon)\hat A_t\right)\right]$$


Meaning of each term:
- $(r_t(\theta)\hat A_t)$: usual surrogate gain.

- $(\text{clip}(r_t,1-\epsilon,1+\epsilon))$: truncates ratio to trust-region-like interval.

- $(\min(\cdot,\cdot))$: pessimistic bound; ignores policy-ratio changes that would artificially improve objective beyond clip band.



Intuition by advantage sign:
- If $(\hat A_t>0)$: increasing action probability helps, but gains are capped once $(r_t>1+\epsilon)$.

- If $(\hat A_t<0)$: decreasing action probability helps, but gains are capped once $(r_t<1-\epsilon)$.


This is why clipping prevents overly aggressive updates while preserving useful directionality.


### 3.3 PPO with adaptive KL penalty (Eq. 8)


$$L^{KLPEN}(\theta)=\hat{\mathbb E}_t\left[r_t(\theta)\hat A_t-\beta\,KL\left(\pi_{\theta_{old}}(\cdot|s_t),\pi_\theta(\cdot|s_t)\right)\right]$$


Paper’s adaptive rule:
- compute $(d=\hat{\mathbb E}_t[KL(\pi_{\theta_{old}},\pi_\theta)])$

- if $(d<d_{targ}/1.5)$, set $(\beta\leftarrow\beta/2)$

- if $(d>d_{targ}\times1.5)$, set $(\beta\leftarrow\beta\times2)$


The paper includes this as a baseline; empirically, clipped PPO generally performs better.


## 4) Full Actor-Critic Training Objective (Eq. 9)

When policy and value function are trained together:


$$L_t^{CLIP+VF+S}(\theta)=\hat{\mathbb E}_t\left[L_t^{CLIP}(\theta)-c_1L_t^{VF}(\theta)+c_2S[\pi_\theta](s_t)\right]$$


Where:
- $(L_t^{VF}(\theta)=(V_\theta(s_t)-V_t^{targ})^2)$: value regression loss.
- $`(S[\pi_\theta](s_t))`$: entropy bonus (encourages exploration).
- $(c_1, c_2)$: tradeoff coefficients.



Interpretation:
- maximize policy improvement term,

- penalize value error,

- reward entropy to avoid premature collapse.



## 5) Advantage Estimation in PPO (Eq. 10-12)

### 5.1 Finite-horizon estimator (Eq. 10)


$$\hat A_t=-V(s_t)+\sum_{l=0}^{T-t-1}\gamma^l r_{t+l}+\gamma^{T-t}V(s_T)$$

Meaning: truncated return-to-go minus baseline value at $(s_t)$.


### 5.2 Truncated GAE (Eq. 11-12)


$$\hat A_t=\sum_{l=0}^{T-t-1}(\gamma\lambda)^l\delta_{t+l}$$

$$\delta_t=r_t+\gamma V(s_{t+1})-V(s_t)$$


Meaning:
- $(\delta_t)$: one-step TD residual.

- GAE accumulates discounted TD residuals.

- $(\lambda)$ controls bias-variance tradeoff.

- $(\lambda=1)$ recovers the finite-horizon estimator form.



## 6) PPO Algorithm (Actor-Critic style, Algorithm 1)

For each iteration:
1. Run $(N)$ parallel actors using $(\pi_{\theta_{old}})$ for $(T)$ timesteps each (total $(NT)$ samples).

2. Compute $(\hat A_t)$ (typically GAE) and value targets.

3. Optimize chosen surrogate objective ($(L^{CLIP})$ or $(L^{KLPEN})$) for $(K)$ epochs with minibatches of size $(M\le NT)$.

4. Set $(\theta_{old}\leftarrow\theta)$ and repeat.


Key practical point: multiple SGD epochs on the same on-policy batch are what give PPO much better sample use than one-update-per-sample vanilla PG.


## 7) Hyperparameters Reported in the Paper

### MuJoCo benchmark (Table 3)
- Horizon $(T=2048)$

- Adam step size $(3\times10^{-4})$

- Epochs $(=10)$

- Minibatch size $(=64)$

- $(\gamma=0.99)$

- $(\lambda=0.95)$

- clipping values tested: $(\epsilon\in\{0.1,0.2,0.3\})$



### Atari setup (Table 5)
- Horizon $(T=128)$

- Adam step size $(2.5\times10^{-4}\cdot \alpha)$ (annealed $(\alpha:1\to0)$)

- Epochs $(=3)$

- Minibatch size $(=32\times8)$

- Number of actors $(=8)$

- $(\gamma=0.99,\ \lambda=0.95)$

- clip parameter $(\epsilon=0.1\cdot\alpha)$

- $(c_1=1,\ c_2=0.01)$



## 8) Main Empirical Findings in the Paper

- On continuous control, clipped PPO outperformed no-clip/no-penalty and generally beat KL-penalty variants.

- Against prior online policy methods (TRPO, A2C variants, CEM, tuned vanilla PG), PPO was best on most MuJoCo tasks tested.

- On Atari (49 games), PPO won more games on the "average over full training" metric, while ACER won more on the "final 100 episodes" 
metric.

- Overall conclusion: PPO gives a strong tradeoff between stability, sample efficiency, simplicity, and wall-time.



## 9) What Makes PPO Work

1. **Proximal updates** via ratio clipping (or adaptive KL) prevent policy collapse.

2. **Multiple minibatch epochs** improve data usage per trajectory.

3. **Advantage normalization/estimation quality** strongly affects stability.

4. **Actor-critic objective composition** balances policy learning, value fitting, and exploration.



In short, PPO keeps the implementation close to vanilla policy gradients, but injects trust-region-like safeguards through a `clipped surrogate objective` that is easy to optimize with first-order methods.
