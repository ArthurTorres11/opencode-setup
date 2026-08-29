---
name: ml-engineer
description: >
  Design, implement, review, debug, and improve machine learning systems across
  the full lifecycle from data and training to evaluation, inference, deployment,
  monitoring, and reproducibility. Use when the task specifically involves ML
  pipelines, training systems, inference serving, model registries, experiment
  tracking, model versioning, feature consistency, deployment, monitoring,
  retraining, drift, train-inference skew, or productionizing machine learning
  models. For general software engineering, code review, APIs, or Python tasks
  that are not specifically about the ML lifecycle, use python-engineer instead.
---

# ML Engineer

Use this skill when engineering concerns materially affect a machine learning
system.

Typical cases include:

* designing ML pipelines;
* implementing training pipelines;
* building inference systems;
* productionizing models;
* reviewing ML repositories;
* debugging training or inference failures;
* improving reproducibility;
* designing model-serving architectures;
* managing model artifacts and versions;
* experiment tracking;
* monitoring production models;
* investigating training/inference skew;
* evaluating deployment readiness;
* designing retraining workflows.

This skill focuses on the engineering lifecycle around ML models.

Use `model-evaluator` when the primary question is whether a model is actually
good.

Use `experiment-designer` when the primary question is how to design a valid
experiment.

Use `statistician` when statistical validity is the primary concern.

## Main Goal

Build ML systems that are:

* correct;
* reproducible;
* testable;
* deployable;
* observable;
* maintainable;
* reliable in production.

A model performing well in a notebook is not sufficient evidence that the ML
system is production-ready.

## System First

Before changing an ML system, understand its lifecycle.

Map:

1. data source;
2. ingestion;
3. preprocessing;
4. feature generation;
5. dataset construction;
6. target construction;
7. train/validation/test split;
8. training;
9. evaluation;
10. artifact creation;
11. model registry or storage;
12. deployment;
13. inference;
14. monitoring;
15. feedback or retraining.

Not every project requires every component.

Do not introduce infrastructure that the project does not need.

## Repository Inspection

For substantial work, inspect:

* project structure;
* Python environment;
* dependencies;
* data access;
* training entry points;
* inference entry points;
* configuration;
* model artifacts;
* tests;
* CI/CD;
* experiment tracking;
* deployment configuration;
* monitoring.

Identify what is actually implemented before recommending a new architecture.

Prefer existing conventions when they are sound.

## Data Contracts

Understand what data the model expects.

Check:

* schema;
* types;
* required fields;
* units;
* categorical values;
* missing-value behavior;
* preprocessing assumptions;
* feature ordering;
* metadata;
* versioning.

Training and inference should agree on the meaning of each feature.

Treat silent schema changes as a production risk.

Validate important assumptions at appropriate boundaries.

## Dataset Construction

Verify how datasets are created.

Check:

* source consistency;
* filtering;
* deduplication;
* sampling;
* target creation;
* split strategy;
* preprocessing;
* leakage;
* reproducibility.

For temporal data, respect chronological availability.

For grouped data, consider whether observations from the same entity can leak
across splits.

For NLP and computer vision datasets, also consider duplicate or near-duplicate
examples across splits.

Do not assume random splitting is appropriate.

## Training Pipeline

A training pipeline should make important choices explicit.

Track when relevant:

* dataset version;
* code version;
* feature configuration;
* preprocessing;
* model configuration;
* hyperparameters;
* random seeds;
* environment;
* metrics;
* artifacts.

Training should be reproducible to the degree required by the application.

Avoid hidden notebook state as a dependency of production training.

## Reproducibility

Determine what is necessary to reproduce a model.

Potential requirements include:

* source code revision;
* dependency versions;
* dataset version;
* configuration;
* random seeds;
* preprocessing artifacts;
* model artifacts;
* training metadata.

Do not promise perfect determinism when the underlying hardware or algorithms do
not provide it.

Distinguish reproducibility from exact bit-for-bit determinism.

## Experiment Tracking

Use the project's existing experiment-tracking system when available.

Examples may include:

* MLflow;
* Weights & Biases;
* cloud-native experiment tracking;
* custom metadata stores.

Track information that supports comparison and reproduction.

Do not log large amounts of metadata without a clear use.

At minimum, important experiments should make it possible to understand:

* what changed;
* what data was used;
* what configuration was used;
* what result was obtained.

## Model Artifacts

Treat model artifacts as versioned outputs.

Consider:

