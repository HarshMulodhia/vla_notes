# Chapter 08 - Inference-Time Behavior, Prompting, and Control Loops

## 8.1 Runtime loop

At each control cycle:
1. collect latest observation(s),
2. build model context (instruction, history, state),
3. infer action/action chunk,
4. apply safety filter,
5. execute through controller,
6. log and monitor.

## 8.2 Prompting and instruction strategy

Instruction format affects behavior significantly:
- explicit goal + constraints,
- scene/object references,
- success criteria.

Prompt templates should be standardized for consistency.

## 8.3 Closed-loop vs open-loop execution

- Open-loop chunks reduce latency but are brittle.
- Closed-loop replanning is robust but compute-heavy.

Practical systems usually blend both.

## 8.4 Latency and control frequency

Latency budget must include:
- sensor read,
- preprocessing,
- model inference,
- safety checks,
- actuator command pipeline.

If inference is too slow, policy quality gains may not translate to control performance.

## 8.5 Runtime fallback hierarchy

Recommended hierarchy:
1. normal VLA control,
2. constrained fallback policy,
3. safe stop/hold,
4. human takeover.

