# Maximum Likelihood Estimation (MLE): Deep Dive

## 1) Core Idea

Given observed data \(x_{1:n} = (x_1, \ldots, x_n)\) and a parametric model \(p(x \mid \theta)\), MLE chooses the parameter value that makes the observed data most probable under the model.

\[
\hat{\theta}_{MLE} = \arg\max_{\theta} L(\theta; x_{1:n})
\]

where the likelihood is

\[
L(\theta; x_{1:n}) = \prod_{i=1}^n p(x_i \mid \theta)
\]

If data are conditionally independent but not identically distributed, replace \(p(x_i \mid \theta)\) with the correct per-example term \(p_i(x_i \mid \theta)\).

## 2) Log-Likelihood

Because \(\log(\cdot)\) is monotone increasing, maximizing likelihood is equivalent to maximizing log-likelihood:

\[
\ell(\theta) = \log L(\theta) = \sum_{i=1}^n \log p(x_i \mid \theta)
\]

\[
\hat{\theta}_{MLE} = \arg\max_{\theta} \ell(\theta)
\]

Why this matters:
- Product becomes a sum (easier derivatives, better numerics).
- Avoids floating-point underflow from multiplying many small probabilities.

## 3) First- and Second-Order Conditions

### Score function

\[
s(\theta) = \nabla_\theta \ell(\theta)
\]

For an interior optimum, a necessary condition is:

\[
s(\hat{\theta}) = 0
\]

### Hessian

\[
H(\theta) = \nabla_\theta^2 \ell(\theta)
\]

At a local maximum, \(H(\hat{\theta})\) is negative semidefinite (negative definite for strict local maximum).

### Observed and Fisher information

Observed information:
\[
J(\theta) = -\nabla_\theta^2 \ell(\theta)
\]

Fisher information:
\[
I(\theta) = \mathbb{E}\left[ \left(\nabla_\theta \log p(X \mid \theta)\right)\left(\nabla_\theta \log p(X \mid \theta)\right)^T \right]
       = -\mathbb{E}\left[\nabla_\theta^2 \log p(X \mid \theta)\right]
\]

For i.i.d. data, \(I_n(\theta)=n I_1(\theta)\).

## 4) Canonical Closed-Form Examples

## 4.1 Bernoulli(\(p\))

Data: \(x_i \in \{0,1\}\), \(x_i \sim \text{Bernoulli}(p)\). Let \(k=\sum_i x_i\).

\[
\ell(p) = k\log p + (n-k)\log(1-p)
\]

Set derivative to zero:
\[
\frac{d\ell}{dp} = \frac{k}{p} - \frac{n-k}{1-p} = 0
\quad \Rightarrow \quad
\hat{p} = \frac{k}{n}
\]

Interpretation: MLE equals empirical success frequency.

## 4.2 Categorical / Multinomial

If category counts are \(n_1,\dots,n_K\), \(\sum_j n_j=n\), parameters \(p_j\), \(\sum_j p_j=1\):

\[
\hat{p}_j = \frac{n_j}{n}
\]

Interpretation: MLE matches empirical category proportions.

## 4.3 Gaussian mean (\(\sigma^2\) known)

Data: \(x_i \sim \mathcal{N}(\mu,\sigma^2)\), known \(\sigma^2\).

\[
\ell(\mu) = -\frac{1}{2\sigma^2}\sum_{i=1}^n (x_i-\mu)^2 + C
\]

Maximizer:
\[
\hat{\mu} = \bar{x} = \frac{1}{n}\sum_{i=1}^n x_i
\]

## 4.4 Gaussian variance (\(\mu\) unknown)

For \(x_i \sim \mathcal{N}(\mu,\sigma^2)\), joint MLE:

\[
\hat{\mu} = \bar{x}, \qquad
\hat{\sigma}^2_{MLE} = \frac{1}{n}\sum_{i=1}^n (x_i-\bar{x})^2
\]

Note: \(\hat{\sigma}^2_{MLE}\) uses \(1/n\), so it is biased for finite \(n\). The unbiased sample variance uses \(1/(n-1)\), but that is not the MLE.

## 4.5 Exponential(\(\lambda\))

Data: \(x_i \ge 0\), \(p(x \mid \lambda)=\lambda e^{-\lambda x}\).

\[
\ell(\lambda) = n\log\lambda - \lambda \sum_{i=1}^n x_i
\]

\[
\hat{\lambda} = \frac{n}{\sum_i x_i} = \frac{1}{\bar{x}}
\]

## 4.6 Poisson(\(\lambda\))

Data: \(x_i \in \{0,1,2,\ldots\}\), \(x_i \sim \text{Poisson}(\lambda)\).

\[
\ell(\lambda) = \sum_i \left(x_i\log\lambda - \lambda - \log(x_i!)\right)
\]

