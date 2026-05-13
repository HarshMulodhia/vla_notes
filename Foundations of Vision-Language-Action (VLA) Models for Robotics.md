# Foundations of Vision-Language-Action (VLA) Models for Robotics

## Executive Overview

Vision-Language-Action (VLA) models extend vision-language models to directly output robot actions, enabling instruction-driven control across diverse tasks and platforms. These models combine pretrained visual encoders, large language models, and action decoders trained on large-scale robot demonstration datasets such as Open X-Embodiment.[^1][^2][^3][^4][^5]

VLAs sit at the intersection of representation learning, imitation and reinforcement learning, sequence modeling, and robotics, and are increasingly framed as robotic foundation models. This document introduces the core concepts, architectures, training paradigms, datasets, and evaluation practices needed to understand and build modern VLA systems, with an emphasis on technical detail suitable for a graduate-level ML/robotics background.[^6][^7][^8][^9]

## 1. Conceptual Foundations

### 1.1 What is a VLA Model?

A VLA model is typically a transformer-based policy that maps multi-modal observations (images, text, robot state) to actions, often autoregressively over time. It can be viewed as a conditioned sequence model where visual tokens and language tokens form a context prefix and action tokens form a suffix that the model learns to generate.[^2][^10][^11][^1][^6]

Compared to traditional task-specific policies, VLAs aim to be generalist: one model is trained across many tasks, environments, and robot embodiments, and can be controlled via natural language instructions. This generality arises from large-scale pretraining on diverse robot datasets and from leveraging powerful vision-language backbones.[^3][^11][^9]

### 1.2 Relationship to VLMs, Policies, and Planners

Modern VLAs build on vision-language models (VLMs) that already integrate visual and textual understanding, but add action and state tokens as extra modalities. In this sense, a VLA is a VLM extended with an action head or an action-generating suffix that encodes the policy.[^11][^1][^6]

There are multiple ways to combine VLMs and control:
- Use a VLM/LLM as a high-level planner and separate low-level controllers ("Type-1" VLA schemes).[^1]
- Use generative image/video models as high-level planners and dedicated controllers as executors.[^1]
- Use a single, end-to-end VLM backbone that outputs executable actions directly ("end-to-end VLAs").[^2][^1]
These design choices trade off modularity, interpretability, and data efficiency.

### 1.3 Problem Formulation

At a high level, VLAs tackle a conditional policy learning problem:

- Observations: robot sensory state, typically RGB images, proprioception, and sometimes depth or tactile inputs.
- Context: language instruction describing the task, possibly additional text describing constraints or goals.
- Actions: continuous or discretized controls over joint space, end-effector space, or higher-level primitives.

Given a trajectory \((o_{1:T}, a_{1:T}, g)\), where \(o_t\) are observations, \(a_t\) are actions, and \(g\) is a language goal, a VLA learns a policy \(\pi_\theta(a_t \mid o_{1:t}, g)\) (or the entire sequence \(a_{1:T}\)) by minimizing behavior cloning or likelihood loss on action tokens.[^10][^6][^11]

## 2. Core Prerequisite Concepts

### 2.1 Transformers and Sequence Modeling

Most VLAs are based on transformers because they scale well and naturally handle multi-modal token sequences. Key points:[^6][^2]
- Self-attention allows joint reasoning over image patches, text tokens, state tokens, and action tokens.
- Causal masking enforces autoregressive action generation conditioned on past observations and actions.
- Positional or time-step encodings index both observation frames and action horizons.[^11][^6]

Understanding encoder–decoder vs. decoder-only transformers, attention complexity, and multi-head attention is necessary to follow VLA architectures.[^6][^2]

### 2.2 Representation Learning

VLAs rely heavily on learned latent representations for perception and control. Typically:[^8][^6]
- A vision encoder (often a ViT or ConvNeXt variant) maps images to visual tokens.
- A text encoder or LLM embedding layer maps instructions to linguistic tokens.
- These tokens are projected into a shared embedding space so they can be processed jointly by a transformer backbone.[^2][^1]

Representation quality from large-scale pretraining (e.g., on web images and text) is crucial for generalization to novel objects, scenes, and language instructions.[^9][^3][^6]

### 2.3 Imitation Learning and Behavior Cloning

The dominant training paradigm for VLAs is imitation learning, specifically behavior cloning from expert demonstrations. Given a dataset of trajectories, the model is trained to maximize the likelihood of expert actions conditioned on observations and goals.[^10][^11][^6]

