---
name: model-evaluator
description: >
  Evaluate machine learning models rigorously using statistical, predictive,
  operational, and business-oriented analysis. Use when the task involves
  comparing models, validating predictive performance, analyzing errors,
  calibration, robustness, segment performance, stability, baselines,
  leakage, overfitting, drift, or deciding whether a traditional ML model
  is useful and production-ready.
---

# Model Evaluator

Use this skill when evaluating traditional machine learning or statistical
predictive models.

Typical cases include:

* regression;
* classification;
* ranking;
* forecasting;
* anomaly detection;
* tabular models;
* tree-based models;
* linear models;
* support vector machines;
* neural networks used for predictive ML;
* formula-based predictors;
* model comparison;
* production model validation.

Do not use this skill as the primary evaluator for LLM, RAG, prompt, or agent
quality.

Use `llm-evaluation` for probabilistic generative AI systems.

## Main Goal

Determine whether a model is genuinely useful, reliable, and appropriate for the
decision it supports.

Do not evaluate models only by aggregate metrics.

Understand:

* what the model is expected to do;
* whether it beats meaningful alternatives;
* where it performs well;
* where it fails;
* how severe failures are;
* whether performance is stable;
* whether evaluation is trustworthy;
* whether the model is appropriate for production use.

## Define the Decision

Before selecting metrics, determine what decision depends on the model.

Identify:

* prediction target;
* task type;
* downstream decision;
* acceptable error;
* cost of false positives;
* cost of false negatives;
* cost of large regression errors;
* latency requirements;
* operational constraints.

The evaluation should reflect the actual use case.

Do not optimize a metric that is disconnected from the decision being made.

## Validate the Evaluation Setup

Before trusting model results, verify that the evaluation itself is valid.

Check:

* train/validation/test separation;
* temporal ordering when applicable;
* group leakage;
* duplicate samples;
* target leakage;
* preprocessing leakage;
* feature availability;
* evaluation contamination;
* representative test distribution.

A sophisticated metric cannot rescue an invalid evaluation setup.

## Data Leakage

Inspect whether information unavailable at prediction time enters the model.

Potential sources include:

* future information;
* target-derived features;
* post-event variables;
* global preprocessing before splitting;
* duplicated observations across splits;
* entity leakage;
* target statistics;
* improperly constructed rolling features.

A suspiciously strong model should trigger a leakage investigation.

Use `debugging` when deeper root-cause analysis is required.

## Baseline Comparison

Always identify meaningful baselines.

Possible baselines include:

* majority class;
* random classifier;
* constant prediction;
* mean or median target;
* previous value;
* naive forecast;
* simple linear model;
* existing production model;
* current business rule;
* human process.

The baseline should represent the actual alternative to deploying the model.

Ask:

* Does the model beat the baseline?
* By how much?
* Is the improvement stable?
* Is the improvement practically meaningful?

Never interpret model metrics in isolation when a relevant baseline exists.

## Metric Selection

Choose metrics based on the task and failure costs.

Do not automatically compute every available metric.

## Regression Metrics

Possible metrics include:

### MAE

Measures average absolute error.

Useful when errors should contribute approximately linearly.

### RMSE

Penalizes larger errors more strongly.

Useful when large errors are especially harmful.

### Median Absolute Error

Provides a robust view when extreme errors exist.

### R²

Measures explanatory performance relative to a mean baseline.

Do not treat high R² as sufficient evidence of usefulness.

### Bias

Measures systematic overprediction or underprediction.

A model with acceptable MAE may still have harmful directional bias.

### Quantile Loss

Useful for quantile or probabilistic predictions.

### MAPE

Use cautiously.

MAPE can behave poorly when actual values are near zero.

Do not select it automatically for regression tasks.

## Classification Metrics

Possible metrics include:

### Accuracy

Useful when classes are reasonably balanced and error costs are similar.

### Precision