\[
\hat{\lambda} = \bar{x}
\]

## 5) MLE for Supervised Models

## 5.1 Linear regression as MLE

Model:
\[
y = X\beta + \epsilon,\quad \epsilon \sim \mathcal{N}(0,\sigma^2 I)
\]

Negative log-likelihood is proportional to squared error:
\[
-\ell(\beta) \propto \|y - X\beta\|_2^2
\]

So MLE for \(\beta\) equals ordinary least squares:
\[
\hat{\beta} = (X^T X)^{-1}X^T y
\]
(assuming \(X^T X\) invertible).

## 5.2 Logistic regression as MLE

For binary labels \(y_i \in \{0,1\}\):
\[
p(y_i=1\mid x_i,\theta)=\sigma(\theta^T x_i),\quad \sigma(z)=\frac{1}{1+e^{-z}}
\]

Log-likelihood:
\[
\ell(\theta)=\sum_{i=1}^n \left[y_i\log \sigma(\theta^T x_i) + (1-y_i)\log(1-\sigma(\theta^T x_i))\right]
\]

No closed form in general. Solve numerically (Newton, L-BFGS, SGD variants).

## 6) Numerical Optimization for MLE

When closed forms do not exist, maximize \(\ell(\theta)\) with iterative methods.

Gradient ascent:
\[
\theta_{k+1} = \theta_k + \eta_k \nabla_\theta \ell(\theta_k)
\]

Newton-Raphson:
\[
\theta_{k+1} = \theta_k - H(\theta_k)^{-1} \nabla_\theta \ell(\theta_k)
\]

Practical notes:
- Use log-likelihood, not raw likelihood.
- Reparameterize constrained params (example: \(\sigma = e^\rho\) for positivity).
- Check multiple initializations if non-convex.
- Monitor train/validation log-likelihood for misspecification or overfitting.

## 7) Large-Sample Properties

Under standard regularity conditions:

## 7.1 Consistency
\[
\hat{\theta}_{MLE} \xrightarrow{p} \theta_0
\]
MLE converges in probability to the true parameter \(\theta_0\).

## 7.2 Asymptotic normality
\[
\sqrt{n}\left(\hat{\theta}_{MLE}-\theta_0\right)
\xrightarrow{d}
\mathcal{N}\left(0, I_1(\theta_0)^{-1}\right)
\]

Equivalent finite-\(n\) approximation:
\[
\hat{\theta}_{MLE} \approx \mathcal{N}\left(\theta_0,\ I_n(\theta_0)^{-1}\right)
\]

## 7.3 Asymptotic covariance estimate
\[
\widehat{\text{Cov}}(\hat{\theta}) \approx J(\hat{\theta})^{-1}
\]

This gives approximate standard errors and confidence intervals.

## 8) Inference from Likelihood

## 8.1 Wald interval (component \(j\))
\[
\hat{\theta}_j \pm z_{\alpha/2}\sqrt{\widehat{\text{Var}}(\hat{\theta}_j)}
\]

## 8.2 Likelihood ratio test

For null \(H_0\) nested in full model:
\[
\Lambda = 2\left(\ell(\hat{\theta}_{full})-\ell(\hat{\theta}_{null})\right)
\]

Asymptotically, \(\Lambda \sim \chi^2_{df}\), where \(df\) is parameter count difference.

## 9) MLE vs MAP (Regularized MLE)

MLE:
\[
\hat{\theta}_{MLE}=\arg\max_\theta \ell(\theta)
\]

MAP:
\[
\hat{\theta}_{MAP}=\arg\max_\theta \left[\ell(\theta)+\log p(\theta)\right]
\]

So MAP is MLE plus log-prior term:
- Gaussian prior \(\Rightarrow\) L2-like penalty
- Laplace prior \(\Rightarrow\) L1-like penalty

MLE is recovered with a flat prior.

## 10) Common Failure Modes

- Model misspecification: the true data process is outside the assumed family.
- Non-identifiability: multiple parameter values produce same likelihood.
- Boundary estimates: optimizer pushes parameters to edges (example: Bernoulli all-ones data gives \(\hat p=1\)).
- Outlier sensitivity: heavy-tail mismatch can distort estimates.
- Local maxima: especially in latent-variable or mixture models.

## 11) Practical MLE Checklist

1. Write the correct data likelihood \(p(x_{1:n}\mid\theta)\).
2. Convert to log-likelihood and simplify.
3. Derive gradient (and Hessian if useful).
4. Solve analytically if possible; otherwise optimize numerically.
5. Validate optimum (gradient near zero, Hessian sign/curvature).
6. Compute uncertainty (information matrix or bootstrap).
7. Diagnose fit with held-out likelihood and residual checks.

MLE is the default estimation backbone in statistics and ML because it is principled, usually efficient under correct specification, and broadly compatible with modern optimization.
