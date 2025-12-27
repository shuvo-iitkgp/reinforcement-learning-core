## Likelihood Ratio 

Objective : Maxmize expected return : 

$$ J(\theta) = E_{\tau \approx \pi_\theta} [\sum_{t=0} \gamma^t r_t]$$

Core Result (policy gradient theorem) : 

$$ \nabla\theta J(\theta) = E_\pi_\theta [\sum_{t=0}^{T-1} \nabla_\theta \log \pi_\theta (a_t | s_t) Q^{\pi_theta} (s_t, a_t)]$$

Replace Q with sampled return $G_t  = \sum_{t' = t}^{T-1} r_t'

This is unbiased and has extremely variance. 

## Baselines: Variance reduction without bias 

Subtract any function of state $ b(s_t)$: 

$$ \nabla\theta J(\theta) = E_\pi_\theta [\sum_{t=0}^{T-1} \nabla_\theta \log \pi_\theta (a_t | s_t) (G_t - b(s_t))]$$

Weighted least-squares solution gives : 

$$ b(s) \approx E[G_t | s_t = s] $$ 

which is exactly the value function. 

## Advantage methods 

Define : 

$$ A^\pi (s, a) = Q^\pi (s, a) - V^\pi (s) $$

Policy gradient becomes: 

$$ \nabla_\theta J(\theta) = E [\sum_t \nabla_\theta \log \pi(a_t | s_t) A^\pi (s_t, a_t)]$$

Interpretation: 

1. Increase prob. of actions that performed better than expected and decrease prob. of actions that underperformed. 


## Actor critic methods 

Structure: 

1. Actor : policy $\pi_\theta(a | s) $ 

2. Critic: value estimator $V_w(s)$

Updates : 

Actor :

$$ \theta = \theta + \alpha \nabla_\theta \log \pi(a_t | s_t) \hat{A_t}$$

Critic: 

$$ w = argmin_w || V_w (s_t) - \hat{R_t}||^2$$

This replaces monte carlo returns with learned value estimates. 

Result: 

1. lower variance

2. some bias 

3. much better learning

Famous method of actor critic is [A3C](https://arxiv.org/pdf/1602.01783)

## N-Step and Bootstrapped Advantage Estimators

One step TD advantage 

$$ A_t^{(1)} = r_t + \gamma V(s_{t+1}) - V(s_t) $$

Low variance and high bias

Monte Carlo advantage 

$$ A_t^{(\infty)} = G_t - V(s_t) $$ 

High variance and low bias

## Fundamental problem with vanilla policy gradient 

1. **Sample inefficiency** 

> On-policy 

> Discards data after one gradient step 

2. **Parameter space is not policy space**

> Small parameter change can radically change the policy

> Step size is meaningless without policy space control 

## Policy Performance Difference Lemma

For any old policy $\pi$ and new policy $\pi'$: 

$$ J(\pi') - J(\pi) = \frac{1}{1-\gamma} E_{s\approx d^\pi' ; a \approx pi'} [A^\pi (s, a)]$$

where $d^\pi(s) = (1-\gamma) \sum_{t=0}^\infty \gamma^t P(s_t = s | \pi) $

This expresses improvement using advantages of the old policy. 

## Surrogate Objective via Approximation

Approximate state distributions : 

$$ d_\pi' \approx d_\pi $$ 

Define surrogate objective: 

$$ L_\pi (\pi') = \frac{1}{1-\gamma} E_{s\approx d^\pi' ; a \approx pi'} [\frac{\pi'(a | s)}{\pi (a | s)} A^\pi (s, a)] $$

This uses old data, uses importance sampling, is accurate only when policies are close. 

## Why KL Divergence Matters

Performance bound: 

$$ |J(\pi') - J(\pi) - L_\pi (\pi') | \leq C \cdot E_{s\approx d_\pi} [D_{KL} (\pi' || \pi)]$$

If KL divergence is small, surrogate objective is reliable. Large policy updates break everything. 

## Proximal Policy Optimization 

**PPO with Adaptive KL Penalty** 

Policy update solves unconstrained optimization problem: 

$$ \theta_{k+1} = argmax_\theta L_{\theta_k} (\theta) - \beta_k D_{KL} (\pi_\theta || \pi_\theta_k) $$ 

Algorithm : 

1. Measure KL after update

2. Increase penalty if KL too large

3. Decrease penalty if KL too small

Works but requires tuning and more complex. 

## PPO with Clipped Objective 

Define probability ratio: 

$$ r_t(\theta) = \frac{\pi_\theta(a_t | s_t)}{\pi_theta_k (a_t| s_t)}$$

clipped objective is 

$$ L^{CLIP}  = E[min(r_t A_t , clip (r_t, 1 - \epsilon, 1+ \epsilon) A_t)] $$

what clipping does: 

1. prevents overly large policy updates

2. enforces conservative improvement

3. acts as pessimistic lower bound. 

