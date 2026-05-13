# Appendix - Glossary and Quick Revision Notes

## A.1 Core glossary (expanded)

- **VLA (Vision-Language-Action Model):** Multimodal policy that maps perception and language context to executable actions.
- **VLM (Vision-Language Model):** Multimodal model that usually outputs semantic/textual predictions rather than control actions.
- **Behavior Cloning (BC):** Supervised learning from demonstration actions.
- **DAgger-style aggregation:** Iterative dataset expansion with policy-induced states corrected by expert/supervisor labels.
- **Action Tokenization:** Converting continuous control space into discrete symbolic units for autoregressive modeling.
- **Action Chunking:** Predicting short action sequences at once for smoother and faster execution.
- **Embodiment Conditioning:** Providing robot-specific context so one model can adapt behavior across morphologies.
- **Distribution Shift:** Deviation between training and deployment state-action distributions.
- **Sim-to-Real Gap:** Mismatch between simulated and physical dynamics/sensing.
- **Fallback Policy:** Safe backup behavior triggered when uncertainty or risk exceeds threshold.
- **Intervention Rate:** Frequency of safety/human overrides during deployment.
- **Calibration:** Degree to which confidence estimates align with actual correctness/safety.

## A.2 Quick concept map

- Ch. 01 defines taxonomy and architecture categories.
- Ch. 02 formalizes objective and shift challenges.
- Ch. 03–04 define model and action-interface design.
- Ch. 05–06 define data and optimization levers.
- Ch. 07 defines post-training and planning integration.
- Ch. 08–11 define runtime, evaluation, safety, and deployment.
- Ch. 12 translates understanding into research direction.

## A.3 Exam-style deep checks

1. Why can better offline action loss fail to improve long-horizon success?
2. Under what conditions is tokenized action better than continuous regression?
3. How do you diagnose whether a failure is perception-grounding vs control-interface mismatch?
4. Why must safety constraints be layered rather than model-internal only?
5. What evaluation slices are mandatory before real-world rollout?

## A.4 1-page revision summary

- VLA unifies perception, language grounding, and control into one policy interface.
- Taxonomy-driven thinking prevents architecture choices from being ad hoc.
- Action interface design is as important as model backbone choice.
- Data quality/coverage and synchronization are dominant determinants of robustness.
- BC is a strong start but usually insufficient without post-training and safety wrapping.
- Runtime reliability depends on latency budgets, uncertainty gating, and fallback hierarchy.
- Proper evaluation needs robustness, error taxonomy, and intervention metrics.
- Deployment success requires engineering discipline: lineage, canarying, monitoring, and rollback.

## A.5 Practical pre-deployment checklist

- [ ] Action units/scales validated end-to-end.
- [ ] Safety limits and emergency stop tested.
- [ ] Robustness tests passed on perturbation suites.
- [ ] Intervention and incident logging operational.
- [ ] Rollback and operator takeover runbook verified.

