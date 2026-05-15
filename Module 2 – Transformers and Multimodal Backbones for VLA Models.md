# Module 2 – Transformers and Multimodal Backbones for VLA Models

## 2.1 Why transformers dominate current VLAs

Transformers are the default policy class in modern VLAs because they scale naturally with sequence data, support long multimodal contexts, and can inherit strong priors from large language and vision-language pretraining. In robotics, that matters because the policy must jointly reason over instructions, visual observations, robot state, and often recent action history.

A useful shift from the deep-RL view is this: instead of a policy taking one state vector and producing one action vector, the policy now consumes a structured sequence of tokens with different semantics and time scales.

## 2.2 Canonical multimodal pipeline

Most VLA backbones can be decomposed into five blocks:
1. a vision encoder,
2. a language model or text tower,
3. optional state/proprioception encoders,
4. a fusion transformer or multimodal trunk,
5. an action decoder or action expert.

The hard problem is not only building strong blocks. It is aligning them so that grounded action-relevant information survives all the way to the control output.

## 2.3 Vision encoders in practice

Vision encoders for VLAs need more than good image classification features. They must support:
- cluttered manipulation scenes,
- small-object discrimination,
- viewpoint variation,
- multi-camera or ego/exo camera fusion,
- temporal consistency under control latency.

This is why architectural discussions about ViTs, patch size, frozen vs fine-tuned encoders, and image token count are not cosmetic details. They directly affect grounding and runtime cost.

## 2.4 Language backbones in practice

Language backbones contribute:
- instruction following,
- compositional task understanding,
- semantic disambiguation,
- reuse of web-scale world knowledge.

But the important systems question is not "does the model have an LLM?" It is how language remains available during rollout. A model that compresses the instruction too aggressively can act as if language were mere task labeling instead of an active control constraint.

## 2.5 Fusion design patterns

Common fusion choices include:
- early fusion over one long token sequence,
- late fusion after modality-specific processing,
- cross-attention bridges between modalities,
- hierarchical fusion where slow semantic context and fast control context are separated.

No pattern is universally best. Early fusion gives rich interaction but raises compute cost. Late fusion is cheaper but can miss subtle grounding dependencies. Dual-rate systems often benefit from hierarchical fusion because the slow semantic context does not need to be recomputed at every control step.

## 2.6 Temporal modeling is a first-class design choice

VLAs do not only need multimodal understanding; they need temporal competence. Common strategies include:
- frame stacking,
- explicit temporal transformers,
- recurrent or memory tokens,
- chunk-level latent states,
- decoupled slow and fast loops.

Long-horizon failures often come from weak temporal modeling rather than weak single-frame perception.

## 2.7 Modern architectural split: action head vs action expert

The awesome-vla-study flow makes a useful distinction:

### VLM plus action head
CogACT, GR00T N1, and X-VLA illustrate the case where a VLM trunk produces a strong grounded representation and a separate action head turns that representation into control outputs. This is often easier to engineer, but it can limit expressiveness if the action module only sees the last hidden state.

### VLM plus action expert
π0 and related models go further by giving a separate action expert access to richer internal features. That action expert can use diffusion or flow-matching-style generation to produce smoother trajectories and capture multimodal action structure better than a small readout head.

## 2.8 What to inspect in an architecture diagram

When reading a paper figure, annotate:
- which tokens are visual, linguistic, proprioceptive, and action-related,
- whether the model is decoder-only, encoder-decoder, or hybrid,
- where time enters the model,
- where embodiment conditioning enters,
- whether actions are generated directly or via a separate generative module.

If you cannot answer those five questions from the diagram and text, you do not yet understand the architecture.

## 2.9 Architecture trends that matter for deployment

Recent systems highlight three deployment-relevant trends:
1. **cross-embodiment conditioning** through robot descriptors or prompts,
2. **decoupled action generation** for smoother high-frequency control,
3. **hierarchical runtime structure** so that heavy multimodal reasoning does not bottleneck every actuator update.

These trends connect architecture design directly to the later topics of dual-system execution and efficient inference.

## 2.10 Common architecture failure modes

Architecture failures often present as:
- language being ignored,
- visual confusion between similar objects,
- oscillatory or overly cautious action outputs,
- collapsed cross-robot transfer,
- context-window saturation that hurts response time.

These are rarely solved by one more training epoch. They usually indicate that modality fusion, temporal structure, or action decoding is mismatched to the target behavior.

## 2.11 Questions to carry into the paper-reading weeks

- Why did this paper choose an action head instead of an action expert?
- What information is visible to the action module?
- How expensive is the trunk relative to the control frequency required in deployment?
- Does the architecture cleanly separate semantic reasoning from motor execution, or does it force one loop to do both?
