# Module 1 – Core Definition and Taxonomy of Vision-Language-Action Models (VLA)

## 1.1 A precise working definition

A vision-language-action model is a policy that conditions on visual observations, language instructions, and often robot state, then outputs actions suitable for execution on a real or simulated robot. In formal terms, it is a multimodal policy
\[
\pi_\theta(a_t \mid o_{\le t}, g, s_{\le t})
\]
where the parameters are usually initialized from a large vision-language backbone and then adapted to robot data.

The defining property is not just that the model sees images and text. It is that **action is a native output modality** rather than a separate hand-built downstream controller interface.

## 1.2 VLA vs VLM vs classical robotics stack

- **VLM**: understands and generates over vision and language, but does not directly close the control loop.
- **VLA**: extends that multimodal backbone into an executable robot policy.
- **Classical stack**: keeps perception, planning, and control as distinct engineered modules.

Many real systems are hybrids. A practical robot may use a VLA for grounded action proposals and still rely on classical controllers, collision checking, or scripted recovery primitives.

## 1.3 Taxonomy axis A - stack integration

### End-to-end VLAs
A single model maps multimodal context directly to actions.

### Planner-controller systems
A high-level model proposes subgoals, skills, or plans, and another module executes them.

### Hybrid systems
A learned policy handles semantic grounding and intent, while classical components enforce feasibility or fast low-level tracking.

This axis is central because it predicts debugging difficulty, interpretability, and how much runtime infrastructure remains outside the learned model.

## 1.4 Taxonomy axis B - action granularity

- **Low-level actions**: torques, joint velocities, end-effector deltas.
- **Mid-level actions**: short waypoint sequences or action chunks.
- **High-level actions**: skills, options, code calls, or language plans.

The right abstraction level depends on the task horizon, controller quality, and how much embodiment transfer you need.

## 1.5 Taxonomy axis C - action representation

- continuous regression heads,
- per-dimension discrete bins,
- codebook or vector-quantized actions,
- action chunks over short horizons,
- diffusion or flow-matching trajectory decoders,
- compressed tokenizations such as FAST.

This axis heavily affects sequence length, control precision, inference speed, and compatibility with pretrained LLM-style decoding.

## 1.6 Taxonomy axis D - training regime

- offline behavior cloning only,
- imitation plus corrective aggregation,
- imitation plus RL fine-tuning,
- preference or safety post-training,
- hybrid world-model or reasoning-augmented training.

A mature system often moves through several of these stages rather than picking only one.

## 1.7 Taxonomy axis E - embodiment scope

- single robot,
- robot family,
- explicitly conditioned multi-embodiment generalist policy.

Cross-embodiment results are only meaningful if you also understand the embodiment tokens, normalization scheme, and control interface used across robots.

## 1.8 Taxonomy axis F - reasoning style

- **reactive** policies act directly from context,
- **explicit reasoning** policies expose planning or chain-of-thought-like structure,
- **latent reasoning** policies compress that structure into hidden planning states.

This axis became more important with CoT-VLA, ThinkAct, and Fast-ThinkAct because reasoning is no longer assumed to mean long textual traces.

## 1.9 Anchor papers for the taxonomy

Use these papers as mental landmarks:
- **RT-1**: large-scale robot transformer without the full VLM-to-VLA framing.
- **RT-2**: makes the VLM-to-action transition explicit.
- **Octo**: open-source generalist policy with strong modular design.
- **OpenVLA**: open-source VLM-based VLA reference.
- **BeT / Diffusion Policy / ACT**: three important action-modeling families.
- **CogACT / GR00T N1 / X-VLA**: VLM plus action-head family.
- **π0 / InternVLA-M1**: VLM plus action-expert family.
- **ThinkAct / Fast-ThinkAct**: reasoning-aware extensions.

## 1.10 Why taxonomy is operational, not cosmetic

Taxonomy tells you what failure modes to expect.

For example:
- end-to-end, low-level, tokenized systems often fight sequence length and precision,
- planner-controller systems often fail at interface mismatch,
- multi-embodiment systems often fail at normalization and conditioning,
- reasoning-heavy systems often pay a latency tax unless planning is compressed.

## 1.11 A fast classification template

For every VLA, write down:
1. inputs and outputs,
2. action abstraction level,
3. action representation,
4. embodiment scope,
5. training stages,
6. runtime control frequency,
7. safety/fallback assumptions,
8. whether the model is best understood as a reactive policy, a deliberative policy, or a hybrid.

## 1.12 What to compare when two systems look similar

Two models can both be described as "VLM-based VLAs" and still differ profoundly. Always compare:
- whether they use the final hidden state or richer intermediate features,
- whether actions are tokenized, chunked, or generated by a separate expert,
- how they were fine-tuned after pretraining,
- which benchmarks and embodiments they actually cover,
- how much non-model infrastructure is required for reliable deployment.
