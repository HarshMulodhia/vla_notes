# Chapter 04 - Action Spaces, Tokenization, and Control Interfaces

## 4.1 Why action interface is the core design decision

In VLA systems, action definition determines:
- what the model is expected to learn,
- what safety layer can enforce,
- what runtime latency is acceptable,
- how transferable policy behavior is across robots.

Poor action design cannot be fixed later by bigger models alone.

## 4.2 Continuous action outputs

Common outputs:
- joint velocity/position deltas,
- end-effector pose deltas,
- gripper state.

Advantages:
- high control fidelity,
- direct compatibility with classical controllers.

Risks:
- sensitivity to normalization and scaling,
- unstable outputs under distribution shift,
- harder integration with token-pretrained backbones.

## 4.3 Tokenized/discrete action outputs

Action tokenization maps continuous control to bins/codebooks.

Advantages:
- aligns with language-model decoding machinery,
- supports sequence-level autoregressive reasoning.

Risks:
- quantization error,
- long token sequences for high-DoF control,
- discretization design becoming a hidden bottleneck.

## 4.4 Chunked and latent action parameterization

Predicting chunks or latent plans can improve smoothness and throughput.

Trade-off:
- larger chunk: lower compute and better smoothness,
- but reduced responsiveness to sudden environment changes.

Chunk size should be tuned jointly with control-loop frequency.

## 4.5 Interface with low-level controllers

A robust pipeline typically does:
1. VLA predicts high/mid-level command,
2. controller tracks command under dynamics constraints,
3. safety layer validates command feasibility.

This separation helps combine learning flexibility with control guarantees.

## 4.6 Calibration, clipping, and units

Many real failures arise from unit mismatch and uncalibrated ranges.

Minimum checks:
- explicit units for each action dimension,
- consistent scaling in train and inference,
- hard clipping ranges,
- acceleration/jerk limits.

## 4.7 Multi-robot portability

For portability, define a normalized abstract action interface where possible (for example end-effector delta in task frame), then map to robot-specific controllers.

Direct joint-space outputs may perform best per-robot but transfer poorly across embodiments.

## 4.8 Failure patterns by action interface

- Continuous: jitter, overshoot, instability near contacts.
- Tokenized: staircase trajectories, coarse placement errors.
- Chunked: delayed corrections and lagging recovery.

These patterns guide which representation to revise first.

## 4.9 Action-design checklist

Before finalizing the interface, answer:
1. Which safety constraints can be enforced downstream?
2. Is control frequency achievable on target hardware?
3. How much precision does the task truly need?
4. Is cross-embodiment transfer a requirement?

