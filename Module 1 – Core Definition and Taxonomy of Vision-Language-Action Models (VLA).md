# Module 1 – Core Definition and Taxonomy of Vision-Language-Action Models (VLA)

## 1.1 Module Goals (Given Deep RL & Autonomy Background)

This module formalizes what a Vision-Language-Action (VLA) model is, and situates different VLA architectures within a taxonomy that will be useful when reading papers and repos. It assumes comfort with MDPs/POMDPs, policy parameterizations, and classical perception–planning–control stacks from courses in deep RL and autonomous systems.[^1][^2]

The objectives are:
- Provide a precise, research-level definition of VLA models and how they extend vision–language models (VLMs).[^3][^4]
- Introduce a taxonomy along axes you already know from autonomy (hierarchical vs. flat, model-based vs. model-free) and new VLA-specific axes (planner–controller vs. end-to-end, tokenized vs. continuous actions).[^5][^1]

## 1.2 Formal Definition of a VLA Model

In robot learning, a VLA model is a multimodal foundation model that takes visual inputs (images or video), language inputs (instructions, context), and sometimes additional state, and outputs actions for a physical or simulated robot. It jointly integrates visual perception, language understanding, and action generation within a single architecture, typically built on transformers.[^6][^2][^3][^1]

Concretely, a VLA instantiates a parameterized policy \(\pi_\theta(a_t \mid o_{1:t}, g)\), where \(o_t\) includes image and proprioceptive observations and \(g\) is a language goal, with \(\theta\) initialized from a large VLM or LLM. The model is usually trained via supervised sequence modeling or behavior cloning on trajectories \((o_{1:T}, g, a_{1:T})\), optionally followed by RL-based fine-tuning.[^7][^8][^3][^1]

## 1.3 Relationship to Vision–Language Models (VLMs)

Vision–language models (VLMs) are multimodal models that jointly process images and text to perform tasks such as captioning, VQA, and image-text retrieval. They typically output text tokens only, using vision features as context, and are not designed to control actuators directly.[^4][^2]

VLAs extend VLMs by adding **actions as a first-class modality**: they either attach a policy head that predicts continuous actions, or they treat actions as discrete tokens in the same sequence as text. This turns a perception-and-language backbone into an embodied policy, enabling direct mapping from instructions and images to motor commands or higher-level action primitives.[^8][^3][^7][^6]

## 1.4 Core Architectural Pattern: Multimodal Sequence Model

Most VLA architectures can be abstracted as a single transformer processing a concatenated sequence of modality-specific tokens:
\[
[\text{text tokens}, \text{image tokens}, \text{state tokens}, \text{action tokens}]
\]
processed with causal masking so actions attend to past observations and actions but not future ones. Vision tokens come from a CNN or ViT encoder, text tokens from a standard tokenizer, and state tokens from linear projections of joint angles, velocities, or other proprioception.[^2][^7][^6][^5]

Actions are either discrete tokens sampled from a vocabulary (e.g., binned deltas, codebook entries) or continuous outputs produced by a regression head on selected transformer positions. This unified sequence view is a useful mental model when comparing different VLA papers and understanding where they inject visual or language information.[^2][^5]

## 1.5 High-Level Taxonomy: Planner–Controller vs. End-to-End

One major axis in VLA design is **how much of the autonomy stack is collapsed into a single model** versus kept modular.[^9][^5]

- **Planner–controller VLAs**: A VLM or LLM-based module takes images and language and outputs high-level actions (subgoals, waypoints, skill calls), which a separate low-level controller executes.[^7][^9]
  - Example: systems that output a sequence of textual sub-instructions or motion primitives that are then translated into trajectories.
- **End-to-end VLAs**: A single transformer takes images and instructions and directly outputs low-level robot actions at some control frequency, without an explicit separate planner.[^1][^2]
  - Example: RT-style models and many recent open-source VLAs that output end-effector deltas or joint commands from the multimodal context.

From an autonomous systems viewpoint, planner–controller VLAs preserve some of the classical stack’s structure, while end-to-end VLAs push more of the burden into the learned model.[^8][^5]

## 1.6 Action Level Taxonomy: High-Level vs. Low-Level Control

A second axis is the **granularity of actions** that the VLA outputs.[^1][^8]

- **High-level actions**: symbolic skills, options, or semantically meaningful subgoals (e.g., "grasp mug", "place in bin").[^10][^9]
  - Often used when VLAs act as task planners, combined with skill libraries or classical controllers.
