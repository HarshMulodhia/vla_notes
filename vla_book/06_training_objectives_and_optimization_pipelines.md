# Chapter 06 - Training Objectives and Optimization Pipelines

## 6.1 Typical training stages

1. Multimodal pretraining initialization.
2. Robot demonstration supervised fine-tuning (behavior cloning).
3. Optional post-training (RL/safety/preference tuning).

## 6.2 Core losses

- Action prediction loss (MSE, discretized CE, or hybrid).
- Auxiliary alignment losses (vision-language grounding).
- Regularization for stability (weight decay, dropout, EMA).

## 6.3 Teacher forcing and rollout mismatch

Teacher forcing helps optimization but causes exposure bias.
Common fixes:
- scheduled sampling,
- dataset aggregation,
- periodic on-policy relabeling.

## 6.4 Optimization engineering

- mixed precision,
- gradient checkpointing,
- distributed data parallelism,
- careful sequence packing and masking.

## 6.5 Parameter-efficient adaptation

LoRA/adapters are widely used to adapt large backbones to specific robots/tasks with lower compute and lower overfitting risk.

## 6.6 Debugging training

Watch these metrics together:
- action loss,
- task success on held-out simulated rollouts,
- calibration/stability indicators,
- safety-intervention frequency.

Low loss with poor success often indicates objective mismatch or action-space issues.

