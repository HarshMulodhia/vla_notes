# Chapter 08 - Inference-Time Behavior, Prompting, and Control Loops

## 8.1 Runtime control loop anatomy

At each cycle:
1. acquire synchronized sensor state,
2. build context window (history + instruction + state),
3. infer action or action chunk,
4. apply safety and feasibility checks,
5. execute via controller,
6. log outcomes and monitor anomalies.

Reliability depends on the consistency of this full loop, not only model accuracy.

## 8.2 Prompting and instruction protocol

Prompt structure should standardize:
- task intent,
- object references,
- constraints,
- success condition,
- optional style or safety hints.

Inconsistent prompting produces avoidable behavior variance.

## 8.3 Context management

Decide what historical information to retain:
- fixed-size sliding window,
- event-based memory tokens,
- summarized state features.

Too little history harms disambiguation; too much history increases latency and attention noise.

## 8.4 Closed-loop versus open-loop execution

- **Closed-loop:** frequent replanning, better robustness, higher compute demand.
- **Open-loop chunks:** lower overhead, weaker recovery to disturbances.

Many systems use adaptive control: open-loop chunk execution with periodic closed-loop correction triggers.

## 8.5 Latency budget and control frequency

End-to-end latency includes:
- sensor capture,
- preprocessing,
- model forward pass,
- safety filters,
- command dispatch.

If latency approaches the plant dynamics timescale, control quality collapses even with accurate predictions.

## 8.6 Runtime uncertainty and gating

Use uncertainty estimates to decide:
- continue normal control,
- reduce action magnitude,
- request replanning,
- switch to fallback mode.

Uncertainty-aware gating is one of the most practical reliability upgrades for VLA deployment.

## 8.7 Fallback and recovery hierarchy

Recommended hierarchy:
1. normal VLA policy,
2. constrained VLA mode,
3. scripted safe primitive,
4. stop-and-hold,
5. human takeover.

Each transition should have explicit trigger conditions and logging.

## 8.8 Inference-time debugging checklist

When behavior is unstable, inspect:
- prompt consistency,
- context window integrity,
- action scaling/clipping,
- latency spikes,
- frequency of safety-layer overrides.

## 8.9 Efficient inference is now a model capability question

SmolVLA, RTC, Helix, and Fast-in-Slow make an important point: inference design is not downstream plumbing. Compression, asynchronous execution, frozen context reuse, and dual-rate control can change what tasks a model can complete at all. In VLA systems, a strong model with a weak runtime loop often loses to a smaller model with a better latency budget.

## 8.10 Slow reasoning plus fast execution is becoming a default pattern

Dual-system designs separate heavy semantic reasoning from lightweight motor execution. This pattern appears in different forms across Helix and Fast-in-Slow and is likely to become more common as models grow. The practical lesson is that not every control cycle needs full multimodal deliberation; many cycles only need stable execution conditioned on a slowly updated plan.

## Deepening resources

- SmolVLA (https://arxiv.org/abs/2506.01844)
- RTC (https://arxiv.org/abs/2506.07339)
- Fast-in-Slow (https://arxiv.org/abs/2506.01953)

For a broader reading index across all chapters, see Chapter 13.
