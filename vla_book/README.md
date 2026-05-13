# Vision-Language-Action (VLA) Book Notes

This directory contains a **book-style VLA handbook** that explains concepts in depth, with framing tailored to your background in:
- Deep RL,
- Computer Vision,
- Autonomous Systems.

## Reading philosophy

The notes are organized to move from **foundations → modeling choices → training → deployment**. Each chapter now explicitly covers:
- definitions and formal framing,
- why each concept matters in practice,
- common trade-offs and failure modes,
- links to adjacent chapters.

## How to read this book

1. Read Chapters 00–02 to align terminology and formal problem setup.
2. Read Chapters 03–07 to understand model, data, and training design choices.
3. Read Chapters 08–11 for runtime behavior, reliability, and system engineering.
4. Read Chapter 12 and the Appendix for research direction and revision.

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
- [Appendix - Glossary and Quick Revision Notes](./appendix_glossary_and_quick_revision.md)

## Suggested weekly flow (optional)

- Week 1: Ch. 00–02 (mental model + math framing)
- Week 2: Ch. 03–05 (architecture + actions + data)
- Week 3: Ch. 06–08 (optimization + RL post-training + runtime)
- Week 4: Ch. 09–12 + Appendix (evaluation + safety + research)

