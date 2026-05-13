# Chapter 07 - RL, Planning, and Post-Training for VLAs

## 7.1 Why post-training is necessary

Behavior cloning provides a strong initialization but often underperforms in:
- sparse-reward tasks,
- long-horizon adaptation,
- recovery from off-nominal states,
- policy robustness under perturbations.

Post-training closes these gaps.

## 7.2 RL integration patterns

### Head-only RL fine-tuning
Adjust action head while freezing most backbone.
- Efficient and stable, but limited representational correction.

### Adapter-level RL fine-tuning
Train LoRA/adapters with RL objective.
- Good compromise between flexibility and stability.

### Full-policy RL fine-tuning
Most expressive, highest risk (instability/catastrophic forgetting).

### Value-guided reranking
Generate multiple action candidates and choose with a learned value signal.
- Useful when direct policy updates are risky.

## 7.3 Reward design in VLA context

Reward can combine:
- task completion,
- progress shaping,
- smoothness/control penalties,
- safety penalties,
- language-alignment terms.

Poorly weighted reward terms can silently bias the policy away from instruction fidelity.

## 7.4 Planning with VLA priors

VLA can be integrated as:
- policy prior for search,
- high-level skill proposer,
- rollout policy within model-predictive loop.

Planning layers improve robustness for long-horizon tasks where pure reactive policies drift.

## 7.5 Preference and safety post-training

In addition to RL, post-training can include:
- human preference optimization,
- corrective demonstration incorporation,
- constraint-aware distillation.

These methods target behavior quality dimensions that raw imitation loss does not capture.

## 7.6 Failure mining and targeted adaptation

Collect failure buffers from deployment-like rollouts, then:
1. cluster failure modes,
2. prioritize highest-risk classes,
3. generate targeted corrective data,
4. retrain/evaluate with focused slices.

This loop usually yields faster gains than generic data scaling.

## 7.7 Stability and forgetting safeguards

During post-training, protect core capabilities with:
- replay of base multitask data,
- regularization toward baseline policy,
- capability regression tests.

Without safeguards, narrow improvements can degrade broader competence.

