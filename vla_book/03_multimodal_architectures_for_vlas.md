# Chapter 03 - Multimodal Architectures for VLAs

## 3.1 Canonical architecture blocks

A practical VLA model usually has:
1. vision encoder,
2. language encoder/tokenizer,
3. state encoder,
4. fusion policy core,
5. action decoder/head.

The key challenge is not only each block quality, but modality alignment under temporal constraints.

## 3.2 Vision encoders in VLA

Consider these dimensions:
- spatial fidelity (small object handling),
- temporal encoding (motion cues),
- robustness to lighting/camera shift,
- compute cost at control frequency.

Even strong image encoders can fail in robotics if they are not adapted to egocentric and cluttered manipulation scenes.

## 3.3 Language conditioning design

Language is used for:
- high-level intent,
- constraints (“avoid touching X”),
- disambiguation (“left red block”).

Good policies keep language context persistent through rollout rather than compressing it once and discarding nuance.

## 3.4 State/proprioception integration

Proprioception often carries fast-changing control-relevant information that vision cannot recover quickly enough.

Design considerations:
- token normalization and scaling,
- update rate alignment with vision,
- sensor noise filtering.

## 3.5 Fusion strategies

### Early fusion
Strong cross-modal interactions but expensive and harder to stabilize.

### Late fusion
Cheaper and modular but may underutilize cross-modal reasoning.

### Cross-attention bridges
Flexible compromise: one modality queries another selectively.

No fusion strategy is universally best; match it to latency and task complexity constraints.

## 3.6 Temporal modeling options

- frame stacking,
- explicit temporal transformer layers,
- recurrent memory tokens,
- chunk-level latent state propagation.

Long-horizon tasks typically require more than naive frame stacking.

## 3.7 Action decoder design patterns

- direct regression head,
- autoregressive action tokenizer,
- latent-action decoder.

Decoder choice should co-design with Chapter 04 action interface, not be decided independently.

## 3.8 Embodiment conditioning for transfer

Cross-embodiment models often require explicit conditioning:
- embodiment ID tokens,
- kinematic descriptors,
- action-space normalization.

Without this, the model conflates morphology differences and transfer quality collapses.

## 3.9 Architecture debugging signals

Typical early warnings:
- language ignored (instruction-invariant actions),
- visual grounding drift (wrong object selection),
- oscillatory control outputs,
- context-window saturation.

These symptoms should be tied to architecture-level ablations, not only training hyperparameter tuning.

