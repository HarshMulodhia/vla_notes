# Chapter 01 - VLA Foundations and Taxonomy

## 1.1 What is a VLA model?

A Vision-Language-Action (VLA) model is a policy model that maps:
- visual observations,
- language instructions/context,
- optional robot state,
into robot actions.

In compact form:
\[
\pi_\theta(a_t \mid o_{1:t}, g, s_{1:t})
\]
where:
- \(o_t\): visual observation (image/video tokens),
- \(g\): language goal/instruction,
- \(s_t\): optional proprioception,
- \(a_t\): action output.

## 1.2 VLA vs VLM vs classical robotics stacks

- **VLM**: image + language in, text out.
- **VLA**: image + language (+ state) in, action out.
- **Classical stack**: separate perception/planner/controller modules with explicit interfaces.

VLAs collapse more of the stack into one learned model.

## 1.3 Taxonomy axes

### A. Architecture integration
- Planner-controller decomposition
- End-to-end policy
- Hybrid (high-level LM + low-level controller)

### B. Action representation
- Continuous action head
- Discrete tokenized actions
- Latent/action-codebook actions

### C. Task and embodiment scope
- Single-task vs multitask
- Single-embodiment vs cross-embodiment

### D. Training paradigm
- Pure behavior cloning
- BC + RL fine-tuning
- BC + preference/safety post-training

### E. Reasoning style
- Reactive (direct mapping)
- Deliberative (intermediate plan/reasoning tokens)

## 1.4 Why taxonomy matters

Taxonomy helps you quickly predict:
- expected data volume,
- runtime latency,
- stability during deployment,
- safety properties,
- transfer/generalization behavior.

