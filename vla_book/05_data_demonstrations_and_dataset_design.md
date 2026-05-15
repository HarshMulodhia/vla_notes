# Chapter 05 - Data, Demonstrations, and Dataset Design

## 5.1 Why data dominates VLA performance

For VLA systems, data quality and coverage often matter more than incremental architecture tweaks.

Three reasons:
1. actions are tightly coupled to scene context,
2. language conditioning introduces semantic ambiguity,
3. deployment distributions are broader than lab demonstrations.

A small but diverse dataset can outperform a larger but narrow one when tasks require compositional generalization.

## 5.2 What a trajectory record must contain

A useful trajectory is not just (image, action). It should include:
- synchronized multi-camera observations,
- proprioception and robot state,
- control commands with units and timestamps,
- instruction/goal text,
- task boundary markers,
- success/failure outcome labels,
- environment and embodiment metadata.

If synchronization is weak, training learns spurious temporal alignments.

## 5.3 Demonstration collection modes and trade-offs

### Human teleoperation
- Pros: natural recovery strategies, rich semantics.
- Cons: inconsistent quality and operator bias.

### Scripted controllers
- Pros: clean trajectories and reproducibility.
- Cons: narrow behavior manifold, weak robustness signals.

### Policy-generated rollouts with filtering
- Pros: scalable hard-negative collection.
- Cons: risk of reinforcing model bias if filtering is weak.

### Simulation-first collection
- Pros: scale and controlled variation.
- Cons: sim-to-real mismatch in visual and contact dynamics.

Best practice is mixed-source datasets with source-aware balancing.

## 5.4 Language annotation quality

Instruction text should be:
- specific enough to be actionable,
- consistent in vocabulary,
- explicit about constraints and success criteria.

Weak annotation patterns:
- vague verbs (“do it,” “handle object”),
- missing disambiguators (“pick the block” in multi-block scenes),
- inconsistent task naming across episodes.

These issues directly degrade language grounding and instruction following.

## 5.5 Dataset balancing and curriculum

Common imbalance: many easy clean episodes, very few recovery/hard cases.

Balancing strategies:
- per-task and per-failure reweighting,
- long-tail sampling quotas,
- staged curriculum (easy → varied → adversarial).

A good curriculum should increase scene complexity and instruction complexity separately.

## 5.6 Data schema and storage design

For reproducibility, include:
- immutable dataset snapshots,
- clear schema versioning,
- deterministic preprocessing config,
- provenance metadata (collector, environment, controller version).

Without strict lineage, performance regressions become impossible to diagnose.

## 5.7 Data-centric debugging workflow

When policy fails, inspect data first:
1. Is the failure mode represented in training?
2. Are labels/commands temporally aligned?
3. Are instruction variants present for the same task?
4. Is proprioception quality degraded in the failing subset?

Many “model issues” are unresolved data coverage issues.

## 5.8 Dataset readiness checklist

Before large-scale training, confirm:
- synchronization sanity checks passed,
- unit conventions are consistent,
- language templates are quality-controlled,
- success/failure tags are reliable,
- hard-case slices are explicitly represented.

## 5.9 Why OXE, AgiBot World, and UMI changed the conversation

The importance of OXE and AgiBot World is not only scale. These datasets made embodiment metadata, synchronization quality, shared schemas, and cross-lab variation unavoidable design concerns. UMI and later human-video-based pipelines further expanded the data question from "how do we teleoperate more?" to "how do we convert diverse human and robot behavior into reusable robot policy supervision?"

## 5.10 Training formats versus logging formats

A useful distinction is to separate logging formats such as rosbag and MCAP from training-oriented schemas such as RLDS or LeRobotDataset. Logging formats are optimized for faithful capture and replay; training formats are optimized for batch loading, slicing, normalization, and metadata access. Strong VLA systems usually need both layers to be well designed.