Extensions include sequence-level losses, such as predicting entire trajectories, and incorporating auxiliary prediction tasks (e.g., affordances, future states) to enrich representations and stabilize training.[^8][^9][^10]

### 2.4 Reinforcement Learning and Policy Optimization

While many VLAs rely purely on imitation, some incorporate reinforcement learning (RL) or offline RL to refine policies and address distribution shift. RL-based post-training can be used to:[^8][^6]
- Optimize long-horizon success beyond demonstration behavior.
- Fine-tune for specific deployments or safety constraints.
- Balance multi-objective rewards (e.g., success, efficiency, smoothness).

Understanding basic RL concepts (MDPs, value functions, policy gradients, off-policy learning) is helpful but not strictly necessary for basic VLA use.

## 3. Taxonomy of VLA Architectures

### 3.1 Unified View via Action Tokens

A recent survey proposes a unified view in which VLA models process vision and language inputs into a chain of action tokens that progressively encode more grounded and actionable information. The key design axis is how these action tokens are formulated, which can include:[^10]
- Natural language descriptions (e.g., high-level textual subgoals).
- Code or programs (e.g., API calls to motion planners).
- Affordances or object-centric descriptors.
- Trajectories, goal states, latent actions, or raw low-level controls.[^9][^10]

This perspective helps organize seemingly diverse architectures under a single sequence modeling framework.

### 3.2 High-Level Planner + Low-Level Controller

In planner–controller decompositions, a VLM or LLM serves as a high-level reasoner that, given images and language, outputs symbolic actions or subgoals (e.g., waypoints, skills), which are then executed by task-specific low-level policies.[^1][^8]

Examples include systems where:
- The planner outputs natural language sub-instructions for a library of skills.
- The planner outputs parameterized motion primitives that a classical controller executes.
These systems can be easier to interpret and debug but require substantial infrastructure for skill libraries and planning.

### 3.3 End-to-End VLAs (Single Transformer)

In end-to-end VLAs, a single transformer backbone takes image and language tokens and directly outputs action tokens. Architecturally:[^3][^2][^1]
- A vision encoder produces visual tokens from camera images.
- A projector maps vision tokens into the LLM’s embedding space.
- The transformer (often initialized from an LLM) processes \[text tokens, vision tokens, state tokens, action tokens\].
- During training, actions are teacher-forced; during inference, actions are generated autoregressively.[^3][^11][^1]

Models like OpenVLA-7B, RT-2/RT-X, and π0 fall into this general category, differing mainly in backbones, action tokenization, and training datasets.[^12][^5][^11][^3]

### 3.4 Hybrid and Hierarchical Designs

Hybrid VLAs combine multiple planning modalities, such as using a language model plus an image or video generator to predict intermediate visual goals, and then a low-level controller to reach them. Hierarchical VLAs may:[^8][^1]
- Predict high-level options or subgoals at a slow time scale.
- Predict low-level motor commands at a higher frequency.

Such designs can improve long-horizon performance and sample efficiency but are more complex to train and tune.

## 4. Model Components in Detail

### 4.1 Vision Encoders

VLAs typically use pretrained vision backbones such as ViT, ConvNeXt, or custom dual-branch encoders that handle wide and narrow field-of-view inputs. For example, the Prismatic-7B VLM underpinning OpenVLA uses a two-part visual encoder and then projects image embeddings into the LLM space.[^12][^6][^3]

Important design choices include:
- Resolution and patch size (trade-offs between detail and compute).
- Whether to freeze or fine-tune the vision encoder during robot training.
- Incorporation of depth or multi-view observations for better 3D understanding.[^6][^3][^8]

### 4.2 Language Backbones

Most VLAs use general-purpose LLMs (e.g., LLaMA derivatives) or smaller VLM text towers as the language backbone. These provide strong priors for instruction following, commonsense reasoning, and compositionality.[^2][^3][^1]

Key considerations:
- Freezing vs. fine-tuning the LLM during robot policy training.
- Prompt formats (e.g., system prompts framing the robot’s role, special tokens for instructions vs. observations).
- Multilingual capabilities if non-English commands are desired.[^3][^2]

### 4.3 Modality Projection and Tokenization

To combine vision and language, VLAs use projection layers that map visual features into the LLM’s embedding dimension. Tokenization design spans:[^1][^2]
- Image tokenization: patch tokens, region proposals, or object-centric tokens.
- Text tokenization: standard BPE or sentencepiece vocabularies.
- State tokens: concatenations of joint angles, end-effector poses, gripper states, etc., often linearly projected into embeddings.[^11][^1]

