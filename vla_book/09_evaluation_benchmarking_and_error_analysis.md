# Chapter 09 - Evaluation, Benchmarking, and Error Analysis

## 9.1 What should be evaluated

A complete VLA evaluation includes:
- task success rate,
- time-to-completion and efficiency,
- robustness under perturbations,
- instruction-following fidelity,
- safety incident frequency,
- recovery effectiveness.

A single aggregate score is not enough to understand deployment risk.

## 9.2 Evaluation hierarchy

### Unit-level checks
Verify action formatting, range, and constraint compliance.

### Scenario-level simulation
Evaluate in controlled tasks with systematic variation.

### Stress and perturbation testing
Inject visual noise, distractors, delays, and actuator perturbations.

### Physical trials
Run staged real-world tests with strict safety supervision and intervention logs.

Each level should gate progression to the next.

## 9.3 Benchmark design principles

Good benchmarks must vary:
- objects and layouts,
- language phrasings and ambiguity,
- camera viewpoints and lighting,
- initial states and goal distributions,
- embodiment or controller settings.

If benchmark variation is too narrow, models overfit to protocol artifacts.

## 9.4 Generalization testing framework

Use explicit holdout axes:
- unseen objects,
- unseen language paraphrases,
- unseen compositions of known skills,
- unseen environmental conditions.

Report both in-distribution and out-of-distribution performance to avoid misleading conclusions.

## 9.5 Error taxonomy for diagnosis

Classify each failure as one or more of:
- perception grounding failure,
- language grounding failure,
- action decoding/control mismatch,
- temporal planning drift,
- safety-layer conflict,
- hardware/runtime systems fault.

Taxonomy-driven debugging shortens iteration cycles by targeting the correct subsystem.

## 9.6 Ablation strategy

Minimum ablations:
- action representation,
- context length,
- language prompt template,
- data scale and balancing,
- post-training strategy.

Ablations should be designed to isolate causal factors, not only produce leaderboard improvements.

## 9.7 Reporting standards

For reproducible reporting, include:
- dataset and split definitions,
- exact evaluation protocol,
- confidence intervals over multiple seeds,
- failure class histograms,
- intervention statistics.

Transparent reporting is essential for comparing VLA systems fairly.

