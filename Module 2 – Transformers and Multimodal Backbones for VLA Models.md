# Module 2 – Transformers and Multimodal Backbones for VLA Models

## 2.1 Module Goals (Given Deep RL & Autonomy Background)

This module explains how transformer architectures and multimodal backbones are used in VLA models, assuming familiarity with neural networks and sequence modeling from deep RL.[^1][^2] It focuses on the specific design patterns that enable joint processing of vision, language, and robot state, rather than re-deriving basic attention.

By the end, you should be able to:
- Interpret architecture diagrams of VLM/VLA backbones and map them onto encoder/decoder/decoder-only transformer variants.[^2][^3]
- Understand how vision encoders, language decoders, and projection layers are composed into a single multimodal policy for robotics.[^4][^3][^5]

## 2.2 Transformers as the Default Policy Class in Modern Robotics

Modern robotic foundation models overwhelmingly use transformers as their core architecture due to their scalability, ability to model long-range dependencies, and compatibility with internet-scale pretraining.[^5][^6][^7] In the robotics context, transformers process sequences of multimodal tokens: image patches, text tokens, and state or action tokens.

Lecture-style resources on transformers for robotics emphasize that virtually all recent generalist policies—ACT, RT-style models, Groot, π0, OpenVLA, and SmolVLA—are built on transformer architectures.[^1][^8] For someone coming from deep RL, transformers replace the smaller MLP or CNN policy networks typically used in RL with a much larger, sequence-oriented model class that can ingest trajectory histories and multiple modalities simultaneously.

## 2.3 Transformer Variants: Encoder, Decoder, and Encoder–Decoder

Standard transformer variants include encoder-only (e.g., BERT), decoder-only (e.g., GPT), and encoder–decoder (sequence-to-sequence) architectures.[^2] Most large language models used as backbones in VLAs are decoder-only, while vision encoders may be transformer-based (ViT) or CNN-like, and are often treated as separate modules.[^4][^3]

In VLMs such as LLaVA, a typical pattern is: a frozen vision encoder (e.g., CLIP image encoder or ViT), a multimodal projection layer, and a decoder-only language model that generates text conditioned on both image and text embeddings.[^3] VLAs largely follow this pattern but extend the decoder stage to generate actions instead or in addition to text, retaining the decoder-only core for auto-regressive generation of action sequences.[^5][^9]

## 2.4 Vision Encoders: From CNNs to Vision Transformers (ViT)

Vision encoders in VLMs and VLAs extract visual features that are later fused with language and state information.[^4][^3] Earlier systems used CNN-based backbones, but modern VLMs and robotics foundation models increasingly adopt ViTs, which treat images as sequences of patches and apply transformer-style self-attention.[^4][^10]

ViT-based encoders produce a sequence of image tokens representing patches or regions, which align naturally with the token-based processing in transformer decoders.[^4][^3] For robotics, these tokens often capture both global context and local details in scenes relevant for manipulation or navigation, and can be extended to multiple camera views or depth modalities.[^5][^10]

## 2.5 Language Backbones: Decoder-Only LMs for Conditioning and Action Generation

Language backbones in VLA systems are usually large decoder-only LMs (e.g., LLaMA variants) that are either frozen or partially fine-tuned when adapting to robotics.[^5][^2] They provide strong priors for instruction following, commonsense reasoning, and compositionality, which is crucial for mapping natural language tasks to action sequences.

From a structural standpoint, these backbones receive a sequence of text tokens (instructions, context) and multimodal tokens (projected image and state embeddings) and then generate a continuation sequence, which in VLAs includes action outputs.[^5][^9] This architecture leverages the same auto-regressive token prediction mechanisms that make LLMs effective, but repurposes them for control rather than pure text generation.[^2][^3]

## 2.6 Multimodal Fusion: Projection Layers and Token Concatenation

