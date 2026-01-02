## Bayesian bandits 

The reward parameter itself is random. We start with a prior. Learning is posterior updating. 

For each arm $a$: 

- Unknown mean $\theta_a$

- Prior $p(\theta_a)$

- After data $D_t$, posterior $p(\theta_a | D_t)$

The difference is philosophical and mathematical: 

- We optimize expected performance under the prior

- Not worst-case performance over all environments

## Bayesian regret

Bayesian regret: 

$$ BayesRegret(T) = E_{\theta \approx prior} [\sum_{t=1}^T (Q_\theta(a*) - Q_\theta(a_t))]$$

Expectation is over: 

1. The random environment drawn from the prior

2. The algorithm's randomness

## Probably Approximately Correct (PAC) for bandits

PAC is different evaluation framework. 

Find $\hat{a}$ such that 

$$ Pr(Q(\hat{a}) \geq Q(a*) - \epsilon ) \geq 1 - \delta $$

PAC focuses on : 

- Sample complexity

- Stopping early 

- Identification

Regret focuses on: 

- Cumulative performance 

- Online decision making 

## Thompson sampling 

At time t: 

1. For each arm a, sample

$$ \tilda{\theta_a} \approx p(\theta_a | D_t) $$

2. Choose 

$$ \a_t = argmax_a \tilda{\theta}_a $$

## Why Thompson sampling works

Two properties come for free: 

1. Exploration - If an arm is uncertain, its posterior has high variance. It will sometimes sample high values.

2. Exploitation - As evidence accumulates the posterior concentrates. Bad arms stop winning samples.

## Bayesian bandits as optimal control 

A Bayesian bandit is a fully observable MDP where: 

- State = posterior distribution

- Action = which arm to pull

- Transition = Bayesian update

- Reward = expected reward under posterior

This is called the belief MDP. 

## Gittins index

Under a specific condition: 

- Discounted rewards

- Independent arms

- Geometric discount factor $\gamma$

There exists an optimal policy

That policy is : 

- Compute an index $G(a)$ for each arm

- Pull the arm with the highest index

- The index depends only on that arm's posterior 

This is the Gittins index theorem. 





