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