- **Mid-level actions**: waypoints, end-effector poses, or parameterized motion primitives that a tracking controller follows.[^9][^2]
- **Low-level actions**: joint velocities, torques, or small Cartesian deltas at tens of Hz, akin to the control layer in classical stacks.[^5][^1]

Your deep RL background maps naturally here: high-level actions correspond to options or macro-actions, mid-level to goal-conditioned policies, and low-level to standard control actions in continuous MDPs.

## 1.7 Action Representation Taxonomy: Continuous vs. Tokenized

VLAs differ in **how they represent actions inside the model**.[^5][^1]

- **Continuous heads**: the transformer outputs continuous vectors interpreted as actions, trained with regression losses.[^8][^2]
  - Pros: straightforward interface to controllers, high precision; cons: less aligned with token-based LLM pretraining.
- **Discrete tokenization**: actions or trajectories are discretized into tokens (e.g., bins, codebooks, or DCT-based coefficients) and appended to the sequence.[^1][^5]
  - Pros: reuse of language-model pretraining, easy integration into autoregressive decoding; cons: trade-offs in precision and sequence length.

Recent work distinguishes purely token-based VLAs from architectures that attach separate diffusion or flow-matching action "experts" to a VLM backbone—these can be seen as a hybrid where actions live in a separate continuous generative module.[^11][^5]

## 1.8 Single-Embodiment vs. Multi-Embodiment VLAs

Another axis is **embodiment coverage**: whether a VLA is trained for a single robot or many.[^10][^1]

- **Single-embodiment VLAs**: trained and deployed on one robot (e.g., a specific manipulator), often with environment-specific data.[^8]
- **Multi-embodiment or generalist VLAs**: trained on datasets spanning many robot types, with embodiment identifiers or shared action spaces enabling cross-robot generalization.[^10][^1]

For someone familiar with transfer in RL, multi-embodiment VLAs can be seen as a strong form of multi-task and multi-robot transfer, where a single model shares perception and language grounding while learning embodiment-specific actuation mappings.[^10][^1]

## 1.9 Offline Imitation vs. RL-Augmented Training

Though primarily a training axis (covered more deeply later), it is useful to include **training paradigm** in the taxonomy.[^1][^8]

- **Pure offline imitation VLAs**: trained solely with behavior cloning on large offline datasets of demonstrations, akin to sequence models for language.[^7][^2]
- **RL-augmented VLAs**: start from an imitation-trained backbone and fine-tune with RL for specific tasks, safety, or long-horizon goals.[^12][^5]

For a deep RL practitioner, VLAs can be interpreted as policy classes that are particularly amenable to pretraining, after which RL plays a refining rather than primary role.[^13][^5]

## 1.10 Reasoning-VLA vs. Reactive-VLA

Recent discussions distinguish **reasoning VLAs** from more reactive policies.[^14][^11]

- **Reactive VLAs**: map \((o_{1:t}, g)\) to \(a_t\) with minimal explicit reasoning trace, relying on implicit knowledge baked into parameters.[^2][^8]
- **Reasoning VLAs**: augment the model with explicit chain-of-thought or intermediate planning tokens, producing reasoning traces along with or before actions.[^11][^14]

This mirrors the distinction between standard RL policies and methods like hierarchical RL or planning with options, but implemented inside the sequence modeling framework.[^12][^11]

## 1.11 A Taxonomy Table for Quick Reference

The following conceptual table summarizes key axes:

| Axis | Category 1 | Category 2 | Notes |
|------|------------|------------|-------|
| Stack integration | Planner–controller | End-to-end VLA | Separation of high- vs. low-level control.[^7][^2] |
| Action granularity | High-level skills | Low-level control | Options vs. primitive actions.[^9][^5] |
| Action representation | Continuous head | Tokenized actions | Regression vs. discrete sequence modeling.[^2][^1] |
| Embodiment coverage | Single robot | Multi-embodiment | Per-robot vs. generalist policies.[^1][^10] |
| Training paradigm | Offline imitation | RL-augmented | BC-only vs. BC + RL.[^7][^12] |
| Reasoning style | Reactive | Reasoning VLA | Implicit vs. explicit reasoning traces.[^11][^14] |

This taxonomy allows you to quickly categorize a given VLA paper or system and compare it to others using concepts that map naturally to RL and autonomy.

## 1.12 Suggested Reading for Module 1

To deepen understanding of these definitions and axes:
- Read one or both short definitions of VLAs from encyclopedic or glossary-style sources for a clear baseline.[^3][^6]
- Skim a recent survey on VLA concepts and progress, focusing on the sections that propose taxonomies over action representation and system architecture.[^5][^1]
- Read a practitioner-friendly conceptual article on how VLAs are used in autonomy stacks and how they differ from classical pipelines.[^2][^8]

