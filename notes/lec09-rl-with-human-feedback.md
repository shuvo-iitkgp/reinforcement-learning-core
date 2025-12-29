## RLHF step 1: learn a reward model 

Data looks like: 

$$ (x, y_w, y_\ell)$$

Prompt, preferred response, rejected response. 

You train a reward model $r_\phi (x, y) $ by minimizing: 

$$ -log \sigma (r_\phi (x, y_w) - r_\phi (x, y_\ell)) $$

This is just binary logistic regression on reward differences. 

## RLHF step 2: optimize a policy with that reward

Now we treat the reward model as ground truth and solve : 

$$ max_\pi E[r(x, y)] - \beta D_{KL} (\pi || \pi_{ref}) $$ 

Interpretation: 

- First term: say good things according to humans

- Second term: do not drift too much from the base model . 

This KL penalty is not optional. 

This leads to instability, reward hacking, huge engineering overhead. This motivates DPO

## Key insight behind DPO

If you solve the RLHF objective exactly, the optimal policy has a closed form: 

$$ \pi* (y | x ) \propto \pi_{ref}(y | x) \exp (\frac{1}{\beta} r(x, y))$$

This means: every RLHF policy implicitly defines a reward function. Every reward function implicitly defines a policy. 

## Flip the equation: eliminate the reward model 

Rearrange the equation: 

$$ r(x, y) = \beta log \frac{\pi(y|x)}{\pi_{ref}(y|x)} + \beta log Z(x)$$

Now plug this directly into the Bradley Terry loss. 

The partition function $Z(x) $ cancels. You never compute rewards or do rollouts.  

## The DPO loss

Final DPO objective: 

$$ L_{DPO} = \log \simga (\beta [\log \frac{\pi_{\theta} (y_w | x)}{\pi_{ref} (y_w | x)} - \log \frac{\pi_{\theta} (y_\ell | x)}{\pi_{ref} (y_\ell | x)}]) $$