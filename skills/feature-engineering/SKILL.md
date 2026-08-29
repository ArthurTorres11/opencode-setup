---
name: feature-engineering
description: >
  Design, review, validate, and improve feature engineering for machine learning
  systems. Use when the task involves creating, transforming, selecting, or
  validating predictive input variables for tabular data, time series, NLP,
  computer vision, ranking, anomaly detection, or predictive models, especially
  when feature availability, leakage, preprocessing consistency, representation
  quality, or train-inference skew may affect model performance. Do not invoke
  for software-engineering "data models" such as database schemas, ORM models,
  entities, or application data structures.
---

# Feature Engineering

Use this skill when model quality depends materially on how raw data is
transformed into predictive representations.

Typical cases include:

* tabular machine learning;
* time series;
* classification;
* regression;
* ranking;
* anomaly detection;
* NLP features;
* computer vision features;
* preprocessing pipelines;
* feature selection;
* feature transformations;
* leakage investigation;
* train-inference consistency.

Do not invoke this skill merely because a model has input columns.

Use it when feature design, validity, availability, representation, or
transformation is part of the problem.

## Main Goal

Create features that are:

* useful;
* available at prediction time;
* reproducible;
* leakage-safe;
* robust;
* interpretable when needed;
* consistent between training and inference.

A feature is not useful if it improves offline metrics but cannot be reproduced
correctly in production.

## Start With Prediction Time

Before creating features, determine exactly when the prediction happens.

Define:

* prediction timestamp;
* information available at that moment;
* target timestamp;
* prediction horizon;
* data delays;
* update frequency;
* source availability.

Every feature should answer:

> Could this exact value have been known at prediction time?

If not, the feature is invalid unless the use case explicitly allows it.

## Feature Contract

For important features, define:

```text
name
source
transformation
timestamp
availability
data type
missing-value behavior
expected range
training behavior
inference behavior
```

This helps prevent silent differences between experimentation and production.

## Feature Categories

Possible feature types include:

### Raw Features

Direct source variables.

Use them when the original representation already contains useful information.

### Transformations

Examples:

* log;
* square root;
* clipping;
* scaling;
* normalization;
* discretization.

Apply transformations only when they solve a real modeling or numerical problem.

### Ratios

Useful when relative magnitude matters.

Check denominator stability and zero handling.

### Differences

Useful when relationships between variables are more informative than absolute
values.

### Interactions

Examples:

```text
A × B
A / B
A - B
```

Interactions can expose relationships a model cannot easily infer.

Do not create large combinatorial interaction spaces without evidence.

### Aggregations

Examples:

* mean;
* median;
* minimum;
* maximum;
* standard deviation;
* count;
* sum;
* quantiles.

Aggregation windows and grouping logic must reflect the prediction context.

### Frequency Features

Examples:

* event counts;
* category frequency;
* historical occurrence rates.

Ensure statistics are computed without leaking validation or test information.

### Categorical Features

Potential representations include:

* one-hot encoding;
* ordinal encoding;
* frequency encoding;
* target encoding;
* learned embeddings.

Choose based on:

* cardinality;
* model type;
* data volume;
* semantics.

## Numerical Features

Inspect:

* scale;
* skew;
* extreme values;
* missing values;
* domain boundaries.

Not all models require scaling.

For example, many tree-based models are largely insensitive to feature scale.

Do not normalize automatically.

## Categorical Encoding

### One-Hot Encoding

Useful for relatively low-cardinality categories.

### Ordinal Encoding

Use only when ordering exists or when the model can tolerate arbitrary category
codes appropriately.

### Target Encoding

Can be effective for high-cardinality variables but has significant leakage
risk.

Target-based statistics should normally be estimated using training-only or
out-of-fold procedures.

Do not compute target encoding over the complete dataset before splitting.

## Missing Values

Missingness can contain information.

Possible approaches include:

* model-native missing handling;
* statistical imputation;
* constant values;
* explicit missing category;
* missingness indicator.

