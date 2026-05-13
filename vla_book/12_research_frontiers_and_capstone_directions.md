# Chapter 12 - Research Frontiers and Capstone Directions

## 12.1 Active research themes

- Better action tokenization with lower latency and higher precision.
- Long-horizon planning with multimodal memory.
- Strong sim-to-real transfer under realistic sensing noise.
- Cross-embodiment scaling laws and universal action interfaces.
- Safety-aware alignment and verifiable constraints in learned policies.

## 12.2 Open technical questions

1. How should we represent actions so that language-model scaling benefits transfer to control without losing precision?
2. What is the best split between end-to-end learning and modular control for reliability?
3. How can we measure and guarantee safe behavior under unseen instructions/environments?
4. How should world models and VLAs be combined for long-horizon tasks?

## 12.3 Capstone project tracks

### Track A: End-to-end VLA in simulation
- Build a baseline VLA policy for a multitask manipulation simulator.
- Compare continuous vs tokenized action outputs.
- Report transfer to unseen objects/instructions.

### Track B: Hybrid planner-controller system
- Use language/vision model for high-level decomposition.
- Execute with conventional low-level controllers.
- Evaluate reliability vs pure end-to-end VLA.

### Track C: Safety-first VLA deployment prototype
- Implement intervention-aware runtime stack.
- Quantify safety interventions and recovery success.
- Produce governance-ready logs and incident analyses.

## 12.4 Suggested thesis-level extensions

- Preference-tuned VLA for human style alignment.
- Cross-domain adaptation (warehouse to household manipulation).
- Embodied reasoning traces for interpretable action generation.

