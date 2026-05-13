# Chapter 04 - Action Spaces, Tokenization, and Control Interfaces

## 4.1 Action space design

You must define what the model predicts:
- joint torques,
- joint velocities,
- joint positions,
- end-effector deltas,
- skill-level macro actions.

The right choice depends on robot dynamics, controller availability, and safety guarantees.

## 4.2 Continuous actions

Pros:
- high precision,
- natural for control theory interfaces.

Cons:
- harder to align with token-pretrained LLM-style backbones,
- sensitive to scale/normalization.

## 4.3 Discrete/tokenized actions

Pros:
- aligns with autoregressive token generation,
- can reuse language-model training machinery.

Cons:
- quantization error,
- long sequences for high-DoF control,
- token design complexity.

## 4.4 Chunked and latent actions

Instead of one-step actions, predict action chunks or latent plans then decode to controls.
Benefits:
- reduced decoding overhead,
- smoother execution,
- better long-horizon consistency.

## 4.5 Safety-aware action interfaces

In practice, VLA outputs should flow through a safety envelope:
- joint and workspace limits,
- velocity/acceleration clipping,
- collision checks,
- emergency stop triggers.

This keeps learned policy errors from becoming unsafe commands.