Proper normalization and scaling of state features are important to avoid dominance by any modality in attention layers.[^11][^1]

### 4.4 Action Representation and Tokenization

Action representation is a central design choice and a focal point of recent surveys. Common schemes include:[^9][^10]

- **Continuous actions:** Directly regress joint velocities or end-effector deltas using MSE or Gaussian losses, often via an MLP head on top of transformer outputs.
- **Discrete binning:** Discretize each action dimension into bins and treat actions as tokens, enabling purely autoregressive modeling.[^11][^1]
- **Vector quantization (VQ):** Learn a discrete codebook of action vectors, then generate code indices.[^10]
- **Compressed trajectory tokens:** Encode entire time windows using transforms (e.g., DCT) and generate compact tokens, as in FAST tokenization used by π0-FAST.[^11]

Discrete tokenization aligns naturally with LLM-style architectures but can struggle with high-frequency control and fine-grained precision.[^9][^10][^11]

### 4.5 Policy Heads and Decoding

Given the transformer outputs, VLAs attach policy heads to produce actions:
- For continuous actions, an MLP reads the final hidden state and outputs means (and possibly variances) for action distributions.
- For discrete tokens, a linear layer with softmax yields logits over action bins or codebook entries.[^1][^11]

During inference, actions are either sampled or taken as argmax, sometimes with temperature, top-k, or nucleus sampling to trade off exploration vs. stability.[^2][^11]

## 5. Training Paradigms

### 5.1 Offline Behavior Cloning at Scale

State-of-the-art VLAs are typically trained on hundreds of thousands to millions of robot demonstrations aggregated across labs and platforms, often using the RLDS format. Open X-Embodiment provides over one million real robot trajectories on 22 different robot embodiments, serving as a key substrate for models like RT-X and OpenVLA.[^4][^5][^3]

Training uses standard supervised learning with sequence modeling losses:
- Cross-entropy on discrete action tokens.
- Regression losses on continuous actions.
- Masking schemes to handle varying trajectory lengths.

Regularization techniques (dropout, layer norm, data augmentation) and curriculum strategies (simpler tasks first) are often used to stabilize training.[^10][^8][^9]

### 5.2 Multi-Stage Pretraining and Fine-Tuning

VLAs usually follow a multi-stage training pipeline:
- Stage 1: Pretrain vision and language backbones on generic multimodal data (images, captions, videos) to learn broad world knowledge.[^6][^3]
- Stage 2: Jointly train or adapt the backbone to robot data, learning alignment between multimodal context and actions (robot pretraining).
- Stage 3: Fine-tune on specific robot setups or tasks (e.g., your lab’s arm, home robot) using a smaller dataset of demonstrations.[^12][^3][^11]

Parameter-efficient fine-tuning (e.g., LoRA) is widely used to adapt large backbones without full retraining.[^12][^3][^9]

### 5.3 RL-Based Post-Training

Some works apply RL or offline RL on top of pre-trained VLAs to optimize long-horizon success, safety, or efficiency metrics. Strategies include:[^8][^6]
- Fine-tuning action heads with policy gradients while freezing most of the backbone.
- Using value functions or Q-functions conditioned on multimodal context.
- Incorporating human feedback for preference optimization.

Data efficiency and safety constraints make RL on real robots challenging, so this is usually a later-stage refinement.

### 5.4 Regularization, Data Balancing, and Curriculum

Large multi-source datasets like Open X-Embodiment are highly imbalanced across tasks, robots, and labs. Effective VLAs use:[^5][^4]
- Re-weighting or sampling strategies to prevent domination by a few large datasets.
- Task or robot identifiers as condition tokens.
- Difficult case mining or curriculum schedules to progressively expose the model to harder tasks.[^9][^10]

Understanding dataset curation is as important as architecture design for achieving robust performance.

## 6. Key Models and Case Studies

### 6.1 RT-2 and RT-X

RT-2 (Robotics Transformer 2) extended a vision-language model to output robotic actions directly, demonstrating zero-shot generalization from web-scale data to real-robot skills. RT-X and the Open X-Embodiment initiative generalized this to a cross-lab, cross-robot setting with over a million trajectories and many robot types, enabling a generalist manipulation backbone.[^4][^5][^3]

These models illustrate the power of scaling both data and model capacity for generalist robotics.

### 6.2 OpenVLA-7B

