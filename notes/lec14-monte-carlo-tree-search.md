## Simple Monte Carlo Search 

Core idea: 

1. Try each action

2. Simulate futures

3. Pick action with highest average return 

Procedure: 

1. From current state $s_t$. 

2. For each action $a \in A$

3. Run $K$ simulated episodes using some rollout policy $\pi$

4. Estimate: 

$$ Q(s_t ,a) = \frac{1}{K} \sum_{k=1}^K G_k$$

5. Choose: 

$$ a_t = argmax_a Q(s_t, a)$$

## Expectimax Tree Search 

Explicitly builds a search tree

Alternates between 

- Max nodes for agent actions

- Chance nodes for stochastic transitions

At depth $H$ tree size is : 

$$O(|S|.|A|)^H$$

This explodes fast. 

## Monte Carlo Tree Search 

MCTS combines: 

- Tree search 

- Monte Carlo rollouts

- Incremental value estimation

Instead of fully expanding the tree, you: 

- Grow it selectively

- Spend more computation on promising branches

Each node stores: 

- N(s, a) : visit count

- Q(s, a) : mean return estimate

High - level loop : 

1. Selection

2. Expansion

3. Simulation

4. Backpropagation

This repeats thousands or millions of times. 

## The key problem: which action to simulate next? 

This is where Upper Confidence Tree comes in. Each node behaves like a multi armed bandit. 

At a state s, each action a is an arm. 

We want: 

- Exploit actions with high Q(s, a)

- Explore actions with low visits

UCT selection rule :

$$ a = argmax_a [Q(s, a) + c \sqrt{\frac{log N(s)}{N(s, a)}}]$$

First term: exploitation

Second term: exploration bonus

c: controls aggressiveness of exploration

## Self play for Go 

Self play solves a subtle RL problem of no labeled data and no expert demonstrations. 

Idea is agent plays itself. 

Why rewards becomes 'dense' : Opponents are evenly matched. Games are not trivial blowouts.


This is implicit curriculum learning. As the agent improves the task difficulty increases automatically. 

Neural network outpus Policy prior, value estimate. 

Modified UCT: 

$$ U(s, a) \propto \frac{P(s, a)}{1 + N(s, a)}$$

Effect: 

- High prior actions explored earlier

- Exploration decays faster

