# Chapter 10 - Safety, Reliability, and Governance for Physical Deployment

## 10.1 Layered safety architecture

Physical deployment requires defense-in-depth:
1. model-level constraints,
2. command-level limit enforcement,
3. controller-level protective checks,
4. hardware interlocks and emergency stop,
5. human supervisory override.

No single layer should be trusted as the sole barrier.

## 10.2 Constraint design

Typical constraints include:
- workspace boundaries,
- joint and velocity limits,
- collision and self-collision avoidance,
- force/torque thresholds,
- forbidden-zone constraints.

Constraints should be explicit, testable, and verified in regression suites.

## 10.3 Reliability operations

Reliability requires ongoing monitoring for:
- distribution shift,
- anomaly bursts,
- policy degradation after updates,
- intervention frequency trends.

Operational reliability is a lifecycle process, not a one-time validation artifact.

## 10.4 Human-in-the-loop control

Human roles include:
- intervention during unsafe behavior,
- corrective demonstration generation,
- failure triage and labeling,
- policy release approval.

Human-in-the-loop pipelines should optimize both safety and learning velocity.

## 10.5 Governance and auditability

Keep immutable logs of:
- sensor snapshots,
- instructions/prompts,
- model and config versions,
- action outputs,
- safety overrides,
- outcomes.

Without audit-quality logs, post-incident analysis and compliance become unreliable.

## 10.6 Security for VLA deployments

Threat surfaces include:
- instruction channel tampering,
- sensor spoofing,
- insecure model artifact updates,
- unauthorized remote control.

Mitigations include authenticated command channels, signed artifacts, strict network isolation, and access controls.

## 10.7 Release gating checklist

Before rollout:
1. pass simulation safety regression,
2. pass staged physical trial thresholds,
3. confirm rollback plan,
4. validate monitoring and alerting,
5. sign off by operations and safety owners.