The core engineering challenge in VLMs and VLAs is multimodal fusion: combining vision, language, and sometimes additional modalities in a single model.[^11][^12] A common strategy is to keep modality-specific encoders (vision encoder, language tokenizer/embedding layer, state encoder) and then use projection layers to map all outputs into a shared embedding space.

Typical VLM structures concatenate projected image embeddings with text token embeddings and feed the resulting sequence into a transformer decoder, sometimes with modality-specific tokens or prompts to distinguish segments.[^3][^4] In robotics foundation models, this idea extends to proprioceptive or trajectory tokens, creating unified sequences like:

$$
[\text{text tokens}, \text{image tokens}, \text{state tokens}, \text{(optional past action tokens)}]
$$
which are processed jointly to produce future actions.[^5][^13]

## 2.7 Specialized Transformer Designs for VLAs (e.g., Actra)

While many VLAs use vanilla causal transformers, recent work explores architectures optimized for segmented multimodal trajectories.[^14][^13] Actra, for example, introduces "trajectory attention" and learnable action queries, designed to better process segmented multimodal trajectories in language-conditioned imitation learning.[^14][^13]

Actra pairs this architecture with a contrastive dynamics learning objective to explicitly encode environment dynamics and multimodal alignment, complementing the behavior cloning loss.[^14] This illustrates a trend: transformer backbones are being refined with trajectory-aware attention patterns and auxiliary objectives tailored to robot control, rather than reusing generic LLM designs verbatim.[^13][^6]

## 2.8 Transformers vs. Alternative Architectures in Robotics

There is ongoing debate about whether transformers are truly foundational for robotics or merely convenient given current compute and data trends.[^7] Critics note their heavy resource demands, real-time constraints, and potential mismatch with low-power, embedded platforms common in robotics.[^7][^10]

Nonetheless, surveys of foundation models in robotics suggest that, for now, transformers dominate for large-scale multimodal learning due to their flexibility and compatibility with sequence data and internet-scale pretraining.[^6][^9] Your deep RL background will be helpful when thinking about alternatives (e.g., smaller recurrent architectures, world models with separate policy heads) and trade-offs between model class and deployment constraints.

## 2.9 Connecting Back to Deep RL Policy Architectures

In deep RL, policies are typically MLPs or CNNs mapping states or images to actions, sometimes with recurrent layers for partial observability.[^6] In VLAs, transformers generalize this pattern to **multi-token, multi-modal sequences**: instead of a single state vector, you feed a sequence of tokens representing a history of observations and instructions.

This shift has several implications:
- Policies can condition on long context windows of past observations and language, analogous to recurrent RL but with more flexible attention mechanisms.[^1][^2]
- The same backbone can be pretrained on non-robot data (images, captions, text corpora) and then adapted to robot control, effectively serving as a rich prior over policies.[^5][^6]
- Architectural decisions (e.g., where to insert state tokens, how to segment trajectories) become as important as reward design or exploration strategy in classical RL.

## 2.10 Suggested Reading for Module 2

To deepen understanding of transformers and multimodal backbones in robotics:
- Review a transformer architecture tutorial focusing on encoder, decoder, and encoder–decoder variants and their use in LLMs.[^2]
- Read an introductory or blog-style explanation of VLMs, such as IBM’s overview and Hugging Face’s VLM blog, paying attention to how vision encoders and language decoders are connected via projection and token concatenation.[^4][^3]
- Skim sections of robotics foundation model surveys or overviews that describe typical multimodal backbones used in RT-style models and OpenVLA-like architectures.[^5][^6][^8]
- Read the Actra paper as a concrete example of a VLA-specific transformer design that deviates from vanilla causal attention.[^14][^13]

## 2.11 Recommended Exercises (Leveraging Your Background)

1. **Implement a Minimal Multimodal Transformer Policy (Simulation)**
   - Build a small transformer-based policy in PyTorch that takes as input a sequence of image embeddings (from a frozen CNN or ViT) and a text-encoded goal, and outputs actions for a simple environment (e.g., gridworld or a toy manipulation sim).
   - Compare its behavior and data efficiency with a baseline CNN+MLP policy trained with behavior cloning or RL for the same task.

