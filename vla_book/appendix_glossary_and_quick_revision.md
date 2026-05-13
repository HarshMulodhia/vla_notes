# Appendix - Glossary and Quick Revision Notes

## A.1 Core terms

- **VLA**: model mapping vision + language (+ state) to actions.
- **VLM**: model mapping vision + language to text/semantic outputs.
- **Behavior Cloning (BC)**: supervised imitation of demonstration actions.
- **Action Tokenization**: discretizing/encoding continuous actions into sequence tokens.
- **Embodiment**: specific robot morphology, kinematics, and control interface.
- **Sim-to-Real**: transfer from simulation-trained behavior to physical deployment.
- **Distribution Shift**: mismatch between training data distribution and deployment distribution.

## A.2 Exam-style quick checks

1. Why can a low imitation loss still yield poor task success?
2. When should you prefer tokenized actions over continuous outputs?
3. Why is fallback control mandatory in physical VLA systems?
4. What is the minimum evaluation protocol before real deployment?

## A.3 1-page revision summary

- VLA unifies perception, language grounding, and control.
- Data quality/diversity is often the strongest determinant of generalization.
- Action interface design is central to both performance and safety.
- Pure BC is rarely enough for robust real-world deployment.
- Runtime safety layers, monitoring, and fallback logic are non-negotiable.
- Benchmarking must include robustness and failure taxonomy, not only success rate.

