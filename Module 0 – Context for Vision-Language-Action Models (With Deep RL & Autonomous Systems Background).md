# Module 0 – Context for Vision-Language-Action Models (With Deep RL & Autonomous Systems Background)

## 0.1 What This Module Assumes

This module assumes familiarity with Markov decision processes, value and policy iteration, policy gradients, actor–critic methods, and deep RL implementations, as well as exposure to planning, perception, and control architectures from a principles-of-autonomous-systems course. The goal is not to reteach RL or autonomous systems, but to reposition that knowledge in the context of robotics foundation models and VLAs.[^1][^2]

VLAs are best understood as **robotic foundation models**: large, pretrained multimodal models that map sensory observations and language directly to actions and are then adapted to specific robots or tasks. This module frames VLAs within embodied AI and robotics foundation models so later modules can focus on architecture and training details.[^3][^4][^5]

## 0.2 From Classical Autonomy to Robotics Foundation Models

Classical autonomous systems courses typically decompose the stack into perception → state estimation → planning → control, with each module designed and tuned separately. Foundation models in robotics instead advocate building a single, very large model that encodes rich priors about perception, language, and control, and then adapting it to particular embodiments and tasks.[^4][^6][^7][^1]

Robotics foundation models draw inspiration from NLP foundation models (GPT, BERT) and vision models, but extend them to embodied settings where agents must act in continuous environments with real-time constraints. VLAs form one concrete instantiation of this idea, integrating vision, language, and action into a unified sequence model that can be reused across many robots and tasks.[^5][^8][^3][^4]

## 0.3 Embodied AI, Physical AI, and Where VLAs Sit

Embodied AI refers to AI systems that are embedded in physical bodies—robots, vehicles, or smart spaces—that perceive, reason, and act in the real world. These systems couple perception, prediction, planning, and control under physical constraints, often using simulation and real-world data jointly.[^6][^9]

Within this landscape, VLAs are **policy-level models** that take multimodal observations (images, text, state) and output actions, sitting downstream of low-level actuation and often upstream of safety and constraint layers. They can be paired with world models, trajectory optimizers, or traditional planners, but they themselves are not full autonomy stacks—they are reusable policy components for embodied agents.[^7][^8][^3][^5]

## 0.4 Why VLAs after Deep RL?

Deep RL provides a principled framework for optimizing policies from interaction data but is notoriously data- and compute-hungry on real robots, with limited generalization across tasks and embodiments. In contrast, VLAs are primarily trained via large-scale imitation learning and supervised sequence modeling on diverse offline datasets, then optionally refined with RL.[^2][^8][^3][^5]

From a deep RL perspective, VLAs can be viewed as **strongly regularized, pretrained policy priors** that make RL feasible in high-dimensional, sparse-reward robotic tasks by providing good initial policies and representations. Instead of learning from scratch per task, RL fine-tunes a multimodal foundation that already encodes perception, language grounding, and basic manipulation or navigation skills.[^9][^3][^2]

## 0.5 Comparing Stacks: Classical vs. VLA-Centric

The classical robotics stack uses modular perception, planning, and control with clear interfaces (e.g., costmaps, waypoints, setpoints), while VLA-centric stacks collapse much of this into a single learned mapping from sensory inputs and language to actions. In practice, deployed systems often adopt hybrids: foundation models for perception and high-level decision-making, combined with traditional planners and controllers to ensure safety and performance guarantees.[^3][^1][^6][^7]

VLAs also change how data flows: instead of carefully engineered features and task-specific datasets, data collection focuses on large, heterogeneous demonstration corpora across many tasks and robots, plus web-scale vision–language corpora for pretraining. For someone with an autonomous systems background, the key shift is from **designing algorithms around a fixed model of the robot and environment** to **designing datasets and interfaces around a large, adaptable model**.[^4][^5][^7][^3]

## 0.6 Formal Positioning of VLAs

In the standard MDP or POMDP formalism, a VLA implements a parameterized policy \(\pi_\theta(a_t \mid o_t, g)\) where observations \(o_t\) include images and proprioception, and \(g\) is a language goal or instruction. Unlike conventional policies parameterized by small neural networks, \(\pi_\theta\) is often a billion-parameter transformer initialized from an LLM or VLM and trained with sequence modeling objectives on \((o_{1:T}, g, a_{1:T})\) trajectories.[^8][^5]

The distinction from pure VLMs is the addition of **action tokens or heads** as a first-class modality, making actions part of the same sequence as vision and language and enabling multi-step reasoning over all three. From an algorithmic perspective, VLAs blur the line between policy and world model: they may implicitly encode affordances and short-horizon dynamics through their training data, even when no explicit model-based RL is used.[^10][^11][^8][^3]

## 0.7 Relationship to World Models and Planning

