# Chapter 10 - Safety, Reliability, and Governance for Physical Deployment

## 10.1 Safety stack layers

No single model is enough for physical safety. Use layered protection:
- model-level constraints,
- command-level filters,
- controller-level safeguards,
- hardware interlocks,
- operator supervision.

## 10.2 Reliability engineering for VLAs

Key practices:
- distribution shift monitoring,
- confidence-aware gating,
- anomaly detection on observations/actions,
- rollback-capable deployment strategy.

## 10.3 Human-in-the-loop operations

Human oversight should include:
- intervention tooling,
- corrective demonstration capture,
- post-incident replay and diagnosis.

## 10.4 Governance and auditability

Keep complete logs of:
- observation snapshots,
- prompts/instructions,
- model versions,
- output actions,
- safety interventions,
- final outcomes.

This is essential for debugging, compliance, and accountability.

## 10.5 Security concerns

Protect against:
- prompt/instruction tampering,
- sensor spoofing,
- unsafe model updates,
- insecure remote control paths.