Useful when false positives are costly.

### Recall

Useful when false negatives are costly.

### F1

Balances precision and recall.

Do not use F1 automatically if false-positive and false-negative costs differ
substantially.

### ROC-AUC

Measures ranking ability across thresholds.

A good ROC-AUC does not determine the correct production threshold.

### PR-AUC

Often more informative for imbalanced positive classes.

### Log Loss

Evaluates probabilistic predictions.

### Balanced Accuracy

Useful when class frequencies are uneven.

## Ranking Metrics

For ranking or recommendation tasks, consider:

* Precision@K;
* Recall@K;
* MAP;
* MRR;
* NDCG;
* Hit Rate.

Choose metrics based on how ranked results are consumed.

## Threshold Analysis

For probabilistic classification, evaluation should not stop at model scores.

Evaluate threshold-dependent behavior.

Inspect:

* precision;
* recall;
* false-positive rate;
* false-negative rate;
* expected cost;
* operational volume.

The default threshold of `0.5` has no special meaning unless justified.

Choose thresholds based on actual requirements.

## Calibration

When predicted probabilities are used as probabilities, evaluate calibration.

Inspect:

* calibration curves;
* Brier score;
* reliability diagrams;
* expected vs observed frequencies.

A model can rank examples well while producing poorly calibrated probabilities.

Distinguish discrimination from calibration.

## Error Analysis

Metrics indicate how much error exists.

Error analysis explains where it comes from.

Inspect representative failures.

For regression, analyze:

* large positive errors;
* large negative errors;
* extreme residuals;
* systematic bias;
* error distribution.

For classification, analyze:

* false positives;
* false negatives;
* high-confidence errors;
* ambiguous cases.

Do not inspect only correctly predicted examples.

## Residual Analysis

For regression models, inspect residual behavior when appropriate.

Check:

* distribution;
* center;
* variance;
* skewness;
* heavy tails;
* outliers;
* relationship with prediction;
* relationship with important features.

Questions include:

* Is error systematically biased?
* Does variance increase with prediction magnitude?
* Are specific regions much harder?
* Are residual patterns indicating missing structure?

Do not require perfectly normally distributed residuals unless the modeling
assumptions require it.

## Segment Analysis

Aggregate metrics can hide important failures.

Evaluate performance by meaningful segments.

Potential segments include:

* class;
* customer type;
* geography;
* device;
* product;
* time period;
* data source;
* difficulty;
* demographic group where appropriate and permitted;
* target range;
* important feature ranges.

Use segments that correspond to actual system behavior.

Ask:

* Does performance collapse in one group?
* Is a small segment responsible for most failures?
* Is average performance masking a critical problem?

## Distribution Analysis

Compare prediction and target distributions.

Inspect when relevant:

* mean;
* median;
* variance;
* tails;
* class distribution;
* predicted score distribution.

Unexpected distribution differences may reveal:

* bias;
* calibration problems;
* clipping;
* preprocessing differences;
* drift.

## Robustness

Evaluate how sensitive the model is to realistic variation.

Potential tests include:

* missing features;
* noisy inputs;
* small perturbations;
* unseen categories;
* rare values;
* distribution shifts;
* degraded data quality.

Do not create unrealistic perturbation tests that have no relationship to
production behavior.

## Temporal Stability

When data evolves over time, measure performance across periods.

Possible analysis includes:

* daily;
* weekly;
* monthly;
* rolling windows;
* pre/post deployment.

Inspect whether performance remains stable.

Do not assume one static test set represents future behavior indefinitely.

## Drift

Distinguish:

### Data Drift

Input distribution changed.

### Concept Drift

Relationship between input and target changed.

### Prediction Drift

Prediction distribution changed.

### Performance Drift

Observed predictive quality changed.

These are related but not equivalent.

Do not claim model degradation solely because a drift detector fired.

Use `ml-engineer` for production monitoring architecture and `debugging` for
unexpected degradation.

