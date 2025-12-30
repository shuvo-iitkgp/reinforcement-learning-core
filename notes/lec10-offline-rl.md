## Why imitation learning is not enough 

It cannot : 

1. Combine good parts of different trajectories

2. Extrapolate better decisions

3. Improve beyond the demonstrator

Given random level played by students, offline RL learned a policy that increased persistence by 30% beating imitation learning decisively. 

## The core difficulty : censored data

You never observe: 

$$ r(s, a') for a' \neq a$$

This creates two problems: 

1. Counterfactuals are missing

2. Generalization is required 

This is why Q-learning fails offline. 

## Offline policy evaluation: three families

### Model based evaluation

Learn : 

- Transition model $\hat{p} (s' | s, a)$

- Reward model $\hat{r} (s, a) $

Then compute: 

$$ V^\pi \approx (I - \gamma \hat{P}^\pi)^{-1} \hat{R}$$ 

### Model free evaluation

Fitted Q evaluation

$$ \hat{Q}(s, a) = r + \gamma E_{a' \approx \pi } [\hat{Q}(s', a')]$$

Train via supervised regression on dataset transitions. Key differences from DQN: 

- No exploration 

 -No max over actions

 - Policy $\pi$ is fixed. 

### Importance Sampling

Rewrite expectations under $\pi$ using data from $\pi_b$: 

$$ E_\pi [R] = E_{\pi_b} [\Pi_t \frac{\pi (a_t | s_t)}{\pi_b (a_t  |s_t)} R ]$$

Properties : 

1. Unbiased

2. No model

3. No markov assumption

Costs: 

1. Variance explodes exponentially with horizon

2. Requires full coverage : $\pi_b (a | s) > 0 $ wherever $\pi (a | s) > 0 $


## Why offline policy optimization is harder than evaluation

The optimal policy may lie outside data support. 

If there is no overlap: 

- Importance sampling fails

- Model based methods hallucinate

- Value based methods overestimate

This is exactly the same distribution shift problem. 

## The key modern idea: pessimism under uncertainty 

Algorithmic principle:

- Trust estimates where data is dense

- Penalize or ignore regions with weak support

- Optimize only within the behavior-supported policy class

This leads to Conservative Q learning, BCQ, BEAR, MBS methods. 

## Why pessimism works

With pessimism : 

- Only well supported improvements survive

- Learned policy is safely better than behavior

- Guarantees become possible even with function approximation. 