Understand why values are missing before choosing a strategy.

Distinguish:

* random missingness;
* source failure;
* not applicable;
* delayed availability;
* absent event.

Do not silently replace every missing value with zero.

## Missingness Indicators

An indicator such as:

```text
feature_is_missing
```

may be useful when absence itself carries information.

Use only when supported by the data and model behavior.

## Outliers

Determine whether extreme values represent:

* valid rare cases;
* measurement errors;
* data corruption;
* distribution tails.

Potential strategies include:

* clipping;
* robust scaling;
* transformations;
* explicit anomaly flags;
* preserving the value.

Do not remove outliers automatically.

Removing rare but valid examples may make the model worse precisely where it
needs robustness.

## Time-Based Features

Possible features include:

* hour;
* day of week;
* month;
* quarter;
* day of year;
* time since event;
* age;
* recency;
* cyclical encodings.

Use calendar features only when time genuinely influences the target.

For cyclical variables, representations such as sine and cosine may preserve
periodicity.

## Lag Features

Lag features use previous observations.

For every lag define:

* source timestamp;
* lag duration;
* update frequency;
* missing behavior;
* availability at inference.

Example:

```text
x(t - 1)
x(t - 7)
x(t - 30)
```

Do not assume a row offset equals a time offset when sampling is irregular.

## Rolling Features

Possible rolling statistics include:

* mean;
* median;
* minimum;
* maximum;
* standard deviation;
* quantiles;
* count;
* slope.

The window must contain only information available at prediction time.

Avoid centered windows for forward-looking prediction unless the use case
explicitly allows future data.

## Exponentially Weighted Features

EWM statistics can emphasize recent observations while preserving longer
history.

They may be useful when recency matters more than equal weighting.

Ensure initialization and state handling are reproducible in inference.

## Rates of Change

Examples:

```text
x(t) - x(t-1)
(x(t) - x(t-k)) / k
percentage change
slope
```

These may capture trend or acceleration better than raw values.

Handle unstable denominators carefully.

## Cumulative Features

Examples:

* cumulative count;
* cumulative sum;
* time since start;
* historical frequency.

Cumulative statistics must not incorporate future observations.

## Event Features

Examples:

* time since previous event;
* number of events in recent window;
* event sequence position;
* event type;
* event duration.

These can be useful when behavior depends on event history.

## Group-Based Features

Statistics may be constructed by entity or group.

Examples:

* user history;
* account history;
* device history;
* product history.

Be careful with leakage across train/test splits.

If the same entity appears in multiple splits, determine whether that matches
production usage.

## Historical Target Features

Features derived from historical target values can be useful in temporal tasks.

Examples:

```text
target(t-1)
rolling_target_mean
```

They are also high-risk for leakage.

Verify:

* the target is known at prediction time;
* the lag is real;
* no future target value enters the calculation.

## Feature Availability

A feature may exist historically but still be unavailable in production.

Check:

* source latency;
* refresh interval;
* API availability;
* batch timing;
* delayed labels;
* manual inputs;
* upstream dependencies.

Offline datasets often make unavailable information appear available.

## Leakage

Leakage occurs when information unavailable at prediction time influences the
model.

Common sources include:

### Target Leakage

A feature directly or indirectly contains the target.

### Temporal Leakage

Future information enters past predictions.

### Preprocessing Leakage

Statistics are learned using validation or test data.

Examples:

* scaler fit on full dataset;
* imputer fit on full dataset;
* target encoder fit globally.

### Entity Leakage

Related observations appear across splits in a way that makes evaluation
unrealistically easy.

### Post-Event Leakage

Information produced only after the outcome is included as a feature.

### Manual Correction Leakage

Features contain corrections that would not exist in real-time use.

Investigate suspiciously predictive features aggressively.

## Split Before Learning Feature Parameters

Operations that learn from data should usually be fitted only using training
data.

Examples:

* scaling;
* imputation;
* PCA;
* target encoding;
* vocabulary construction;
* learned embeddings;
* feature selection.

Use pipelines where practical to enforce this boundary.

## Train-Inference Skew

Training and inference must compute equivalent features.

Potential differences include:

* different code;
* different timezone;
* different missing-value logic;
* batch vs streaming behavior;
* inconsistent category handling;
* unavailable features;
* different aggregation windows.

Avoid implementing feature logic independently in multiple systems when a shared
implementation is practical.

Use `ml-engineer` when production feature consistency is the central problem.

## Feature Selection

Feature selection should improve:

* generalization;
* robustness;
* simplicity;
* inference cost;
* interpretability.

Possible approaches include:

* domain reasoning;
* univariate analysis;
* regularization;
* permutation importance;
* recursive selection;
* model-based importance;
* stability analysis.

Do not remove a feature solely because pairwise correlation is low.

Predictive relationships may be nonlinear or interaction-dependent.

## Correlated Features

Highly correlated variables may affect:

* linear model stability;
* interpretability;
* feature importance;
* numerical conditioning.

They are not automatically harmful for every model.

Evaluate based on model type and objective.

## Feature Importance

Importance can help understand model dependence.

Possible methods include:

* built-in importance;
* permutation importance;
* SHAP;
* coefficients.

Do not use importance as proof that a feature is valid.

A leaked feature often appears extremely important.

Validate feature provenance first.

## Representation Learning

Not all features must be manually engineered.

Modern models may learn useful representations directly.

Examples:

* text embeddings;
* image embeddings;
* learned categorical embeddings;
* neural encoders.

Feature engineering can include deciding which representation to use, not only
creating handcrafted columns.

## NLP Features

For NLP tasks, feature representations may include:

* bag-of-words;
* TF-IDF;
* token statistics;
* linguistic features;
* pretrained embeddings;
* sentence embeddings;
* model-generated representations.

Choose based on:

* task;
* dataset size;
* language;
* latency;
* model architecture.

Do not automatically use embeddings when a simpler lexical representation
solves the task.

Use `nlp-engineer` when NLP modeling itself is the primary problem.

## Text Leakage

Common NLP leakage sources include:

* labels embedded in text;
* post-outcome notes;
* template artifacts;
* duplicate documents;
* near-duplicate samples across splits;
* metadata revealing the target.

Check duplicates before trusting unusually strong NLP results.

## Computer Vision Features

Depending on the task, features may include:

* image metadata;
* color statistics;
* texture;
* geometry;
* pretrained image embeddings;
* learned representations;
* detected object attributes.

Manual image features may still be useful for small or constrained problems.

Use `computer-vision-engineer` when image modeling itself becomes central.

## Image Leakage

Potential leakage includes:

* duplicated images across splits;
* near-duplicates;
* filenames encoding labels;
* watermarks;
* capture-device artifacts;
* background patterns associated with classes.

Inspect whether the model learns shortcuts instead of intended concepts.

## Embedding Features

Embeddings can be used as downstream ML features.

Track:

* embedding model;
* version;
* dimensionality;
* normalization;
* source preprocessing.

Changing the embedding model changes the feature representation.

Treat it as a feature-version change.

## Dimensionality Reduction

Potential methods include:

* PCA;
* truncated SVD;
* learned projections.

Use dimensionality reduction when it improves:

* computational efficiency;
* noise reduction;
* visualization;
* model performance.

Fit transformations using training data only.

Do not reduce dimensions merely because the feature matrix is large.

## Data Quality Features

Quality indicators can sometimes help models distinguish reliable from unreliable
inputs.

Examples:

* missingness count;
* source status;
* confidence score;
* measurement age;
* completeness score.

Prefer fixing upstream data problems when possible.

Do not use quality flags to hide broken pipelines.

## Feature Stability

Evaluate whether features remain stable over time.

Inspect:

* distribution changes;
* missingness changes;
* category changes;
* range violations;
* importance instability.

A feature that was predictive historically may become unreliable.