OpenVLA-7B is an open-source VLA model trained on approximately 970k manipulation episodes from Open X-Embodiment, built on a Prismatic-7B VLM with a LLaMA-2-7B language backbone. It takes language instructions and camera images as input and outputs normalized 7-DoF end-effector deltas \((x, y, z, \text{roll}, \text{pitch}, \text{yaw}, \text{gripper})\) suitable for controlling arms such as WidowX and others in the dataset.[^3][^12]

OpenVLA can be used zero-shot on embodiments present in its pretraining mix and is designed for parameter-efficient fine-tuning to new tasks or robot setups.[^12]

### 6.3 π0 and π0-FAST

π0 is a VLA model developed by Physical Intelligence that uses a Paligemma-based VLM backbone and an additional diffusion or flow-matching-based action expert to generate smooth action trajectories at up to 50 Hz. It is trained on a cross-embodiment robot dataset and uses action and state tokens appended after image and text tokens to represent policies.[^1][^11]

π0-FAST introduces FAST tokenization, which applies a discrete cosine transform (DCT) to action sequences, compresses coefficients, and yields a more efficient action token sequence. This yields up to 5× faster training compared to diffusion-based VLAs while improving action representation and generalization across robot morphologies.[^11]

### 6.4 Other Emerging Models (GR-1, RIPT-VLA, etc.)

Recent literature also explores humanoid- and quadruped-focused VLAs (e.g., GR-1 for general-purpose humanoid control and RIPT-VLA variants), which extend similar architectures to whole-body control, locomotion, and dexterous manipulation. These typically emphasize efficient tokenization, multi-sensor fusion (vision, proprioception, sometimes audio), and large-scale demonstration data from complex platforms.[^8][^9][^1]

## 7. Datasets and Data Infrastructure

### 7.1 Open X-Embodiment

Open X-Embodiment is a consolidated robotic dataset aggregating over one million real robot trajectories from 22 different platforms across many labs. It covers diverse tasks such as tabletop manipulation, mobile manipulation, and more, and standardizes data into a common format enabling cross-embodiment learning.[^5][^4]

It serves as the primary training substrate for RT-X and OpenVLA, and its scale and diversity are key to their generalization properties.[^4][^5][^3]

### 7.2 Other Important Datasets

Beyond Open X-Embodiment, the VLA ecosystem uses many datasets, including:
- BridgeData / BridgeV2 manipulation datasets, emphasizing table-top tasks and cross-domain variation.
- RLDS-format repositories for various simulators and real robots, enabling consistent processing and loading.[^7][^8]
- Proprietary or lab-specific datasets from platforms like humanoids, quadrupeds, or mobile manipulators.

Understanding dataset schemas (e.g., RLDS) and metadata (robot type, task ID, camera intrinsics) is critical for training and evaluating VLAs.[^7][^8]

### 7.3 Data Collection Pipelines

Data collection for VLAs typically involves:
- Teleoperation (e.g., VR controllers, haptic devices) to generate high-quality demonstrations.
- Corrective interventions or shared autonomy to improve data quality.
- Automatic labeling of language instructions, either manually written or generated by LLMs from task metadata.[^6][^8]

High-quality, diverse demonstrations with accurate synchronization between sensors, state, and actions are essential for successful behavior cloning.

## 8. Simulation and Embodied Environments

### 8.1 Role of Simulation

Simulation environments such as MuJoCo, Isaac Lab, PyBullet, Habitat, and RoboSuite are heavily used for data generation, pretraining, and evaluation of embodied policies. Simulation offers:[^13][^7][^8]
- Cheap, safe data collection for long horizons.
- Systematic domain randomization for sim-to-real transfer.
- Easy logging, manipulation, and reproducibility.

However, bridging the gap between sim and real remains challenging, especially for contact-rich manipulation and complex perception in cluttered environments.[^13][^7]

### 8.2 Common Simulation Platforms

Key platforms include:
- MuJoCo and RoboSuite for manipulation and locomotion tasks with high-fidelity physics.
- Isaac Lab/Isaac Sim for GPU-accelerated simulation, photorealistic rendering, and tight integration with NVIDIA hardware.
- Habitat and Gibson for navigation and embodied AI tasks in indoor environments.[^7][^13][^8]

Familiarity with these platforms enables scalable data collection and controlled evaluation for VLA research.

## 9. Evaluation and Benchmarks

### 9.1 Task Success and Generalization Metrics

