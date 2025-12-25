## Generalized Policy Improvement (GPI)

Policy iteration still applies conceptually : Evaluate a policy $\pi$ and improve policy using $Q_\pi$. But if $\pi$ is deterministic we only ever see one action per state. Then $Q(s, a)$ for other actions is unknowable. 

So we use a stochastic policies called $\epsilon$-greedy policies. Here $\epsilon$-greedy means: 

> With prob. $1-\epsilon$ take argmax action

> With prob. $\epsilon$ explore randomly

## Monte Carlo Control 

Steps involved are 

> Run full episodes

> Compute return $G_t$

> Use $G_t$ as an unbiased estimate $Q(s, a)$

> Improve policy using $\epsilon$-greedy approach

## GLIE (Greedy in Limit of Infinite Exploration)

Two requirements: 

1. Every (s, a) is visited infinitely often

2. The policy becomes greedy asymptotically

$\epsilon_k = 1/k $ satisfies this. 

## Temporal Difference Control 

TD bootstraps: It replaces the true return with an estimate of future value

Two main algorithm : 

1. Sarsa (on - policy) 

Update uses the action actually taken next: 


$$Q(s, a) = Q(s, a) + \alpha [r + \gamma Q(s', a') - Q(s, a) ] $$

Learns the value of the behavior policy. 

2. Q- learning (off policy) 

Update uses the greedy action: 

$$Q(s, a) = Q(s, a) + \alpha [r + \gamma max_{a'}Q(s', a') - Q(s, a) ] $$

Learns $Q*$ regardless of behavior policy. 

**Convergence of Q-learning** - Q-learning converges if : (I) GLIE exploration, (II) Learning rates satisfy Robbins-Monro conditions, and (III) Finite state and action states. 

## Why function approximation is unavoidable 

Function approximation: 

> Shares information across states

> Reduces memory

> Reduces sample complexity

But it introduces bias and instability. 

This turns RL into online supervised learning with moving targets. 

## MC vs TD with function approximation

**MC + VFA**: 

- Target = $G_t$ 
- Unbiased
- High variance
- Stable 

**TD + VFA** : 

- Target = $r + \gamma \hat{Q}(s')$
- Biased
- Lower variance
- Can diverge

## Deadly triad 

1. Function approximation

2. Bootstrapping

3. Off-policy learning

Q-learning with neural networks has all three. 

## Deep Q Networks (DQN)

It adds two stabilizers : 

1. Experience replay : 

> Breaks correlation between samples

> Improves data efficiency

2. Fixed target network 

> Makes target stationary for a while

> Prevents feedback loops

