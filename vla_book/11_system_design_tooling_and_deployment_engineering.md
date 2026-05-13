# Chapter 11 - System Design, Tooling, and Deployment Engineering

## 11.1 Full-stack architecture view

A production-grade VLA program spans:
- data ingestion and curation,
- training and experiment management,
- evaluation orchestration,
- deployment packaging,
- runtime observability,
- incident and rollback workflows.

Model quality without systems quality does not translate to real-world reliability.

## 11.2 Data and model lineage

Track lineage at every stage:
- raw data source IDs,
- preprocessing pipeline versions,
- training config hashes,
- checkpoint signatures,
- deployment environment snapshots.

Lineage enables exact rollback and root-cause attribution.

## 11.3 Simulation-first development loop

Recommended loop:
1. implement feature in simulator,
2. run targeted and adversarial tests,
3. run safety and regression suite,
4. stage controlled hardware trials,
5. monitor and collect failures,
6. retrain and repeat.

Skipping stages may accelerate iteration short-term but increases operational risk dramatically.

## 11.4 Tooling capabilities that matter most

- reproducible job orchestration,
- unified experiment tracking,
- dataset version management,
- automated evaluation dashboards,
- policy artifact registry,
- canary rollout and rollback tooling.

These capabilities determine team velocity and reliability more than isolated model optimizations.

## 11.5 Deployment patterns

### Centralized inference
- easy updates, higher network dependency.

### Edge inference
- low latency, tighter hardware constraints.

### Hybrid deployment
- high-level policy in cloud, low-level safety/controller on edge.

Pattern choice should be driven by latency, safety, and connectivity requirements.

## 11.6 Cost-performance engineering

Trade-offs include:
- model size vs inference speed,
- control frequency vs compute budget,
- retraining cadence vs operational stability,
- logging depth vs storage cost.

Use explicit service-level objectives to make these trade-offs measurable.

## 11.7 Team process and release discipline

Healthy process includes:
- clear owner for each subsystem,
- standardized evaluation gates,
- incident review loops,
- documented rollback drills.

Strong process is a technical multiplier for VLA deployment success.