VLAs are typically evaluated via:
- Task success rate over a set of language-specified tasks and initial conditions.
- Generalization to unseen objects, scenes, or language instructions (e.g., held-out object categories).
- Robustness to distribution shifts between training and evaluation setups.[^7][^9][^8]

Experiments often report per-task success, aggregate success, and performance broken down by robot embodiment or task family.[^5][^3]

### 9.2 Evaluation Challenges

Evaluating robotic foundation models is difficult due to inconsistent benchmarks, diverse hardware, and limited shared evaluation protocols. Challenges include:[^13][^7]
- Reproducing real-robot evaluations across labs.
- Accounting for human operator differences during deployment.
- Measuring long-horizon stability and safety beyond per-episode success.[^13][^7]

Recent tutorials emphasize the need for standardized evaluation suites and community-shared benchmarks for embodied agents.[^7][^13]

### 9.3 Sim-to-Real Performance

A key question is how well VLAs trained in simulation or on mixed sim/real data transfer to new real-world settings. Techniques such as domain randomization, randomized textures, and sensor noise injection are used to improve sim-to-real robustness.[^7][^8]

Models like OpenVLA and π0 primarily rely on real demonstration data, reducing the sim-to-real gap but increasing the burden of large-scale real-robot data collection.[^4][^3][^11]

## 10. Practical Design and Implementation Considerations

### 10.1 Choosing an Architecture

For many practical applications, starting from an existing backbone such as OpenVLA-7B or π0 and adapting it via fine-tuning is more efficient than training from scratch. The choice between planner–controller vs. end-to-end VLA depends on factors such as:[^3][^12][^11]
- Available data scale and diversity.
- Need for interpretability and modularity.
- Latency and compute constraints on the target hardware.[^2][^9]

For small labs, end-to-end VLAs with parameter-efficient adaptation on a single robot platform are often the most pragmatic path.

### 10.2 Training Infrastructure

Training large VLAs requires substantial compute and careful engineering:
- Multi-GPU or multi-node training with efficient data pipelines.
- Mixed-precision training and gradient checkpointing to manage memory.
- Strong logging, checkpointing, and evaluation infrastructure.[^10][^9]

Frameworks based on PyTorch, JAX/Flax, or DeepSpeed/Megatron-style scaling libraries are commonly used.[^9][^10]

### 10.3 Deployment on Real Robots

Deployment pipelines involve:
- Converting normalized actions back to robot-specific control commands (e.g., scaling end-effector deltas, enforcing joint limits).[^12]
- Integrating with robot middleware (ROS, custom drivers) and safety monitors.
- Handling real-time constraints by optimizing inference (quantization, distillation, or smaller backbones).[^2][^9]

Close-loop integration with perception modules (e.g., object detectors, state estimators) and safety layers is essential in real deployments.

### 10.4 Safety, Reliability, and Human-in-the-Loop

VLAs must operate safely around humans and valuable environments. Common practices include:[^7][^8]
- Conservative action clipping and emergency stop mechanisms.
- Human-in-the-loop supervision and intervention interfaces.
- Logging and auditing of actions for post-hoc analysis.

Some research explores aligning VLA behavior with human preferences and safety constraints using human feedback or explicit safety critics.[^8][^9]

## 11. Efficient and Resource-Constrained VLAs

### 11.1 Efficiency Challenges

VLAs inherit the heavy compute and memory demands of large vision and language models, making deployment on embedded or edge hardware challenging. Bottlenecks include:[^9][^8]
- Large backbone size (billions of parameters).
- High-frequency action generation requirements for control.
- Multi-camera or high-resolution inputs.

### 11.2 Efficient Model Design and Compression

Recent work on efficient VLAs focuses on:
- Smaller backbones, knowledge distillation, and pruning.
- Parameter-efficient fine-tuning (LoRA, adapters) to avoid full-model updates.
- More compact action tokenization (e.g., FAST) to reduce sequence length and compute.[^11][^9]

These techniques can significantly reduce training and inference cost without severely degrading performance.

### 11.3 Efficient Data Collection and Training

Efficient VLA surveys emphasize that data collection is a major bottleneck and propose strategies such as active data collection, demonstration selection, and synthetic augmentation. Efficient training methods include:[^8][^9]
- Curriculum learning over tasks and embodiments.
- Sampling and reweighting schemes to focus compute on underrepresented or challenging trajectories.
- Partial finetuning or frozen modality towers to reduce gradient computation.[^10][^9]

These approaches are crucial for bringing VLA techniques to smaller labs and real-world deployments.