* serialization format;
* preprocessing dependencies;
* model version;
* schema compatibility;
* framework version;
* metadata;
* storage location;
* integrity.

Avoid relying on local paths or manually copied artifacts for production
workflows.

Do not load untrusted serialized model artifacts.

## Training and Inference Consistency

One of the highest-risk ML engineering failures is inconsistency between training
and inference.

Compare:

* feature definitions;
* preprocessing;
* normalization;
* encoders;
* tokenization;
* image transformations;
* missing-value handling;
* categorical mappings;
* feature order;
* model version;
* configuration.

Prefer shared implementations when that reduces the risk of divergence.

Do not duplicate preprocessing logic unnecessarily.

## Inference Systems

Understand the inference mode.

Common patterns include:

* synchronous API;
* asynchronous API;
* batch inference;
* streaming inference;
* scheduled inference;
* embedded inference.

Evaluate:

* latency requirements;
* throughput;
* memory;
* hardware;
* batching;
* concurrency;
* timeouts;
* failure behavior;
* scaling;
* model loading.

Choose architecture based on actual requirements rather than assuming every model
needs an online API.

## Model Serving

For online serving, consider:

* startup time;
* model loading;
* warm-up;
* request validation;
* batching;
* concurrency;
* CPU/GPU utilization;
* memory usage;
* timeouts;
* health checks;
* graceful shutdown.

Avoid loading the model for every request.

Keep serving logic separate from training logic when practical.

## Deployment

Before deployment, establish:

* artifact being deployed;
* model version;
* configuration;
* environment;
* dependencies;
* resource requirements;
* rollback strategy;
* health checks;
* validation criteria.

Prefer repeatable deployments over manual procedures.

Do not assume the newest model should automatically replace the current model.

Promotion should depend on explicit acceptance criteria when model quality matters.

## Model Registry and Versioning

When a registry is justified, maintain enough metadata to answer:

* Which model is in production?
* Which code produced it?
* Which dataset produced it?
* Which configuration produced it?
* Which evaluation approved it?
* Can the previous version be restored?

Do not introduce a model registry solely because it is considered standard MLOps
practice.

Use infrastructure proportional to the project.

## Validation Before Production

Before promoting a model, validate more than serialization or API availability.

Depending on the system, check:

* model-quality gates;
* baseline comparison;
* schema compatibility;
* inference correctness;
* latency;
* resource usage;
* failure behavior;
* integration behavior;
* regression tests.

Use `model-evaluator` for deeper model-quality assessment.

Production readiness includes both model quality and system reliability.

## Monitoring

Separate system monitoring from model monitoring.

### System Monitoring

Examples:

* latency;
* throughput;
* errors;
* timeouts;
* resource usage;
* availability.

### Data Monitoring

Examples:

* schema changes;
* missingness;
* feature distributions;
* unexpected categories;
* data freshness.

### Model Monitoring

Examples:

* prediction distributions;
* confidence or score distributions;
* calibration;
* quality metrics when labels become available;
* drift indicators.

Do not treat drift detection as proof that model performance degraded.

Drift is evidence that may require investigation.

## Drift

Distinguish:

### Data Drift

Input distribution changed.

### Concept Drift

Relationship between inputs and target changed.

### Prediction Drift

Prediction distribution changed.

### Performance Drift

Observed model quality degraded.

These are related but not equivalent.

Do not automatically retrain merely because a drift metric crossed a threshold.

Investigate whether the change is meaningful.

Use `debugging`, `model-evaluator`, or `statistician` when deeper investigation is
required.

## Retraining

Retraining may be:

* manual;
* scheduled;
* triggered;
* continuous.

Before automating retraining, define:

* trigger;
* data requirements;
* evaluation gates;
* approval process;
* deployment policy;
* rollback strategy.

A retraining pipeline should not automatically promote a worse model.

Separate:

```text
retrain
   ↓
evaluate
   ↓
approve
   ↓
deploy
```

Do not collapse these steps without justification.

## NLP Systems

For NLP pipelines, also verify consistency in:

* tokenizer;
* vocabulary;
* text normalization;
* truncation;
* sequence length;
* special tokens;
* model revision;
* label mappings.

For embedding systems, track:

* embedding model;
* model revision;
* dimensionality;
* normalization;
* indexing compatibility.

Use `nlp-engineer` when the task requires deeper NLP methodology.

## Computer Vision Systems

For vision pipelines, verify consistency in:

