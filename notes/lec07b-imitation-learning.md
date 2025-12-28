## Why imitation learning exists

1. Reward is hard to specify

2. Expert behavior exists

3. RL from scratch is too slow

Imitation learning uses expert demonstrations instead of rewards. 

## Behavior Cloning

Approach : 

- Treat as supervised data

- Train policy $\pi(a | s) $ using classification / regression

## Compounding Error Problem

Key issue: 

- During training : states come from expert distribution

- During execution : states come from learned policy

Small error at time t leads to unseen states later. 

Expected total error : $O( \epsilon T ^2 ) $

## [DAGGER (Dataset Aggregation)](https://www.cs.cmu.edu/~sross1/publications/Ross-AIStats11-NoRegret.pdf)

Fixes distribution mismatch: 

Algorithm: 

1. Start with expert policy

2. Roll out current policy

3. Query expert for correct actions on visited states

4. Aggregate dataset

5. Retrain policy 

Outcome: 

1. Policy is trained on its own induces state distribution

2. Error grows linearly, not quadratically 

Key limitation: 

1. Requires expert access during training

2. Expensive for human experts 

## Why Inverse Reinforcement Learning is needed  ?

Behavior cloning copies behavior but does not infer intent. 

IRL tries to infer: 

> What reward function makes the expert optimal ? 

There is no unique reward function. 

## Linear Feature Based IRL 

Assume : $R(s) = w^T x(s)$

Define feature expections: 

$$ \mu (\pi) = E[\sum_t \gamma_t x(s_t)]$$ 

Value: 

$$ V^\pi = w^T \mu(\pi) $$

Goal: Find w such that: 

$$ w^T \mu(\pi*) \geq w^T \mu(\pi) \forall \pi $$

## Feature Matching 

Key theorem: 

If a policy matches expert feature expectation, it performs nearly as well for any bounded reward. 

Formal: 

$$ || \mu(\pi) - \mu(pi*) ||_1 \leq \epsilon \implies |V^\pi - V^\pi^*| \leq \epsilon $$

This avoids explicitly identifying w. 


## Ambiguity in RL

Facts: 

- Infinite reward functions explain expert behavior.

- Infinite policies can match feature counts. 

Resolution: 

- Maximum Entropy IRL chooses the least committed explanation.

- GAIL frames imitation as adversarial learning. 