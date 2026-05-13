# Chapter 03 - Multimodal Architectures for VLAs

## 3.1 Canonical pipeline

1. **Vision encoder** (e.g., ViT/CNN) converts image frames to tokens.
2. **Language tokenizer + text backbone** encodes instruction/context.
3. **State encoder** projects proprioception to token space.
4. **Fusion + policy core** (often transformer decoder).
5. **Action head/token decoder** produces control outputs.

## 3.2 Fusion strategies

- Early fusion: combine modality tokens quickly; rich interactions but expensive.
- Late fusion: modality-specific pathways then merge; cheaper but weaker alignment.
- Cross-attention bridges: text as query over vision/state tokens (or vice versa).

## 3.3 Transformer design choices

- Decoder-only vs encoder-decoder.
- Causal masking patterns.
- Positional encodings across space/time/modalities.
- Memory windows for long-horizon behavior.

## 3.4 Multi-frame and temporal encoding

Robot control depends on motion, not static frames alone.
Common approaches:
- frame stacking,
- temporal transformer blocks,
- latent recurrence for memory.

## 3.5 Embodiment conditioning

For cross-robot generalization, models may include:
- embodiment ID tokens,
- kinematic metadata tokens,
- normalized action interfaces.

## 3.6 Practical architecture trade-offs

- Larger model: better transfer, worse latency.
- Richer vision backbone: stronger grounding, more compute.
- Unified action-text vocabulary: easier autoregression, harder precision control.

