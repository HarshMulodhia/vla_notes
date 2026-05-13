# Chapter 06 - Training Objectives and Optimization Pipelines

## 6.1 Training lifecycle for VLA models

A practical lifecycle is usually:
1. initialize from multimodal/text-vision prior,
2. behavior-clone on robot trajectories,
3. run targeted post-training (RL/safety/preference),
4. iterate with failure-mined data.

This lifecycle repeats; it is not one-shot.

## 6.2 Objective design

### Action prediction loss
- CE for tokenized actions,
- MSE or Gaussian NLL for continuous actions,
- hybrid losses for mixed interfaces.

### Auxiliary losses
- language-vision alignment,
- temporal consistency,
- action smoothness penalties.

The objective should reflect downstream priorities (success, stability, safety), not only single-step prediction fidelity.

## 6.3 Teacher forcing and exposure bias

Teacher forcing stabilizes training but creates rollout mismatch because the model never sees its own errors during supervision.

Mitigations:
- scheduled sampling,
- DAgger-style data aggregation,
- on-policy rollout relabeling.

Ignoring exposure bias is a major source of deployment surprise.

## 6.4 Optimization and systems engineering

Important engineering levers:
- mixed precision for throughput,
- gradient clipping for stability,
- sequence packing efficiency,
- memory-aware batch sizing,
- distributed checkpoint resilience.

Training instability in VLAs often comes from sequence-length and multimodal-batch interactions rather than optimizer choice alone.

## 6.5 Hyperparameter sensitivity

High-impact hyperparameters include:
- learning rate schedule,
- context length,
- action-loss weight,
- instruction dropout ratio,
- augmentation intensity.

Sensitivity should be tested with small but representative ablation sweeps before full-scale runs.

## 6.6 Parameter-efficient adaptation

LoRA/adapters are useful when:
- compute is limited,
- overfitting risk is high,
- multiple task-specific variants must be maintained.

But if base representation is misaligned with target embodiment, full or broader fine-tuning may still be needed.

## 6.7 Monitoring metrics during training

Track these together:
- offline action loss,
- rollout success metrics,
- safety intervention rate,
- robustness under perturbation slices,
- language-following fidelity.

Metric divergence (e.g., loss down, success flat) is a key diagnosis signal.

## 6.8 Training run postmortem template

After each major run, document:
1. dataset snapshot and schema version,
2. exact objective and weights,
3. infrastructure settings,
4. evaluation slices,
5. primary failure classes,
6. next targeted data/architecture changes.