## 12. Recommended Learning Path and Further Reading

### 12.1 Conceptual Checklist

To become comfortable with VLAs, a learner should master:
- Transformer architectures and multimodal representation learning.[^6][^2]
- Imitation learning and behavior cloning from sequence data.[^6][^10]
- Action representation and tokenization strategies for control.[^10][^11]
- Large-scale dataset curation and RLDS-style data formats.[^4][^7]
- Evaluation protocols for embodied agents and generalist robot policies.[^13][^7][^8]

### 12.2 Key Surveys and Tutorials

Useful references include:
- Recent comprehensive surveys on VLA models and efficient VLA design, which organize the field and emphasize action tokenization.[^9][^10]
- Systematic reviews on multimodal fusion with VLAs in robotics.[^8]
- Tutorials on foundation models for embodied agents, highlighting evaluation challenges and design principles.[^13][^7]
- Practitioner-oriented overviews and blog posts explaining VLAs and robotics foundation models at a high level.[^14][^15][^1][^2]

Combined, these resources and the concepts in this document provide a solid foundation for engaging with current VLA research and implementing practical systems.

---

## References

1. [Vision Language Action Models (VLA) & Policies for Robots](https://learnopencv.com/vision-language-action-models-lerobot-policy/) - In this article, we will go in-depth discussing how VLAs have evolved post GPT era with compelling p...

2. [A Comprehensive Overview of Vision-Language-Action ...](https://www.digitalocean.com/community/conceptual-articles/vision-language-action-models) - A VLA model is a type of artificial intelligence model, typically built on a transformer architectur...

3. [OpenVLA to RT-2-X is what Llama-3 is to GPT-4 - TechTalks](https://bdtechtalks.substack.com/p/openvla-to-rt-2-x-is-what-llama-3) - The foundation for OpenVLA is Prismatic-7B, a VLM that uses a two-part visual encoder for extracting...

4. [Open X-Embodiment Dataset](https://www.emergentmind.com/topics/open-x-embodiment-dataset) - Open X-Embodiment is a consolidated, large-scale dataset aggregating robotic demonstrations from 22 ...

5. [Open X-Embodiment: Robotic Learning Datasets and RT-X ...](https://robotics-transformer-x.github.io) - Dataset Overview. We introduce the Open X-Embodiment Dataset, the largest open-source real robot dat...

6. [How Visual-Language-Action (VLA) Models Work](https://towardsdatascience.com/how-visual-language-action-vla-models-work/) - Understanding the distinctions between raisins, green peppers, and a salt shaker might seem trivial,...

7. [Lecture 17: Robot Learning](https://cs231n.stanford.edu/slides/2025/lecture_17.pdf) - Foundation Models for Embodied Agents. ❑ Current foundation models are not tailored for embodied age...

8. [Multimodal fusion with vision-language-action models for ...](https://www.sciencedirect.com/science/article/pii/S1566253525011248) - This systematic review synthesizes the state of the art in VLA research with an emphasis on architec...

9. [A Survey on Efficient Vision-Language-Action Models](https://huggingface.co/papers/2510.24795) - This survey reviews Efficient Vision-Language-Action models, addressing computational and data chall...

10. [[2507.01925] A Survey on Vision-Language-Action Models](https://arxiv.org/abs/2507.01925) - This survey aims to categorize and interpret existing VLA research through the lens of action tokeni...

11. [Vision-Language-Action Models for General Robot Control](https://huggingface.co/blog/pi0) - π0 (Pi-Zero) is a Vision-Language-Action (VLA) model, developed by the Physical Intelligence team de...

12. [openvla/openvla-7b - Hugging Face](https://huggingface.co/openvla/openvla-7b) - OpenVLA 7B ( openvla-7b ) is an open vision-language-action model trained on 970K robot manipulation...

13. [ICCV 2025 Tutorial Time](https://foundation-models-meet-embodied-agents.github.io/uploads/FM-EA-challenges.pdf) - ICCV Tutorial: Foundation Models Meet Embodied Agents. Page 3. The Evaluation Problem of the Robotic...

14. [Vision-Language-Action (VLA) Models: Concepts, ...](https://arxiv.org/html/2505.04769v2) - The presentation begins with an introduction that motivates embodied intelligence and the emergence ...

15. [Foundation Models for Robotics: Vision-Language-Action ...](https://rohitbandaru.github.io/blog/Foundation-Models-for-Robotics-VLA/) - ML blog.