For a more research-oriented view, browse curated lists of VLA papers in embodied AI and note how different works occupy different positions in this taxonomy.[^10]

## 1.13 Recommended Exercises (Leveraging Your Background)

1. **Taxonomy Mapping Exercise**
   - Pick 3–5 VLA-related systems (e.g., an RT-style model, an OpenVLA-like model, and a planning-centric system like VLAbot) and classify them along the axes in Section 1.11: stack integration, action granularity, representation, embodiment coverage, training paradigm, and reasoning style.[^9][^2][^10]
   - For each, explain in one paragraph how the design choices relate to the kinds of data they use and the deployment scenarios they target.

2. **Policy Class Comparison with RL Lenses**
   - Compare a tokenized, end-to-end VLA acting at low level to a classical actor–critic policy network used in deep RL for the same robot.
   - Discuss advantages and disadvantages regarding representation learning, sample efficiency, safety, and interpretability, drawing directly on your RL coursework.

3. **Design Your Own VLA Slot in a Robot Stack**
   - Starting from a canonical perception–planning–control architecture from your autonomous systems course, design a hybrid stack where a VLA occupies one module (e.g., high-level planner or mid-level action generator).
   - Specify what the VLA’s inputs and outputs would be, how you would constrain it with safety layers, and what data you would need to train it.

Working through these exercises will make the formal definition and taxonomy of VLAs concrete and set you up to engage more deeply with specific architectures and datasets in later modules.

---

## References

1. [Vision-Language-Action Models: Concepts, Progress, Applications ...](https://arxiv.org/html/2505.04769v1) - This foundational review presents a comprehensive synthesis of recent advancements in Vision-Languag...

2. [A Comprehensive Overview of Vision-Language-Action Models](https://www.digitalocean.com/community/conceptual-articles/vision-language-action-models) - A Comprehensive Overview of Vision-Language-Action Models ... Check out our tutorial on fine-tuning ...

3. [Vision-language-action model](https://en.wikipedia.org/wiki/Vision-language-action_model) - In robot learning, a vision-language-action model (VLA) is a class of multimodal foundation models t...

4. [Vision-language model](https://en.wikipedia.org/wiki/Vision-language_model) - A vision–language model (VLM) is a type of artificial intelligence system that can jointly interpret...

5. [Vision-Language-Action (VLA) Models: Concepts, ...](https://arxiv.org/html/2505.04769v2) - The presentation begins with an introduction that motivates embodied intelligence and the emergence ...

6. [Vision-Language-Action Model (VLA) Definition](https://encord.com/glossary/vision-language-action-model-definition/) - A Vision-Language-Action Model (VLA) is an AI model that takes visual and language inputs and output...

7. [Vision Language Action Models (VLA) & Policies for Robots](https://learnopencv.com/vision-language-action-models-lerobot-policy/) - In this article, we will go in-depth discussing how VLAs have evolved post GPT era with compelling p...

8. [Vision-Language-Action Models: How Foundation ...](https://www.digitaldividedata.com/blog/vision-language-action-models-autonomy) - Formally, a VLA model is a single architecture that takes multimodal inputs, text, images, sometimes...

9. [VLAbot: A human Vision–Language–Action models ...](https://www.sciencedirect.com/science/article/pii/S0736584526000475) - The VLA framework decomposes high-level instructions into executable robot actions through four spec...

10. [jonyzhang2023/awesome-embodied-vla-va-vln](https://github.com/jonyzhang2023/awesome-embodied-vla-va-vln) - A curated list of state-of-the-art research in embodied AI, focusing on vision-language-action (VLA)...

11. [What Is Reasoning VLA?](https://www.nvidia.com/en-us/glossary/reasoning-vision-language-action/) - Reasoning VLA (vision-language-action) is a unified artificial intelligence model that integrates vi...

12. [Robotics' End Game: From VLA Models to Generative ...](https://robocloud-dashboard.vercel.app/learn/blog/robotics-end-game-world-models) - The strongest near-term robot stack will mix generative world models, action models, controllers, si...

13. [Foundation Models in Robotics: Applications, Challenges ...](https://arxiv.org/html/2312.07843v1) - We explore how foundation models contribute to improving robot capabilities in the domains of percep...

14. [Vision-Language-Action (VLA) Models: The AI Brain ...](https://blog.stackademic.com/vision-language-action-vla-models-the-ai-brain-behind-the-next-generation-of-robots-physical-bced48e8ae94) - Vision-Language-Action (VLA) models are the next big shift in robotics and Physical AI. Instead of j...

