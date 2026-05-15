# Chapter 02 - Problem Formulation and Mathematical View

## 2.1 POMDP framing for VLA control

Robot control is naturally partially observable. Camera streams and proprioception provide incomplete, noisy information, so VLA policies are conditioned on observation history:
\[
a_t \sim \pi_\theta(\cdot \mid h_t, g), \quad h_t=(o_{1:t}, s_{1:t}, a_{1:t-1})
\]

A practical implication: memory design is not optional. If your policy context window is too short, manipulation failures rise sharply in occluded settings.

## 2.2 Sequence modeling objective

Many VLAs optimize autoregressive action likelihood:
\[
\max_\theta \sum_t \log p_\theta(a_t \mid o_{\le t}, s_{\le t}, g, a_{<t})
\]
For tokenized actions this is cross-entropy; for continuous actions this may involve Gaussian NLL or MSE variants.

Important distinction:
- low prediction error does not guarantee high task success,
- rollout-level objectives are still needed for robust deployment.

## 2.3 Temporal abstraction and control horizon

Define the policy timescale explicitly:
- single-step reactive actions,
- multi-step action chunks,
- high-level subgoal emissions.

This determines:
- control frequency,
- stability vs responsiveness,
- how quickly errors can be corrected.

## 2.4 Distribution shift and compounding error

Offline demonstration training assumes expert-state distribution. During deployment, policy mistakes move the system off-distribution. Then prediction quality degrades further.

Mitigations:
- iterative data aggregation,
- uncertainty-aware gating,
- fallback behavior primitives,
- targeted failure replay data.

## 2.5 Objective mismatch

Imitation objective optimizes “matching the demonstrator,” while robotics objective is “safely complete the task.”

Mismatch examples:
- multiple valid actions but one demonstrated path,
- delayed success criteria not captured by local action loss,
- safety failures not penalized by BC objective.

Hence post-training and constrained execution are critical.

## 2.6 Credit assignment in VLA systems

Classical RL handles delayed reward explicitly. VLA BC pipelines often hide this with supervised labels. This can cause weak long-horizon reasoning.

Typical fixes:
- hierarchical policies,
- value-guided reranking,
- RL refinement on difficult rollout segments.

## 2.7 Calibration and uncertainty

For physical deployment, action confidence matters. Useful forms:
- predictive entropy (tokenized output),
- distributional variance (continuous heads),
- ensemble disagreement,
- state novelty scores.

These signals can drive intervention and fallback policies.

## 2.8 Practical math-to-system checklist

Before training, specify:
1. state/observation definition,
2. action parameterization and units,
3. context length and temporal stride,
4. loss terms and weighting,
5. rollout evaluation protocol,
6. uncertainty handling and safety constraints.

## 2.9 Why generative-model math now belongs in VLA notes

The current literature makes it clear that action prediction is no longer only a next-step supervised-learning problem. Diffusion and flow-matching formulations matter because robot action generation is often multimodal, horizon-dependent, and sensitive to smoothness. A mathematical treatment of VLA control should therefore include both autoregressive likelihood views and conditional generative-trajectory views.

## 2.10 Practical bridge from equations to papers

When reading RT-2 or OpenVLA, the main mathematical lens is usually conditional sequence modeling. When reading Diffusion Policy, π0, or FAST-related work, add the lens of trajectory generation, compression, and receding-horizon decoding. The important move is to translate every paper back into the same policy question: what conditional object is the model actually learning to generate?
