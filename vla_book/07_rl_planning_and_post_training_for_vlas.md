# Chapter 07 - RL, Planning, and Post-Training for VLAs

## 7.1 Why RL is still useful after imitation

Imitation gives a strong prior but may not optimize:
- task completion under distribution shift,
- robustness to perturbations,
- long-horizon credit assignment.

RL fine-tuning can improve these aspects.

## 7.2 RL integration patterns

- Fine-tune action head only.
- Fine-tune adapters while freezing backbone.
- Train value/reward heads for reranking action candidates.
- Use model-predictive outer loops around VLA proposals.

## 7.3 Reward design in VLA era

Classical dense reward shaping is still relevant, but now often combined with:
- language-conditioned rewards,
- preference models,
- safety penalties.

## 7.4 Planning with VLA priors

Two common forms:
- VLA as policy prior inside search/planning.
- VLA as high-level skill proposer for classical low-level planners.

## 7.5 Failure-aware post-training

Targeted hard-case tuning on collected failure episodes can dramatically improve deployment robustness.

