# Module 3 – Action Representation and Tokenization for VLA Models

## 3.1 Why action representation is the key systems decision

In VLA work, action representation is not a minor output-layer choice. It determines what the model can express, how easily it can inherit LLM-style pretraining, how long inference takes, and how naturally it interfaces with robot controllers.

Two models with the same backbone can behave very differently purely because they choose different action abstractions.

## 3.2 Continuous actions: the familiar baseline

From an RL perspective, the simplest option is to regress continuous actions such as joint velocities or end-effector deltas. This keeps the interface close to classical robot control and preserves precision.

Advantages:
- easy controller integration,
- no quantization error,
- natural fit for continuous-control tasks.

Weaknesses:
- weaker alignment with autoregressive token modeling,
- harder handling of multimodal action distributions with a simple Gaussian head,
- limited leverage from the discrete-token machinery used by LLMs.

## 3.3 Discrete tokenization and action bins

Discrete action tokens let a transformer reuse its causal next-token machinery for control. Common designs discretize each action dimension into bins and decode one or several categorical outputs per step.

This is attractive because it keeps the whole policy inside a language-model-style framework, but it introduces trade-offs:
- finer precision means larger vocabularies or longer sequences,
- action scaling must be carefully normalized across robots,
- small binning mistakes can create large physical errors after controller execution.

## 3.4 BeT, Diffusion Policy, and ACT as three anchor designs

### BeT
Behavior Transformers make discrete action modeling concrete by clustering or tokenizing actions and then predicting them autoregressively with offsets. This is a strong reference point for thinking about action discretization.

### Diffusion Policy
Diffusion Policy frames control as conditional trajectory generation. It is especially useful when several futures are valid and smooth receding-horizon action sequences are desired.

### ACT / ALOHA
ACT shows why action chunking matters. Predicting short future windows can stabilize long-horizon behavior, reduce compounding error, and improve execution for manipulation tasks.

Together, these three papers explain much of the later VLA design space.

## 3.5 Action chunks, latent actions, and trajectories

Beyond single-step actions, modern VLAs often predict:
- short action chunks,
- latent trajectory codes,
- waypoint-style intermediate plans,
- full trajectory samples from a generative decoder.

These choices matter because the physical robot does not care whether the policy predicted a token, a chunk, or a denoised sample. It cares whether the resulting command stream is smooth, timely, and recoverable.

## 3.6 FAST and compressed action tokenization

FAST highlights an important idea: you can preserve a token-based action interface while compressing the time series. By transforming an action window into a compact frequency-space representation, FAST reduces token length while preserving useful trajectory information.

This matters because many VLA systems otherwise hit a painful trade-off between high-frequency control and long autoregressive sequences.

## 3.7 Action representation and embodiment transfer

Action design strongly affects cross-embodiment learning.

- raw joint actions are highly embodiment-specific,
- end-effector deltas are more portable but still controller-dependent,
- latent or chunked representations can be more transferable if they separate intent from morphology,
- high-level skills are portable only when a stable skill library exists on every target robot.

A paper claiming multi-robot generalization is not complete until it explains how the action space was normalized or conditioned across robots.

## 3.8 Action representation and runtime behavior

Your action interface shapes runtime properties:
- chunking can reduce inference frequency requirements,
- continuous heads may support fast control loops but miss multimodal alternatives,
- diffusion or flow-matching decoders can improve smoothness but raise inference cost,
- compressed tokenizations try to recover efficiency without discarding sequence modeling.

This is why action design cannot be separated from deployment engineering.

## 3.9 A practical comparison checklist

For any action interface, ask:
1. what exactly is emitted by the model,
2. over what horizon,
3. with what units and normalization,
4. how it is decoded into executable commands,
5. how much latency it adds,
6. how much precision is lost,
7. how easily it transfers across embodiments.

## 3.10 Failure modes by action representation

Different interfaces fail differently:
- continuous heads can become over-smoothed or average away multimodality,
- coarse bins can produce jerky or quantized behavior,
- long token sequences can accumulate autoregressive drift,
- chunked policies can delay correction to disturbances,
- generative trajectory models can be strong offline but too slow if deployment is not redesigned.

## 3.11 The main takeaway

When you read later papers such as OpenVLA, π0, SmolVLA, or Fast-ThinkAct, keep returning to one question: **what is the action object this model is really producing?** The best way to understand a VLA is often to understand its action interface before anything else.
