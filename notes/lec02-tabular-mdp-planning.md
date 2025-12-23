## Definitions

Agent : The decision making entity  

Environment : The thing agent interacts with

Task : A complete specification of an environment

States : At each time step t, the agent receives some representation of environment's state

Action : Agent selects the action basis the states

Reward : One time step later, as a consequence of the action it receives a numerical reward. 

Policy : At each time step, the agent implements a mapping of state to probability of selecting an action basis its past experience. 

$$ \pi (a | s) = P (A_t = a | S_t = s) $$

## Markov structures

Markov property : Environment's response at t+1 will be determined only by the state and action representations at step t. 

$$ p(s' | s, a) = P(R_{t+1} = r , S_{t+1} = s' | S_t, A_t) $$

Markov Process : Only states & transitions

Markov Reward Process : Markov Chain + reward functions + discount factor $\gamma$

Markov Decision Process : Markov Reward Process + Actions 

## Return and discounting

We get rewards for each time step after t which can be denoted by $R_{t+1}$, $R_{t+2}$, $R_{t+3}$, ....

Expected return : Sum of all the rewards after time step t which we try to maximize. 

$$ G_t = R_{t+1} + R_{t+2} + R_{t+3} + ... R_{T}  $$

Discounting : The agent tries to select actions so that the sum of the discounted rewards it receives is maximized over time. In particular it chooses A_t to maximize the expected discounted reward. 


$$ G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ...  $$

where $\gamma$ is the discount rate. Range of it is $[0, 1]$

## Value Functions

Value functions : state-action pairs that estimate expected returns for the agent to be in a given state. 

$$ v_\pi (s) = E_\pi [ G_t | S_t = s] $$

Similarly, we define the value of taking action a in state s under a policy $\pi$, denoted by $q_\pi (s, a)$, as the expected return from s, taking the action a, and thereafter following a policy $\pi$: 

$$ q_\pi (s, a) = E_\pi [G_t | S_t = s, A_t = a] $$

## Bellman equations 

For any policy $\pi$ and any state s, the following consistency condition holds between the value of s and the value of its possible successor states: 

$$v_\pi (s) = E_{\pi}[G_t | S_t = s]$$
$$\implies v_\pi (s)  = E_{\pi}[ R_{t+1} + \gamma \sum_{k=0}^\inf \gamma^k R_{t+k+2} | S_t = s] $$
$$\implies v_\pi (s)  =\sum_a \pi (a | s) \sum_{s', r} p(s', r | s, a) [r + \gamma v_\pi (s')] $$

It expresses relationship between the value of a state and the values of its successor states. 

The value function $v_\pi$ is the unique solution of its Bellman equation. 

## Policy evaluation

For any given policy $\pi$ we evaluate it by the following method: 

1. Initialize an array V(s) = 0 for all s $\in$ S
2. Repeat while $\Delta \ge \theta $ (a small number)
3. For each s $\in$ S
    i. $v <- V(s) $
    ii. $V(s) = \sum_a \pi(a|s) \sum_{s', r} p(s', r | s, a) [r + \gamma V(s')]$
    iii. $\Delta <- max (\Delta, |v - V(s)|)
4. Output V $\approx v_\pi $ 

## Policy improvement

Let $\pi$ and $\pi'$ be any pair of deterministic policies such that, for all s $\in$ S, 

$$ q_\pi (s, \pi'(s)) \ge v_\pi (s)$$. 

Then the policy $\pi'$ would be as good as or better than policy $\pi$. Both are identical policies except the one current action $ \pi'(s) = a \neq \pi(s) $. A common choice for $\pi'$ is the greedy policy with respect to $v_\pi$: 

$$ \pi'(s) = argmax_{a} q_\pi (s, a)$$

Therefore, if we make a greedy decision of selecting the best action per step and follow the old policy for rest of the iterations we get a monotonic improvement and never worse solution. 

## Policy iteration

The way we find an optimal policy is called policy iteration. The algorithm is as follows: 

1. Initialize V(s) and $\pi(s) \in A(s) $ 
2. Policy Evaluation 
    i. \Delta = 0 
    ii. Repeat while \Delta > $\theta ( a positive number ) $
        For each s $\in$ S: 
            a. v <- V(s)
            b. V(s) = $\sum_{s', r} p(s', r | s, \pi(s)) [r + \gamma V(s')]$
            c. $\Delta = max(\Delta, |v - V(s)|)$
3. Policy Improvement 
    i. policy-stable = true
    ii. For each s $\in$ S: 
        a. a = $\pi(s)$
        b. $\pi(s) = argmax_a \sum_{s', r} p(s', r | s, a) [r + \gamma V(s')]$
        c. If $a \neq \pi(s) $ then policy-stable = false
    iii. If policy-stable, then stop and return v and \pi; else go to step 2

Policy iteration generates a sequence of policies whose value functions improve monotonically and terminates when the policy is greedy w.r.t. its own value function. 

## Value iteration

Policy iteration's step to evaluate policy is expensive as we iterate Bellman expectation backup until convergence. So, instead of evaluating a policy and then improving it, we do one policy evaluation sweep and immediately follow by policy improvement. 

The value iteration update is 

$$ v_{k+1} (s) = max_a E [R_{t+1} + \gamma v_k (S_{t+1}) | S_t = s, A_t = a ] $$


Algorithm is as follows : 

1. Initialize V(s) arbitarity
2. Policy Evaluation 
    i. \Delta = 0 
    ii. Repeat while \Delta > $\theta ( a positive number ) $
        For each s $\in$ S: 
            a. v <- V(s)
            b. V(s) = $ max_a \sum_{s', r} p(s', r | s, \pi(s)) [r + \gamma V(s')]$
            c. $\Delta = max(\Delta, |v - V(s)|)$
3. Output a deterministic policy, $\pi$ such that 
$$ \pi(s) = argmax_a \sum_{s', r} p(s', r| s, a) [r + \gamma V(s')]$$


Q. Why does value iteration not maintain an explicit policy ? 

A. Because the greedy policy is implicitly defined by the value function at every iteration and an explicit policy is only needed after convergence. 