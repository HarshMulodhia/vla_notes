# Chapter 02 - Problem Formulation and Mathematical View

## 2.1 POMDP framing

VLA control is naturally modeled as a partially observable Markov decision process (POMDP):
\[
\mathcal{P} = (\mathcal{S}, \mathcal{A}, \mathcal{O}, T, R, \gamma)
\]
But in many VLA pipelines, reward is absent during pretraining; supervision comes from demonstration trajectories.

## 2.2 Sequence modeling view

Given trajectory data:
\[
\tau = (o_1, s_1, a_1, ..., o_T, s_T, a_T, g)
\]
training often maximizes:
\[
\sum_t \log p_\theta(a_t \mid o_{\le t}, s_{\le t}, g, a_{<t})
\]
This converts policy learning into conditional next-token prediction.

## 2.3 Horizon and temporal abstraction

Key design choice: what does one predicted action represent?
- instant control primitive,
- short-horizon chunk,
- high-level skill command.

Temporal abstraction affects stability, control frequency, and error accumulation.

## 2.4 Distribution shift in robotics

Unlike offline NLP, robot deployment introduces covariate shift:
- policy-induced states deviate from demonstration states,
- sensor noise and hardware drift break assumptions,
- long-horizon rollout compounds small errors.

Therefore, VLA systems need online correction loops (replanning, fallback controllers, human intervention).

## 2.5 Objective mismatch

A pure imitation objective optimizes action likelihood, not task success directly.
Post-training (RL, preference alignment, constraints) is used to close this gap.

