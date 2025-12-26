# reinforcement-learning-core

This repository implements core reinforcement learning algorithms and environments, starting from dynamic programming methods and extending toward model-free and policy-based approaches

## Jack's Car Rental - Dynamic Programming (Sutton & Barto, Chapter 4)

The goal is to solve a finite discounted Markov Decision Process (MDP) using policy iteration and value iteration, and to analyze how real-world constraints alter the optimal policy.

**Problem Description**: Jack manages two car rental locations. Each day:

1. Customers request cars at each location (Poisson-distributed demand).

2. Cars are returned overnight (Poisson-distributed).

3. Jack may move cars overnight between locations at a cost.

**Algorithm implemented**: Policy Evaluation, Policy Improvement, Policy Iteration, Value Iteration

**Observation**:

1. The free transfer incentivizes aggressive movement from location 1 to location 2,
   reflecting asymmetric demand.

2. The parking penalty discourages hoarding inventory and creates sharp policy boundaries
   near the threshold of 10 cars.

3. The value function saturates earlier compared to the unconstrained case.

4. The optimal policy structure changes qualitatively, not just quantitatively.

**Files**:

1. notebooks/rl_core_jack_car_rental.ipynb


## Racetrack Control with Monte Carlo, TD, and Function Approximation 

This notebook implements Exercise 5.9 (Racetrack) from Sutton & Barto, Reinforcement Learning: An Introduction.
The goal is to learn an optimal driving policy that completes a racetrack as fast as possible without leaving the track.


### Problem Setup

State: (row, col, v_row, v_col)

Position on a discrete grid

Velocity components are discrete, non-negative, < v_max, and not both zero

Action space: 9 actions
Increment velocity by (-1, 0, +1) independently in row and column

### Dynamics

Movement is determined by velocity

Collision detection uses full path traversal (not just landing cell)

Episodes terminate when the finish line is crossed

### Rewards

-1 per time step

-5 on crash

### Crash handling (important)

On crash, the agent is reset to a random start cell with small nonzero velocity

This avoids absorbing crash loops and matches the standard racetrack formulation

Stochasticity

Optional: with probability slip_prob, the car is displaced one extra cell forward or right

Training is first validated with slip_prob = 0.0, then extended to noisy dynamics

### Methods Implemented
1. Monte Carlo Control (Tabular)

First-visit MC

Epsilon-soft policy

Learns from complete episodes

High variance, slow convergence

2. TD Control (Tabular)

SARSA (on-policy)

Q-learning (off-policy)

Much faster and more stable than MC

Requires careful epsilon decay

3. Deep Q-Network (Function Approximation)

Neural network approximates Q(s, a)

Experience replay

Target network

Optional Double DQN

State encoded as normalized continuous features

### Key Implementation Lessons (Hard-Won)

These are not optional details. Getting any of them wrong breaks learning.

Direction matters

“Forward” must move toward the finish

Initial implementation incorrectly moved into walls forever

Crash handling is critical

Keeping velocity after crash causes absorbing failure states

Resetting to start is required for learnability

Finish line must be detected by path crossing

High velocity can skip over finish cells

Line traversal is mandatory

Epsilon must decay

Fixed epsilon leads to useless hovering policies

Linear decay works reliably

Start deterministic

Always train with slip_prob = 0 first

Add stochasticity only after deterministic success