## Feature Analysis

Feature analysis can help identify suspicious model behavior.

Inspect when appropriate:

* feature importance;
* permutation importance;
* SHAP values;
* partial dependence;
* relationships with predictions;
* leakage risk.

Feature importance does not imply causality.

A feature can be highly predictive for the wrong reason.

## Explainability

Use explainability to answer concrete questions.

Examples:

* Why did this prediction occur?
* Which variables drive model behavior?
* Is the model relying on suspicious signals?
* Does behavior match domain expectations?

Do not generate explanation plots merely because a library supports them.

Choose explanation methods appropriate to the model and question.

## Overfitting

Compare performance across:

```text id="w9ip07"
training
validation
test
```

Large gaps may indicate overfitting.

But performance gaps can also result from:

* distribution shift;
* leakage;
* split differences;
* preprocessing problems.

Do not diagnose overfitting from one number alone.

## Underfitting

Possible indicators include:

* poor training performance;
* poor validation performance;
* simple residual patterns;
* inability to capture meaningful structure.

Before increasing model complexity, verify:

* data quality;
* target quality;
* features;
* evaluation correctness.

A larger model cannot compensate for a poorly defined problem.

## Hyperparameter Evaluation

When comparing configurations:

* use the same evaluation protocol;
* preserve test-set isolation;
* track what changed;
* compare against a baseline.

Do not repeatedly tune against the final test set.

Use `experiment-designer` for broader experimental methodology.

## Model Comparison

Compare models under equivalent conditions.

Keep consistent when practical:

* dataset;
* split;
* preprocessing;
* target;
* metrics;
* evaluation window.

A model should not be declared better merely because it was evaluated under
different conditions.

## Statistical Uncertainty

Metrics computed on finite data contain uncertainty.

When decisions depend on small differences, consider:

* confidence intervals;
* bootstrap estimates;
* paired comparisons;
* statistical tests;
* variance across folds or runs.

Use `statistician` for deeper statistical analysis.

Do not treat tiny metric differences as meaningful automatically.

## Cross-Validation

Use cross-validation when appropriate to the problem structure.

Possible strategies include:

* K-fold;
* stratified K-fold;
* group K-fold;
* time-series splits.

Do not apply standard random K-fold blindly to temporal or grouped data.

The validation strategy should reflect future usage.

## Forecasting and Time-Dependent Models

For forecasting or temporally dependent prediction, verify:

* chronological split;
* horizon alignment;
* feature availability;
* lag construction;
* rolling windows;
* resampling;
* seasonal behavior;
* leakage from future information.

Evaluate performance by horizon when multiple horizons exist.

Compare against naive temporal baselines.

Do not use future information indirectly through preprocessing.

## Anomaly Detection

For anomaly detection, evaluate based on the actual objective.

Possible considerations include:

* detection rate;
* false alerts;
* precision;
* recall;
* alert volume;
* detection delay.

If labels are weak or incomplete, acknowledge limitations.

Do not rely solely on unsupervised anomaly scores as proof of real-world quality.

## Class Imbalance

When classes are imbalanced:

* inspect class-specific metrics;
* use appropriate baselines;
* analyze PR curves when useful;
* evaluate operational alert volume.

High accuracy can be meaningless in severely imbalanced problems.

## Cost-Sensitive Evaluation

When error types have different consequences, incorporate their relative cost.

Possible approaches include:

* weighted metrics;
* threshold optimization;
* expected cost;
* business simulation.

Do not assume all errors are equally harmful.

## Operational Evaluation

A model can be statistically strong but operationally unusable.

Consider:

* latency;
* throughput;
* model size;
* required hardware;
* data availability;
* prediction frequency;
* failure handling;
* interpretability requirements;
* integration constraints.

Use `ml-engineer` for deeper production-engineering concerns.

## Business Value

When possible, connect predictive quality to actual value.