Use `model-evaluator` or `ml-engineer` for broader production stability analysis.

## Feature Cost

Some features are expensive.

Consider:

* computation;
* storage;
* API cost;
* inference latency;
* data acquisition;
* pipeline complexity.

A small performance improvement may not justify a very expensive feature.

Evaluate marginal value.

## Ablation

Ablation can measure whether feature groups materially improve the model.

Example:

```text
baseline features
        ↓
+ feature group A
        ↓
+ feature group B
```

Compare using the same evaluation protocol.

Do not infer feature value solely from importance rankings.

## Feature Experiments

When adding new features:

1. define the hypothesis;
2. preserve the baseline;
3. add a coherent feature group;
4. evaluate under the same split;
5. inspect aggregate and segmented metrics;
6. inspect regressions.

Use `experiment-designer` for more rigorous experimental design.

## Validation

Feature validation should occur at multiple levels.

### Schema Validation

Check:

* types;
* required fields;
* ranges;
* categories.

### Transformation Validation

Verify formulas and edge cases.

### Temporal Validation

Ensure no future information enters features.

### Pipeline Validation

Confirm training and inference produce equivalent values.

### Model Validation

Measure whether the features actually improve model behavior.

Do not stop at successful feature computation.

## Debugging Feature Problems

When performance unexpectedly changes, inspect:

```text
source data
   ↓
feature transformation
   ↓
feature values
   ↓
model input
   ↓
prediction
```

Compare training and inference examples where possible.

Potential causes include:

* source schema changes;
* timezone issues;
* missing categories;
* aggregation differences;
* stale features;
* preprocessing version mismatch.

Use `debugging` for systematic investigation.

## Feature Store

A feature store may help when multiple systems require:

* reusable feature definitions;
* offline/online consistency;
* feature versioning;
* low-latency serving.

Do not introduce a feature store automatically.

For smaller systems, shared feature code or materialized tables may be simpler
and sufficient.

## Reproducibility

Important feature transformations should be reproducible.

Track when relevant:

* code version;
* parameters;
* source version;
* feature schema;
* preprocessing artifacts.

Avoid notebook-only feature logic for production-critical models.

## When Other Skills Should Be Combined

Examples:

* `feature-engineering` + `eda-specialist` for discovering candidate
  representations;
* `feature-engineering` + `model-evaluator` for measuring feature impact;
* `feature-engineering` + `experiment-designer` for ablation experiments;
* `feature-engineering` + `ml-engineer` for production feature pipelines;
* `feature-engineering` + `debugging` for train-inference inconsistencies;
* `feature-engineering` + `statistician` for deeper statistical relationships;
* `feature-engineering` + `nlp-engineer` for text representations;
* `feature-engineering` + `computer-vision-engineer` for image representations.

Do not invoke additional skills unless they materially improve the task.

## Required Output

For substantial feature-engineering work, report:

### Prediction Context

Prediction time, target, horizon, and information availability.

### Current Features

Relevant existing representations and transformations.

### Risks

Leakage, availability, instability, skew, or data-quality concerns.

### Proposed Features

Features or feature groups with rationale.

### Expected Value

What signal each proposal is intended to capture.

### Implementation

How features should be computed reproducibly.

### Validation

How correctness, leakage safety, and model impact should be tested.

For simple tasks, keep output proportional to the request.

## Rules

* Define prediction time before creating features.
* Never use information unavailable at inference time.
* Fit learned preprocessing only on training data.
* Treat historical target features as high-risk.
* Avoid centered rolling windows for forward prediction.
* Do not assume row offsets equal time offsets.
* Separate feature validity from feature importance.
* Do not interpret feature importance as causality.
* Preserve training-inference consistency.
* Investigate suspiciously powerful features for leakage.
* Do not create large feature sets without hypotheses.
* Evaluate feature groups against a baseline.
* Consider feature cost and operational complexity.
* Prefer reproducible feature pipelines over notebook-only transformations.
