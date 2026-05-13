# Chapter 01 - VLA Foundations and Taxonomy

## 1.1 What is a VLA model?

A Vision-Language-Action model is a multimodal policy that predicts robot actions conditioned on perception and language context:
\[
\pi_\theta(a_t \mid o_{\le t}, g, s_{\le t})
\]
where:
- \(o_t\): visual observations (frames, depth, scene crops, etc.),
- \(g\): instruction/goal language,
- \(s_t\): optional proprioceptive state,
- \(a_t\): low-level or high-level action output.

The central idea is that action is treated as a first-class output modality, not a downstream module detached from language and perception.

## 1.2 VLA vs VLM vs classical stacks

- **VLM**: aligned vision-text representations, usually text outputs.
- **VLA**: aligned vision-text representations plus executable action outputs.
- **Classical stack**: perception, planning, and control optimized separately.

In practice, VLA systems often combine both worlds: learned multimodal policy + classical control/safety wrappers.

## 1.3 Taxonomy axis A: stack integration

### End-to-end VLA
Single policy predicts actions directly from multimodal context.
- Strength: compact interface, strong representation coupling.
- Weakness: interpretability and strict guarantees are harder.

### Planner-controller decomposition
A high-level model outputs subgoals/skills, low-level controller executes.
- Strength: better control guarantees and debuggability.
- Weakness: interface mismatch and error handoff risk.

### Hybrid systems
Use VLA for perception-grounded intent + classical optimizer/controller for feasibility.
- Common in production where reliability matters more than elegance.

## 1.4 Taxonomy axis B: action granularity

- **Primitive control** (torque/velocity/delta pose): high precision, high sensitivity.
- **Mid-level control** (waypoints/chunks): better temporal stability.
- **Skill-level actions**: compositional and interpretable but coarse.

Granularity changes which failures dominate (precision vs planning fidelity).

## 1.5 Taxonomy axis C: action representation

- Continuous regression heads.
- Discrete/tokenized actions.
- Latent codebook/diffusion-style decoders.

This choice impacts sequence length, precision, optimization stability, and real-time inference cost.

## 1.6 Taxonomy axis D: training regime

- Pure behavior cloning.
- BC with online corrections (DAgger-like).
- BC + RL post-training.
- BC + preference/safety alignment.

These are not mutually exclusive; mature systems chain them over time.

## 1.7 Taxonomy axis E: embodiment scope

- Single robot embodiment.
- Family-level transfer (similar kinematics).
- Cross-embodiment generalist policy.

Cross-embodiment performance usually depends on explicit embodiment conditioning and normalized action interface design.

## 1.8 Taxonomy axis F: reasoning style

- Reactive mapping from context to action.
- Deliberative style using intermediate plans or structured latent reasoning.

Reasoning-style policies may improve long-horizon behavior but add latency and failure surfaces.

## 1.9 Why taxonomy matters operationally

Taxonomy is a decision tool. It helps you predict:
- data needs,
- compute needs,
- deployment risk,
- expected generalization behavior,
- debugging complexity.

If two systems have similar benchmark scores but different taxonomy positions, they may behave very differently under real-world shift.

## 1.10 Quick classification template

When reading a VLA paper/repo, always answer:
1. What are the exact inputs and outputs?
2. Which action representation is used?
3. Is execution closed-loop and at what frequency?
4. What post-training is used beyond BC?
5. What safety and fallback assumptions are made?

