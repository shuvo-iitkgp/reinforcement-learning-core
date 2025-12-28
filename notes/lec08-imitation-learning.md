## Maximum Entropy Inverse RL 

**Principle of Maximum entropy** 

Among all distributions consistent with observed data, pick the one with the minimum entropy. 

Meaning : Do not inject unjustified assumptions; Prefer stochastic policies over brittle deterministic ones. 

**Core formulation** 

We want a tracjectory distribution: 

$$ p(\tau) \propto \exp(r_\phi (\tau)) $$

where $r_\phi (\tau) = \sum_t r_\phi (s_t)$

This is an exponential family model. 

**Learning objective** 

Maximize likelihood of expert trajectories:

$$ max_\phi \sum_{r \in D} \log p_\phi (\tau) $$

Gradient becomes: 

$$ \nabla_\phi J = E_{\tau \approx D} [\nabla_\phi r_\phi (\tau)] - E_{\tau \approx p_\phi} [\nabla_\phi r_\phi (\tau)] $$ 

Push reward up on expert states. Push reward down on states the model visits but experts do not. 

**Algorithm** 

1. Initialize reward $r_\phi$

2. Compute optimal policy for current reward

3. Compute state visitation frequencies

4. Update reward via gradient

5. Repeat

Critical assumption: Original MaxEnt IRL requires known dynamics for step 2 and 3. 

But Finn et al. 2016 removed known-dynamics assumption using policy optimization. 

## Pairwise preference modeling (Bradley-Terry) 

**Model** 

For two items i,j : 

$$ P(i > j ) = \frac{e^{r(i)}}{e^{r(i) + e^{r(j)}}}$$

This applies to action, trajectories, model outputs. 

Use cross-entropy on labeled comparisons. This gives a reward model $r_\theta(.)$.

## RL from Human Feedback (RLHF) 

Pipeline: 

1. Instruction tuning  - Supervised fine tuning on demonstrations. 

2. Reward model training - Train Bradley Terry style model on pairwise comparisons

3. Policy optimization 

- Optimize policy using RL (using PPO) 

- Reward: $r = r_{RM}(x) - \beta KL(\pi || pi_{ref})$

KL penalty prevents reward hacking and language collapse. 