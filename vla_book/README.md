# Vision-Language-Action (VLA) Book Notes

This book is now aligned to the 6-phase, 14-week flow used in the awesome-vla-study curriculum: generative foundations, early robot foundation models, modern VLA architectures, data scaling, efficient inference, and post-training with RL, reasoning, and world models.

## Reading philosophy

Use the book in two passes:
1. **Paper-flow pass** - follow the 14-week curriculum so each chapter has concrete anchor papers.
2. **Systems pass** - reread the book by design question: action interface, data engine, optimization, runtime loop, evaluation, safety, and research direction.

## How to read this book

1. Read Chapters 00–02 together with Phase 1 to connect VLA math to diffusion, flow matching, and sequence modeling.
2. Read Chapters 03–04 with Phase 2 and Phase 3 to understand the evolution from RT-1 and RT-2 to OpenVLA, CogACT, and π0-style systems.
3. Read Chapters 05–06 with Phase 4 to connect data design, RLDS-style schemas, and large-scale optimization practice.
4. Read Chapters 07–08 with Phase 5 and Week 12 of Phase 6 to connect post-training, latency, and dual-system runtime design.
5. Read Chapters 09–12 with the final Phase 6 weeks to frame evaluation, safety, reasoning, world models, and open research questions.

## Chapter map

- [00 - Orientation and Learning Strategy](./00_orientation_and_learning_strategy.md)
- [01 - VLA Foundations and Taxonomy](./01_vla_foundations_and_taxonomy.md)
- [02 - Problem Formulation and Mathematical View](./02_problem_formulation_and_math_view.md)
- [03 - Multimodal Architectures for VLAs](./03_multimodal_architectures_for_vlas.md)
- [04 - Action Spaces, Tokenization, and Control Interfaces](./04_action_spaces_tokenization_and_control_interfaces.md)
- [05 - Data, Demonstrations, and Dataset Design](./05_data_demonstrations_and_dataset_design.md)
- [06 - Training Objectives and Optimization Pipelines](./06_training_objectives_and_optimization_pipelines.md)
- [07 - RL, Planning, and Post-Training for VLAs](./07_rl_planning_and_post_training_for_vlas.md)
- [08 - Inference-Time Behavior, Prompting, and Control Loops](./08_inference_time_behavior_prompting_and_control_loops.md)
- [09 - Evaluation, Benchmarking, and Error Analysis](./09_evaluation_benchmarking_and_error_analysis.md)
- [10 - Safety, Reliability, and Governance for Physical Deployment](./10_safety_reliability_and_governance_for_physical_deployment.md)
- [11 - System Design, Tooling, and Deployment Engineering](./11_system_design_tooling_and_deployment_engineering.md)
- [12 - Research Frontiers and Capstone Directions](./12_research_frontiers_and_capstone_directions.md)
- [13 - Extended References: Papers, Tutorials, and Books](./13_extended_references_papers_tutorials_and_books.md)
- [Appendix - Glossary and Quick Revision Notes](./appendix_glossary_and_quick_revision.md)

## 14-week study map

### Phase 1 - Generative foundations
- **Week 1:** Chapters 00-02 alongside diffusion, ODE/SDE, and score-model basics.
- **Week 2:** Revisit Chapter 02 and Chapter 06 while studying flow matching and guidance.
- **Week 3:** Bridge Chapter 04 and Chapter 07 to generative action models in robotics.

### Phase 2 - Early robot foundation models and core policy ideas
- **Week 4:** Chapters 01 and 03 with RT-1, RT-2, Octo, and OpenVLA.
- **Week 5:** Chapters 04 and 06 with BeT, Diffusion Policy, and ACT.

### Phase 3 - Current architectures
- **Week 6:** Chapter 03 with CogACT, GR00T N1, and X-VLA.
- **Week 7:** Chapters 03, 04, and 08 with π0, InternVLA-M1, and Transfusion background.

### Phase 4 - Data scaling
- **Week 8:** Chapters 05, 06, and 11 with OXE, AgiBot World, RLDS, LeRobotDataset, rosbag, and MCAP.
- **Week 9:** Chapters 05 and 09 with UMI, VITRA, and human-to-robot transfer work.

### Phase 5 - Efficient inference and dual-system execution
- **Week 10:** Chapters 08 and 11 with SmolVLA and RTC.
- **Week 11:** Chapters 07, 08, and 11 with Helix and Fast-in-Slow.

### Phase 6 - RL fine-tuning, reasoning, and world models
- **Week 12:** Chapters 07 and 10 with HIL-SERL, SimpleVLA-RL, and π*0.6.
- **Week 13:** Chapters 07, 08, and 12 with CoT-VLA, ThinkAct, and Fast-ThinkAct.
- **Week 14:** Chapters 02, 07, and 12 with UniVLA, Cosmos Policy, and DreamZero.

## What to extract from each chapter

Each chapter should leave you with four outputs:
1. a concise definition of the design problem,
2. a list of the most important trade-offs,
3. named anchor papers that instantiate those trade-offs,
4. a short checklist you could use when designing or reviewing a real VLA system,
5. one recommended deepening resource from Chapter 13 (paper/tutorial/book).
