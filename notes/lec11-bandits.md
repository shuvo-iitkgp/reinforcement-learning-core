## Multi-armed bandit formalism 

A bandit problem is defined by : 

- Action set A = {1 ... K} 

- Each arm a has an unknown reward distribution $R_a$

- Mean reward : 

$$ Q(a) = E [r | a]$$

At time t: 

- You choose $a_t$

- You observe reward $r_t \approx R_{a_t}$

Goal: 

$$ max \sum_{t=1}^T r_t$$ 

## Why greedy fails 

Greedy strategy: 

$$ \hat{Q}_t(a) = \frac{1}{N_t(a)} \sum_{i=1}^{t-1} r_i 1 (a_i = a) $$

$$ a_t = argmax_a \hat{Q}_t(a) $$

If early samples are unluck greedy can lock onto a suboptimal arm forever. 

## Regret: the correct evaluation metric

Instataneous regret: 
$\ell_t = V* - Q(a_t) $

where 

$$V* = max_a Q(a) $$

Total regret: 

$$ L_T = \sum_{t=1}^T \ell_t$$

Key identity from the lecture: 

$$ L_T = \sum_{a \neq a*} E[N_T(a)] \Delta_a$$

where $\Delta_a = Q(a*) - Q(a)$

Regret grows when you pull bad arms. The worse the arm, the more expensive the pull is . 

## Why $\epsilon$-greedy still fails

$\epsilon$- greedy: 

with probability $1-\epsilon$: greedy

with probability $\epsilon$ : random

Two failure modes: 

1. Fixed $\epsilon$>0 : you explore forever

> linear regret

2. $\epsilon = 0 $ : greedy 

> linear regret

## Lower bound: how hard the problem really is 

$lim_{T \to \infty} L_T \geq log T \sum_{a \neq a*} \frac{\Delta_a}{D_{KL}(R_a || R_{a*})}$

We can't do better than logarithmic regret in general. 

## Optimism under uncertainty

For each arm, construct an upper confidence bound: 

$$ U_t(a) \geq Q(a) $$

with high probability

## Hoeffding's inequality and confidence bounds

Hoeffding's inequality

$$ Pr (|\hat{Q}_n - Q| > u) \leq 2 \exp{-2nu^2}$$

Solve for u to get a confidence radius: 

$$ u(n, \delta) = \sqrt{\frac{\log(1/\delta)}{2n}}$$

This gives us: 

$$Q(a) \leq \hat{Q}_t(a) + \sqrt{\frac{2\log(1/\delta)}{N_t(a)}} $$

## UCB1 algorithm: 


$$ a_t = argmax_a [\hat{Q}_t(a) + \sqrt{\frac{2\log(1/\delta)}{N_t(a)}} ] $$

It rarely pulled arms have large uncertainty. Frequently pulled arms shrink their confidence intervals. 