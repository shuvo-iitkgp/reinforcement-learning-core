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

## Policy improvement

## Policy iteration

## Value iteration

## Contraction property

## Finite vs infinite horizon policies

## Algorithm comparison 