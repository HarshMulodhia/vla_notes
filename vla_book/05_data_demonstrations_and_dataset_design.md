# Chapter 05 - Data, Demonstrations, and Dataset Design

## 5.1 Why data dominates VLA performance

For VLAs, scaling laws are strongly data-driven:
- task coverage,
- environment diversity,
- viewpoint variability,
- instruction quality,
- action consistency.

## 5.2 Dataset units

A high-quality trajectory sample usually includes:
- timestamped images/video,
- robot state/proprioception,
- action vectors,
- language instruction/goal,
- task outcome metadata.

## 5.3 Demonstration collection modes

- Human teleoperation,
- scripted controllers,
- policy-generated + filtered rollouts,
- simulation-to-real pipelines.

Each has bias/quality trade-offs.

## 5.4 Label and instruction quality

Language is not just decoration. Good instruction design improves grounding and compositional transfer.
Important aspects:
- semantic precision,
- actionability,
- consistency of vocabulary.

## 5.5 Dataset balancing

Common failure mode: overrepresenting easy behaviors.
Mitigations:
- per-task reweighting,
- hard-example mining,
- long-tail upsampling,
- curriculum by complexity.

## 5.6 Data schema and reproducibility

Use standardized formats for trajectories and metadata so that training and evaluation remain reproducible across labs.