Questions include:

* What decision changes because of this prediction?
* How often does the model influence a decision?
* What is the cost of errors?
* What benefit does improved prediction produce?
* Is the improvement large enough to justify operational complexity?

Do not convert metrics into financial impact without defensible assumptions.

## Production Readiness

A model is not production-ready merely because offline metrics are strong.

Assess whether:

* evaluation is trustworthy;
* baseline improvement is meaningful;
* important segments are acceptable;
* failure modes are understood;
* data is available at inference time;
* latency is acceptable;
* monitoring is possible;
* rollback exists;
* model behavior matches requirements.

Production readiness is both a model-quality and systems question.

Combine with `ml-engineer` when necessary.

## Evaluation Dataset

The test set should represent expected usage.

Inspect:

* target distribution;
* important segments;
* difficult examples;
* rare cases;
* temporal coverage;
* data source coverage.

Do not assume a large dataset is representative merely because it contains many
rows.

## Error Severity

Not all errors are equally important.

Segment errors by severity where appropriate.

For regression:

* small error;
* moderate error;
* critical error.

For classification:

* low-impact false positive;
* high-impact false positive;
* low-impact false negative;
* high-impact false negative.

Use domain consequences rather than arbitrary categories when possible.

## Failure Taxonomy

Create failure categories when they help guide improvement.

Examples:

* insufficient features;
* noisy input;
* rare segment;
* class confusion;
* target ambiguity;
* data quality;
* drift;
* preprocessing failure;
* extreme values.

A useful taxonomy should lead to specific actions.

## Red Flags

Investigate:

* suspiciously high test performance;
* strong training performance with weak validation;
* high R² with poor practical behavior;
* high accuracy on imbalanced data;
* good aggregate performance but poor critical segments;
* unstable metrics across periods;
* suspicious feature importance;
* train/inference mismatch;
* threshold chosen without analysis;
* performance improvement without baseline comparison;
* evaluation repeatedly tuned against the test set.

These signals require investigation, not automatic rejection.

## When Other Skills Should Be Combined

Examples:

* `model-evaluator` + `ml-engineer` for production-readiness assessment;
* `model-evaluator` + `statistician` for statistical uncertainty;
* `model-evaluator` + `experiment-designer` for controlled comparisons;
* `model-evaluator` + `debugging` for unexplained degradation;
* `model-evaluator` + `feature-engineering` for feature-related failures;
* `model-evaluator` + `eda-specialist` for deeper data investigation.

Use `llm-evaluation`, not this skill, when the primary task is evaluating LLM,
RAG, prompt, or agent behavior.

## Required Output

For substantial model evaluation, report:

### Objective

What the model predicts and what decision depends on it.

### Evaluation Validity

Whether the split, data, and evaluation protocol are trustworthy.

### Baseline

Relevant alternatives and comparison.

### Metrics

Metrics appropriate to the task.

### Error Analysis

Important failure patterns and high-impact errors.

### Segment Analysis

Performance variation across meaningful groups.

### Stability

Temporal or distributional robustness when relevant.

### Risks

Important quality, leakage, bias, robustness, or deployment concerns.

### Recommendation

Whether the evidence supports using, improving, retraining, or rejecting the
model.

For simple evaluation questions, keep output proportional to the request.

## Rules

* Never trust a single metric.
* Never trust aggregate performance alone.
* Validate the evaluation setup before trusting results.
* Always compare against a meaningful baseline when one exists.
* Choose metrics based on the decision and failure costs.
* Investigate leakage when performance is suspiciously strong.
* Analyze errors, not only averages.
* Inspect meaningful segments.
* Distinguish discrimination from calibration.
* Do not interpret feature importance as causality.
* Do not claim drift implies performance degradation automatically.
* Protect the final test set from repeated tuning.
* Report uncertainty when metric differences are small.
* Evaluate operational usefulness, not only predictive performance.
