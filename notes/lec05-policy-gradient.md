## Why policy gradients exist at all

Value based RL learns a value function first then derives a policy from it, usually via epsilon-greedy. That works but it has hard limits. There are problems where: 

1. The state is aliased or partially observable. 

2. The action space is continous

3. The optimal policy must be stocastic. 

Policy based RL skips the value to policy step and directly optimizes the policy. 

## What policy-based RL actually does

You parameterize the policy directly: 

$\pi_\theta(a|s)$  = prob. of taking action in state s. 

Maximize expected return. 

## The core idea of policy gradients

You want to move $\theta$ in the direction that increases the expected return. 

$$ \theta = \theta + \alpha \Delta \theta V(\theta) $$

## Trajectory formulation (this is the key) 

$$ V(\theta) = \sum_\tau P(\tau | \theta) R (\tau) $$

$P(\tau | \theta) $ dependes on the policy but the reward does not. 

## Likelihood ration trick 

$$ \Delta \theta V(\theta) = \sum_\tau P(\tau | \theta) R (\tau) \Delta \theta P(\tau | \theta) $$

This step is what allows policy gradients to work without differentiating rewards or environment dynamics. 

## Why dynamics diasppear

Dynamics terms vanish because they do not depend on $\theta$. Only policy terms remain. 

$$ \Delta \theta P(\tau | \theta) = \sum_t \Delta \theta \log \pi_\theta (a_t | s_t) $$

This is the reason policy gradients are model-free. 

## Monte Carlo policy gradient estimator

Using sampled trajectories: 

$$ \theta V(\theta) \approx (1/m) \sum_i R(\tau_i) \sum_t \Delta \theta \log \pi_\theta (a_t | s_t)$$

The estimator is unbiased and extremely noisy. 

## Temporal structure improvement 

An action at time t cannot affect rewards before time t. 

So instead of weighting all actions by total return, weight them by future return: 

$$ G_t = r_t + r_{t+1} + ... + r_T$$

Now the estimator becomes : 

$$ \theta V(\theta) \approx \sum_t \Delta \theta \log \pi_\theta (a_t | s_t) G_t $$

This is **REINFORCE**

## REINFORCE algorithm 

For each episode: 

> Run the policy

> For each timestep: 

(i) Compute return $G_t$

(ii) Update $\theta$ using $\Delta \theta log \pi_\theta (a_t | s_t) G_t$

## Baselines: variance reduction without bias

We subtract any baseline $b(s_t)$

$$ \theta log \pi_\theta (a_t | s_t) (G_t - b(s_t))$$

Intuition: 

(i) Increases prob of actions that did better than expected. 

(ii) Decrease prob of actions that did worse. 

The best baseline is the state value function $V(s_t)$ 

## Advantage function

Define advantage : 

$$ A_\pi(s, a) = Q_\pi(s, a) - V_\pi (s) $$

Policy gradient becomes : 

$$ \theta V(\theta) = E[\Delta \theta log \pi_\theta (a_t | s_t) A_\pi(s, a)]$$

This is policy gradient theorem. 

## Actor-critic connection

Actor :the policy $\pi_\theta$. 

Critic : a value function $V(s)$

The critic provides low-variance estimates of advantage. The actor uses them to update the policy. 