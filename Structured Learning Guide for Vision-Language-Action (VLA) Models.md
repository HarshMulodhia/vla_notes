# Structured Learning Guide for Vision-Language-Action (VLA) Models

## Purpose

This guide now follows the 6-phase, 14-week flow used in the [awesome-vla-study](https://github.com/MilkClouds/awesome-vla-study) curriculum. The goal is to move from the mathematical and generative foundations behind modern robot policies to the latest work on large-scale VLA architectures, data scaling, efficient deployment, RL fine-tuning, reasoning, and world-model-based control.

## Recommended background

Before starting, you should be comfortable with:
- probability, optimization, and maximum-likelihood training,
- transformers, tokenization, and multimodal encoders,
- imitation learning and the basics of reinforcement learning,
- standard robotics concepts such as perception, control loops, teleoperation, and task success metrics.

If any of these are weak, spend a short prep week on diffusion basics, transformer mechanics, and behavior cloning.

## Suggested weekly format

Each week works best as a small seminar:
1. read the core papers before the session,
2. assign one presentation per paper focused on architecture, data, training, and evaluation,
3. compare design choices across papers instead of treating each paper independently,
4. finish by writing down one unresolved question and one system-design takeaway.

## Phase 1 - Generative model foundations (Weeks 1-3)

### Week 1 - ODE/SDE intuition and diffusion basics
**Core material**
- MIT 6.S184 lectures on ODE/SDE foundations and diffusion models
- course notes on diffusion and flow matching

**What to learn**
- why continuous-time generative models matter for action generation,
- forward and reverse processes,
- score fields, probability paths, and noise schedules,
- how denoising-style training relates to predicting smooth robot action sequences.

**Study questions**
- Why are diffusion-style objectives attractive for multimodal robot actions?
- What parts of the diffusion story are directly useful at inference time and what parts are only training machinery?

### Week 2 - Flow matching, score matching, and guidance
**Core material**
- MIT 6.S184 lectures on flow matching and score matching
- classifier-free guidance and conditional generation notes

**What to learn**
- the difference between score-based diffusion and flow-matching views,
- how conditional generation maps to instruction-conditioned robot behavior,
- when continuous generative decoders can outperform pure autoregressive action tokenization.

**Study questions**
- If a VLA uses a separate action expert, is it acting more like an autoregressive policy or a conditional generative model?
- How does guidance correspond to task conditioning, safety shaping, or value-based reranking?

### Week 3 - Generative robotics bridge
**Core material**
- MIT guest material on diffusion for robotics
- short review of action-sequence generation papers in robot control

**What to learn**
- why smoothness, multimodality, and receding-horizon control matter for robot action generation,
- how continuous-time generative models connect to action chunks, trajectory proposals, and controller interfaces,
- which parts of modern VLA design inherited ideas from diffusion-policy-style control.

## Phase 2 - Early robot foundation models and core robot policy ideas (Weeks 4-5)

### Week 4 - RT-1, RT-2, Octo, OpenVLA
**Core papers**
- RT-1
- RT-2
- Octo
- OpenVLA

**What to learn**
- RT-1 as the large-scale robotics transformer baseline,
- RT-2 as the clearest shift from VLMs to VLA behavior,
- Octo as a strong open-source generalist robot policy without a large VLM core,
- OpenVLA as the major open-source VLM-based VLA reference point.

**Compare explicitly**
- backbone choice,
- action representation,
- dataset source and scale,
- embodiment coverage,
- zero-shot vs fine-tuned deployment story.

### Week 5 - BeT, Diffusion Policy, ACT/ALOHA
**Core papers**
- Behavior Transformers (BeT)
- Diffusion Policy
- ACT / ALOHA

**What to learn**
- BeT for action discretization and multimodal action clustering,
- Diffusion Policy for trajectory denoising and receding-horizon generation,
- ACT for action chunking and long-horizon stabilization.

**Why this week matters**
This week gives the vocabulary needed to understand later VLA papers: action bins, chunking, latent actions, diffusion heads, and sequence length vs control precision trade-offs.

## Phase 3 - Current robot foundation model architectures (Weeks 6-7)

### Week 6 - VLM plus action head
**Core papers**
- CogACT
- GR00T N1
- X-VLA

**What to learn**
- the common pattern where a strong VLM provides grounded features,
- a separate action head or DiT-style module predicts control outputs,
- how cross-embodiment conditioning and soft prompts are used to adapt one model across robots.

**Design lens**
Ask whether the action module only reads the last hidden state or receives richer intermediate visual-linguistic features.

### Week 7 - VLM plus action expert
**Core papers**
- π0
- InternVLA-M1
- Transfusion as architectural background

**What to learn**
- why action experts that access internal VLM features can be more expressive,
- the difference between action heads and separate action experts,
- when autoregressive text-style decoding is too restrictive for real-time robot control.

## Phase 4 - Data scaling (Weeks 8-9)

### Week 8 - OXE, AgiBot World, and dataset infrastructure
**Core papers/resources**
- Open X-Embodiment (OXE)
- AgiBot World
- RLDS
- LeRobotDataset
- rosbag and MCAP as logging formats

**What to learn**
- why large, cross-lab datasets changed what is possible in robot policy pretraining,
- the difference between logging formats and training formats,
- how embodiment metadata, calibration metadata, and task labels shape model quality.

### Week 9 - UMI, VITRA, and human-to-robot transfer
**Core papers**
- UMI
- VITRA
- Human to Robot Transfer

**What to learn**
- data collection without full robot teleoperation,
- human video as weakly aligned supervision for robot policies,
- what scaling laws may look like when action labels are scarce but visual behavior data is abundant.

## Phase 5 - Efficient inference and dual-system control (Weeks 10-11)

### Week 10 - SmolVLA and RTC
**Core papers**
- SmolVLA
- RTC

**What to learn**
- compression vs inference-pipeline optimization,
- asynchronous execution, freezing, and inpainting ideas,
- why control performance is governed by latency budgets and not just benchmark accuracy.

### Week 11 - Helix and Fast-in-Slow
**Core papers/resources**
- Helix
- Fast-in-Slow

**What to learn**
- dual-rate control,
- slow reasoning plus fast execution policies,
- the practical case for splitting cognition and motor control into separate loops.

## Phase 6 - RL fine-tuning, reasoning, and world models (Weeks 12-14)

### Week 12 - RL fine-tuning
**Core papers**
- HIL-SERL
- SimpleVLA-RL
- π*0.6 / Recap

**What to learn**
- when behavior cloning saturates,
- head-only vs adapter vs full-policy RL updates,
- why outcome-based rewards and human-in-the-loop corrections matter.

### Week 13 - Reasoning VLAs
**Core papers**
- CoT-VLA
- ThinkAct
- Fast-ThinkAct

**What to learn**
- explicit reasoning traces versus latent planning,
- whether reasoning must be textual,
- how planning quality can be distilled into faster execution-time representations.

### Week 14 - World-model-enhanced VLAs
**Core papers**
- UniVLA
- Cosmos Policy
- DreamZero

**What to learn**
- world modeling as a training objective vs an inference-time planning tool,
- video foundation models as robot-policy backbones,
- joint world-and-action generation for long-horizon control.

## Cross-cutting comparison template

For every paper, answer the same questions:
1. What are the exact inputs and outputs?
2. How are actions represented?
3. What data scale and embodiment scope are used?
4. Is the policy autoregressive, diffusion-based, flow-matching-based, or hybrid?
5. What is the runtime control frequency and latency story?
6. What post-training, reasoning, or safety layers are added after pretraining?

## How this guide connects to the rest of the repository

- The module notes give deeper lecture-style explanations of foundations, taxonomy, backbones, and action representations.
- The book translates the paper flow into chapter-length notes on system design, data, optimization, deployment, and research direction.
- The most important habit is to compare papers by interface choices, data regime, and runtime assumptions rather than by headline success rate alone.


## Detailed weekly paper list (with direct links)

### Phase 1 (Weeks 1-3): Generative foundations
- MIT 6.S184 course (diffusion and flow matching): https://diffusion.csail.mit.edu/2025/index.html
- Course notes paper: https://arxiv.org/abs/2506.02070
- Diffusion tutorial context for practitioners: https://huggingface.co/learn/diffusion-course/unit1/1

### Phase 2 (Weeks 4-5): Early robot foundation models and core policy ideas
- RT-1: https://arxiv.org/abs/2212.06817
- RT-2: https://arxiv.org/abs/2307.15818
- Octo: https://arxiv.org/abs/2405.12213
- OpenVLA: https://arxiv.org/abs/2406.09246
- Behavior Transformers (BeT): https://arxiv.org/abs/2206.11251
- Diffusion Policy: https://arxiv.org/abs/2303.04137
- ACT/ALOHA: https://arxiv.org/abs/2304.13705

### Phase 3 (Weeks 6-7): Current architecture families
- CogACT: https://arxiv.org/abs/2411.19650
- GR00T N1: https://arxiv.org/abs/2503.14734
- X-VLA: https://arxiv.org/abs/2510.10274
- π0: https://arxiv.org/abs/2410.24164
- InternVLA-M1: https://arxiv.org/abs/2510.13778
- Transfusion (architectural context): https://arxiv.org/abs/2408.11039

### Phase 4 (Weeks 8-9): Data scaling and data engines
- Open X-Embodiment (OXE): https://arxiv.org/abs/2310.08864
- AgiBot World: https://arxiv.org/abs/2503.06669
- UMI: https://arxiv.org/abs/2402.10329
- VITRA: https://arxiv.org/abs/2510.21571
- Human to Robot Transfer: https://arxiv.org/abs/2512.22414
- RLDS: https://github.com/google-research/rlds
- LeRobot docs: https://huggingface.co/docs/lerobot

### Phase 5 (Weeks 10-11): Efficient inference and dual-system designs
- SmolVLA: https://arxiv.org/abs/2506.01844
- RTC: https://arxiv.org/abs/2506.07339
- Helix: https://www.figure.ai/news/helix
- Fast-in-Slow: https://arxiv.org/abs/2506.01953

### Phase 6 (Weeks 12-14): RL fine-tuning, reasoning, and world models
- HIL-SERL: https://arxiv.org/abs/2410.21845
- SimpleVLA-RL: https://arxiv.org/abs/2509.09674
- π*0.6 / Recap: https://arxiv.org/abs/2511.14759
- CoT-VLA: https://arxiv.org/abs/2503.22020
- ThinkAct: https://arxiv.org/abs/2507.16815
- Fast-ThinkAct: https://arxiv.org/abs/2601.09708
- UniVLA: https://arxiv.org/abs/2506.19850
- Cosmos Policy: https://arxiv.org/abs/2601.16163
- DreamZero: https://dreamzero0.github.io/

## Tutorials, courses, and books to run in parallel

### Online tutorials and courses
- Hugging Face VLM primer: https://huggingface.co/blog/vlms
- Hugging Face π0 overview: https://huggingface.co/blog/pi0
- LeRobot policy training docs: https://huggingface.co/docs/lerobot
- UC Berkeley CS285 (Deep RL): http://rail.eecs.berkeley.edu/deeprlcourse/
- Stanford CS231n (vision): https://cs231n.stanford.edu/
- Stanford CS224n (NLP/transformers): https://web.stanford.edu/class/cs224n/

### Foundational books (related fields)
- Sutton & Barto, *Reinforcement Learning: An Introduction* (2nd ed.)
- Lynch & Park, *Modern Robotics*
- Siciliano et al., *Springer Handbook of Robotics*
- Murphy, *Probabilistic Robotics*
- Goodfellow, Bengio, Courville, *Deep Learning*

Use these resources as background reinforcement while following the week-by-week paper sequence.
