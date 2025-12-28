## N-step Advantage estimators

Define TD error: 

$$ \delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)$$

k-step advantage: 

$$ \hat{A_t}^{(k)} = \sum_{l=0}^{k-1} \gamma^l \delta_{t+1} $$

## Generalized Advantage Estimation (GAE) 

GAE is a weighted average of all k-step estimators

Definition: 

$$ \hat{A_t}^{GAE(\gamma, \lambda)} = \sum_{l=0}^\infty (\gamma \lambda)^l \delta_{t+l}$$ 

Role of $\lambda : 

- $\lambda$ = 0 means TD(0) , high bias

$$ GAE(\gamma, 0) : \hat{A_t}:= \delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)$$

- $\lambda$ = 1 means Monte Carlo, high variance. 

$$ GAE(\gamma, 1) : \hat{A_t}:= \sum_{l=0}^\infty \gamma^l \delta_{t+l} = \sum_{l=0}^\infty r_{t+l} - V(s_t)$$ 

Why GAE matters: 

- Much smoother learning

- essential for ppo stability 

- almost universally used in practice

PPO uses truncated GAE since trajectories are finite. 

## Monotonic Improvement Theory (Why PPO works) 

Key bound 

$$ J(\pi') \geq J(\pi) + L_\pi(\pi') - C E[D_{KL} (\pi' || pi)]$$

where $L_\pi(pi')$ is the surrogate objective and KL penalty prevents large policy shifts

If we maximize the RHS performance cannot decrease. 

True constanc C is very conservative. PPO relaxes this using clipping or adaptive KL. 