* resizing;
* cropping;
* normalization;
* channel ordering;
* augmentation;
* color space;
* label mappings;
* model input dimensions.

Training-only augmentations must not accidentally appear during production
inference.

Use `computer-vision-engineer` when deeper vision methodology is required.

## Foundation Models and Embeddings

When ML systems depend on externally hosted models, track when relevant:

* provider;
* model identifier;
* model version;
* embedding dimensionality;
* inference configuration;
* rate limits;
* latency;
* cost;
* fallback behavior.

Do not assume provider behavior is permanently stable.

Version important model dependencies when possible.

## Testing ML Systems

ML systems need both deterministic software tests and probabilistic model
evaluation.

### Deterministic Tests

Examples:

* preprocessing;
* schema validation;
* feature transformations;
* artifact loading;
* API behavior;
* serialization;
* pipeline execution.

### Model Evaluation

Examples:

* predictive quality;
* ranking quality;
* calibration;
* robustness;
* subgroup behavior;
* error analysis.

Do not replace deterministic software tests with model metrics.

Do not replace model evaluation with unit tests.

Both layers solve different problems.

## CI/CD

Use CI/CD to automate deterministic validation when useful.

Potential checks include:

* tests;
* linting;
* typing;
* pipeline validation;
* artifact validation;
* container build;
* integration tests.

Large training jobs usually should not be blindly executed on every code change.

Choose validation proportional to cost and risk.

## Infrastructure

Use infrastructure appropriate to the project's scale.

Potential technologies include:

* containers;
* orchestration platforms;
* managed ML services;
* artifact stores;
* feature stores;
* model registries;
* experiment trackers.

Do not introduce Kubernetes, feature stores, distributed training, or complex
orchestration merely because they are common in large ML platforms.

Complexity must solve a real requirement.

## Cost and Performance

ML systems may be constrained by:

* CPU;
* GPU;
* memory;
* storage;
* network;
* inference latency;
* training duration;
* cloud cost;
* external API cost.

Measure the actual bottleneck before optimizing.

Consider:

* batching;
* caching;
* quantization;
* smaller models;
* hardware acceleration;
* asynchronous processing;
* autoscaling.

Do not sacrifice required model quality solely for infrastructure optimization
without evaluating the tradeoff.

## Security

Treat model pipelines as production software.

Consider:

* secrets;
* access control;
* data permissions;
* artifact integrity;
* untrusted model files;
* sensitive training data;
* dependency security;
* endpoint exposure.

For user-facing ML systems, validate untrusted input appropriately.

## Failure Modes

Common ML engineering failures include:

* training/inference skew;
* stale models;
* stale features;
* wrong model version;
* schema changes;
* inconsistent preprocessing;
* incorrect artifact loading;
* hidden notebook state;
* unreproducible training;
* missing rollback;
* silent data changes;
* broken retraining pipelines;
* incorrect deployment configuration.

Investigate failures systematically rather than assuming the model itself is the
problem.

## When Other Skills Should Be Combined

Use specialized skills when the task crosses boundaries.

Examples:

* `ml-engineer` + `model-evaluator` for production-readiness assessment;
* `ml-engineer` + `experiment-designer` for training experiments;
* `ml-engineer` + `statistician` for statistically sensitive validation;
* `ml-engineer` + `debugging` for pipeline or production failures;
* `ml-engineer` + `python-engineer` for Python implementation;
* `ml-engineer` + `nlp-engineer` for NLP production systems;
* `ml-engineer` + `computer-vision-engineer` for vision production systems.

Do not invoke additional skills when this skill alone is sufficient.

## Required Output

For substantial ML engineering work, report:

### System

Relevant ML lifecycle and architecture.

### Findings

Important engineering issues or constraints.

### Changes

What was implemented or should change.

### Validation

How correctness and production readiness were evaluated.

### Risks

Remaining reliability, reproducibility, deployment, or monitoring concerns.

For simple tasks, keep the output proportional to the request.

## Rules

* A good notebook result is not evidence of production readiness.
* Keep training and inference behavior consistent.
* Track enough information to reproduce important models.
* Separate deterministic testing from model evaluation.
* Do not automate model promotion without appropriate quality gates.
* Do not treat drift as automatic proof of model degradation.
* Avoid unnecessary MLOps complexity.
* Prefer repeatable processes over manual procedures.
* Preserve rollback paths for important production changes.
* Validate significant changes before claiming success.
* Report incomplete validation explicitly.
