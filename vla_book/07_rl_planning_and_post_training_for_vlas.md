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

## 7.8 Anchor papers for modern post-training

Recent work provides a clearer map of post-training than older VLA notes usually include. HIL-SERL highlights human-in-the-loop real-world RL. SimpleVLA-RL makes the case for outcome-based RL on autoregressive VLAs. π*0.6 shows how stronger RL fine-tuning can improve flow-based or trajectory-generating policies. Together, these papers show that post-training is becoming a standard stage rather than an optional extension.

## 7.9 Post-training now overlaps with reasoning and world modeling

The boundary between RL, planning, and reasoning is getting blurrier. ThinkAct-style systems use RL to ground internal plans in task success. UniVLA-style systems inject predictive world knowledge into the training objective. As a result, future VLA stacks may treat post-training not only as reward optimization, but as a way to shape internal reasoning and foresight.

## Deepening resources

- HIL-SERL (https://arxiv.org/abs/2410.21845)
- SimpleVLA-RL (https://arxiv.org/abs/2509.09674)
- UC Berkeley CS285 (http://rail.eecs.berkeley.edu/deeprlcourse/)

For a broader reading index across all chapters, see Chapter 13.
