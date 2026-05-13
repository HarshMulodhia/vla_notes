# Chapter 00 - Orientation and Learning Strategy

## 0.1 Why this book exists

VLA has become the center of modern robot intelligence stacks because it unifies:
- perception (vision),
- instruction following (language),
- decision/execution (action).

With your background, the key shift is this:
- In deep RL courses, you likely optimized policies for a specific task.
- In VLA, you learn to build **generalist pretrained policies** that can be adapted across tasks and sometimes embodiments.

## 0.2 Prior knowledge mapping

### What you already know
- **Deep RL**: policy optimization, exploration-exploitation, actor-critic ideas.
- **CV basics**: feature extraction, representation quality, invariance.
- **Autonomous systems**: perception-planning-control decomposition and safety constraints.

### What is new in VLA
- Language-conditioned control as first-class policy input.
- Sequence-model view of control (actions as tokens or autoregressive outputs).
- Data scaling and pretraining as central performance drivers.
- Generalization across tasks and embodiments.

## 0.3 Suggested learning route

- **Pass 1 (Conceptual):** Chapters 1, 2, 3, 4.
- **Pass 2 (Training):** Chapters 5, 6, 7.
- **Pass 3 (Engineering):** Chapters 8, 9, 10, 11.
- **Pass 4 (Research):** Chapter 12 + capstone.

## 0.4 Core mental model

Treat VLA as:
1. a large multimodal sequence model,
2. conditioned on visual observations + text goals (+ optional state),
3. trained mostly from offline demonstrations,
4. adapted with targeted post-training (RL, safety constraints, preference tuning),
5. integrated into a full robot system with safety and latency guarantees.

