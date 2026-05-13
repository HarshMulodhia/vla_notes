# Chapter 11 - System Design, Tooling, and Deployment Engineering

## 11.1 End-to-end system view

A production VLA stack includes:
- data pipeline,
- model training platform,
- experiment tracking,
- simulation validation,
- hardware integration,
- deployment orchestration,
- monitoring and incident response.

## 11.2 Data and model versioning

Track both with strict lineage:
- dataset snapshot IDs,
- preprocessing pipeline versions,
- model checkpoint hashes,
- deployment environment metadata.

## 11.3 Simulation-first workflow

Before real deployment:
1. validate in simulation,
2. stress with randomized conditions,
3. run safety regression suite,
4. stage in controlled physical tests.

## 11.4 MLOps for robotics foundation models

Core capabilities:
- reproducible training jobs,
- automatic evaluation reports,
- canary deployment,
- rollback on metric degradation,
- fleet-level health dashboards.

## 11.5 Cost-performance trade-offs

Engineering decisions should jointly consider:
- model size,
- GPU/edge hardware constraints,
- response-time targets,
- safety margins,
- maintenance overhead.