2. **Reverse-Engineer a VLM-to-VLA Adaptation**
   - Starting from an open VLM design (e.g., LLaVA-style), sketch how you would adapt it to a VLA by:
     - Deciding where to insert state tokens.
     - Designing a projection for state features.
     - Choosing whether actions are continuous heads or discrete tokens.
   - Relate each choice to your understanding of RL policy architectures (e.g., value of recurrence vs. feedforward).

3. **Analyze a VLA Architecture Diagram**
   - Take a publicly available architecture diagram from a robotics foundation model (e.g., an ACT-like or OpenVLA-style figure) and annotate it with encoder/decoder roles, token types, and projection layers.
   - For each component, articulate its analogue in a classical autonomy stack (e.g., vision encoder ↔ perception, transformer trunk ↔ high-level decision module, action head ↔ policy).

Working through these exercises will give you a concrete mental model of how transformers serve as the backbone of multimodal policies in VLAs, building on your deep RL understanding of policy architectures and representation learning.

---

## References

1. [Lecture 4 - The Transformer Architecture for Robotics - YouTube](https://www.youtube.com/watch?v=8otn0N8Lwkw) - ... Vision Language Action Model (3) Pi0 - Vision Language Action Model (4) SmolVLA - Vision Languag...

2. [Transformer Architectures - Hugging Face](https://huggingface.co/learn/llm-course/chapter1/6) - Most modern Large Language Models (LLMs) use the decoder-only architecture. These models have grown ...

3. [Vision Language Models Explained - Hugging Face](https://huggingface.co/blog/vlms) - Vision language models are broadly defined as multimodal models that can learn from images and text....

4. [What Are Vision Language Models (VLMs)? - IBM](https://www.ibm.com/think/topics/vision-language-models) - Vision language models (VLMs) are artificial intelligence (AI) models that blend computer vision and...

5. [4. Experimental Methods...](https://www.emergentmind.com/topics/robotic-foundation-models) - Robotic foundation models are large-scale pre-trained neural architectures that unify vision, langua...

6. [[PDF] Foundation models in robotics: Applications, challenges, and the ...](https://par.nsf.gov/servlets/purl/10597603)

7. [Are transformers truly foundational for robotics?](https://www.nature.com/articles/s44182-025-00025-4) - Generative Pre-Trained Transformers (GPTs) are hyped to revolutionize robotics. Here we question the...

8. [Foundation Models for Manipulation: Overview - GitHub](https://github.com/Argo-Robot/foundation_models) - Overview about state-of-art imitation learning techniques for robotic manipulation, enabling general...

9. [Foundation Models in Robotics - Niraj Basnet](https://nirajbasnet.github.io/blog-foundation-models.html)

10. [Robots That See, Part 2: Vision Transformers - Creative Strategies](https://creativestrategies.com/robots-that-see-part-2-vision-transformers/) - Here's the Vision Transformer architecture (left) and the standard language Transformer (right). Not...

11. [Multimodal fusion and vision–language models: A survey for robot ...](https://www.sciencedirect.com/science/article/abs/pii/S1566253525007249) - Comprehensive survey of multimodal fusion and VLMs for robotic vision tasks. · Extend beyond segment...

12. [[2504.02477] Multimodal Fusion and Vision-Language Models - arXiv](https://arxiv.org/abs/2504.02477) - Robot vision has greatly benefited from advancements in multimodal fusion techniques and vision-lang...

13. [Actra: Optimized Transformer Architecture for Vision-Language ...](https://arxiv.org/html/2408.01147v1) - In this paper, we introduce Actra, an optimized Transformer architecture featuring trajectory attent...

14. [Actra: Optimized Transformer Architecture for Vision-Language ...](https://openreview.net/forum?id=PPDheO2z5v) - An optimized Transformer architecture for vision-language-action models, designed to process multimo...