World models, as used in model-based RL, aim to learn state transition dynamics and reward structure to enable planning or imagination rollouts. VLAs, by contrast, are typically trained to directly map context to actions without an explicit transition model, though they can be combined with world models for long-horizon planning or uncertainty estimation.[^9][^3]

For someone with a planning background, it can be useful to view VLAs as **learned reactive policies with rich context** that can serve as policy primitives inside larger planners, such as task and motion planners or option-based hierarchies. Research at the interface of VLAs and world models is exploring how to combine generative "world foundation models" with action-generating policies to enable better foresight and safety.[^12][^6][^3][^9]

## 0.8 Robotics Foundation Models Landscape

The broader robotics foundation models landscape includes LLMs used as planners, VLMs for perception and grounding, multimodal generative models for world simulation, and VLAs for action generation. Surveys and blog posts emphasize that the unifying theme is **pretraining on diverse, large-scale datasets followed by task- or robot-specific adaptation**, analogous to how LLMs are adapted via instruction tuning.[^13][^7][^3][^4]

VLAs occupy the portion of this landscape where actions are explicit outputs, often alongside language, enabling instruction-conditioned, generalist policies that can be deployed on multiple robots with minimal per-robot modification. Understanding where VLAs sit relative to other foundation model components will help when designing full autonomy stacks that blend them with classical modules.[^5][^8]

## 0.9 Why This Matters for Your Learning Path

Given prior exposure to deep RL and autonomous systems, the main added value of VLAs and robotic foundation models is in **scaling and reuse**: rather than solving each robot MDP from scratch, you leverage a shared multimodal backbone across tasks, robots, and labs. This changes the research questions from "how do I design a reward or planner for this task?" to "how do I design the data, interfaces, and fine-tuning procedure so a single model can handle many tasks safely and efficiently?"[^7][^3][^4]

Module 0 is therefore about reframing: treat VLAs as a new kind of policy class and robotics foundation models as a new systems design paradigm on top of the RL and autonomy principles you already know. Subsequent modules will then dive into backbone architectures, action tokenization, datasets, and training pipelines with this framing in mind.[^3][^5]

---

## References

1. [Foundation Models for Robotics Series Part 1- Introduction](https://www.kosiasuzu.com/posts/1foundation-models-for-robotics-introduction) - Foundation models enable us to train models that generalize to different applications, tasks and emb...

2. [Robotic Foundation Models | Actuate 2025](https://actuate.foxglove.dev/recordings/robotic-foundation-models) - Sergey Levine and Liyiming Ke from Physical Intelligence explain how foundation models enable robots...

3. [Foundation Models in Robotics: Applications, Challenges ...](https://arxiv.org/html/2312.07843v1) - We explore how foundation models contribute to improving robot capabilities in the domains of percep...

4. [Robotics foundation models](https://nairl.kr/robotics-foundation-models/) - Robotics foundation models are large-scale machine learning models designed to serve as versatile bu...

5. [Foundation Models for Robotics: Vision-Language-Action ...](https://rohitbandaru.github.io/blog/Foundation-Models-for-Robotics-VLA/) - ML blog.

6. [Embodied AI Explained: Principles, Applications, and ...](https://lamarr-institute.org/blog/embodied-ai-explained/) - Embodied AI refers to AI that is integrated into physical systems, such as robots, enabling them to ...

7. [Robotics Foundation Models and the role of data](https://covariant.ai/insights/the-future-of-robotics-robotics-foundation-models-and-the-role-of-data/) - Foundation models have transformed artificial intelligence. The growth trajectory of robotics founda...

8. [Vision-language-action model](https://en.wikipedia.org/wiki/Vision-language-action_model) - In robot learning, a vision-language-action model (VLA) is a class of multimodal foundation models t...

9. [What is Embodied AI? | NVIDIA Glossary](https://www.nvidia.com/en-us/glossary/embodied-ai/) - Embodied AI refers to the integration of artificial intelligence into physical systems, enabling the...

10. [[2510.07077] Vision-Language-Action Models for Robotics](https://arxiv.org/abs/2510.07077) - Throughout this comprehensive survey, this paper aims to offer practical guidance for the robotics c...

11. [Vision Language Action Models (VLA) & Policies for Robots](https://learnopencv.com/vision-language-action-models-lerobot-policy/) - In this article, we will go in-depth discussing how VLAs have evolved post GPT era with compelling p...

12. [The rise of embodied AI: Robots that learn by doing](https://pal-robotics.com/news/the-rise-of-embodied-ai-robots-that-learn-by-doing/) - Embodied AI enables robots to learn by doing; connecting perception, cognition, and action through d...

13. [Foundation Models for Robot Design](https://www.frontiersin.org/research-topics/70813/foundation-models-for-robot-designundefined) - The emergence of robotics foundation models, including large language models (LLMs), vision-language...

