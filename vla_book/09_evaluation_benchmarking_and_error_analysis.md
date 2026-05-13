# Chapter 09 - Evaluation, Benchmarking, and Error Analysis

## 9.1 What to evaluate

- task success rate,
- robustness to perturbations,
- generalization to unseen goals/objects,
- sample efficiency of adaptation,
- safety incident rate,
- recovery behavior quality.

## 9.2 Benchmark levels

- single-task sandbox,
- multitask simulation suites,
- cross-embodiment transfer settings,
- real-world deployment trials.

## 9.3 Generalization tests

Use systematic holdouts:
- unseen object instances,
- unseen language paraphrases,
- camera viewpoint shifts,
- domain randomization variations.

## 9.4 Error taxonomy

Analyze failures by class:
- perception grounding failures,
- instruction misunderstanding,
- dynamics/control mismatch,
- long-horizon compounding drift,
- safety-filter over-conservatism.

## 9.5 Ablation principles

At minimum ablate:
- vision backbone choice,
- action representation,
- context length,
- data scale and diversity,
- post-training method.

