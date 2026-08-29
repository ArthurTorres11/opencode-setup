---
name: experiment-designer
description: >
  Design rigorous, efficient, and interpretable experiments for machine learning,
  LLMs, RAG, AI agents, prompts, embeddings, NLP, computer vision, optimization,
  data systems, and AI engineering. Use when the task involves a controlled
  technical comparison of alternatives, testing a hypothesis, measuring whether
  a change improves a system, designing an ablation, selecting baselines,
  defining success criteria, controlling variables, or planning reproducible
  experiments. Do not invoke merely because something is described as
  "experimental", for product feature flags, or for exploratory tinkering without
  a measurable comparison or decision criterion.
---

# Experiment Designer

Use this skill when a technical question should be answered through controlled
comparison or empirical evidence.

Typical cases include:

* comparing models;
* testing features;
* comparing prompts;
* comparing embedding models;
* testing retrieval strategies;
* comparing chunking strategies;
* evaluating rerankers;
* comparing agent architectures;
* testing tool descriptions;
* testing context strategies;
* comparing NLP approaches;
* comparing computer vision models;
* ablation studies;
* hyperparameter experiments;
* optimization experiments;
* architecture comparisons;
* performance experiments.

Do not invoke this skill for routine implementation when no meaningful
experimental decision exists.

## Main Goal

Transform uncertainty into a measurable experiment that can support a decision.

Prefer:

```text
question
   ↓
hypothesis
   ↓
controlled experiment
   ↓
evidence
   ↓
decision
```

over:

```text
idea
 ↓
change things
 ↓
look at results
 ↓
decide whether they look good
```

Experiments should reduce uncertainty, not merely produce metrics.

## Start With the Decision

Before designing the experiment, identify the decision the result should support.

Ask:

* What are we deciding?
* What uncertainty prevents that decision?
* What evidence would change the decision?
* What happens if the experiment is inconclusive?

A technically interesting experiment may still be useless if its result does not
affect a real decision.

## Problem Definition

Define:

* current behavior;
* observed problem or uncertainty;
* current baseline;
* proposed change;
* desired outcome;
* relevant constraints.

Avoid vague goals such as:

> Improve the model.

Prefer:

> Determine whether replacing the current embedding model improves retrieval
> recall without unacceptable latency increase.

## Hypothesis

When appropriate, express the hypothesis in testable form.

Useful structure:

```text
If X changes,
then Y should change,
because Z.
```

Example:

```text
If hybrid retrieval replaces dense-only retrieval,
Recall@10 should improve for queries containing exact identifiers,
because sparse retrieval preserves lexical matching.
```

The hypothesis should identify:

* intervention;
* expected effect;
* mechanism or rationale.

Do not invent a causal explanation when only an empirical comparison is needed.

## Exploratory Experiments

Not every useful experiment begins with a strong directional hypothesis.

Exploratory experiments may be appropriate when:

* behavior is poorly understood;
* several plausible mechanisms exist;
* the goal is characterization;
* initial evidence is insufficient to predict direction.

In these cases, define the question and measurements before execution.

Example:

```text
Question:
How does retrieval quality vary with chunk size?

Measure:
Recall@K, Precision@K, answer quality, latency, and context tokens.
```

Exploration should still be structured.

Do not use "exploratory" as justification for uncontrolled trial-and-error.

## Baseline

Define the baseline before testing changes.

Possible baselines include:

* current production system;
* previous model;
* simple model;
* naive predictor;
* current prompt;
* dense-only retrieval;
* current embedding model;
* current agent architecture;
* deterministic workflow;
* no-reranking configuration.

The baseline should represent the relevant alternative.

Do not compare against an artificially weak baseline merely to demonstrate
improvement.

## Experimental Unit

Determine what constitutes one observation in the experiment.

Examples:

* row;
* user;
* query;
* conversation;
* document;
* image;
* session;
* time window;
* agent task;
* retrieval query.

Incorrect experimental units can create misleading conclusions.

## Independent Variable

Identify what is intentionally changed.

Examples:

* model architecture;
* feature set;
* prompt;
* embedding model;
* chunk size;
* retrieval strategy;
* reranker;
* agent architecture;
* tool description;
* optimization algorithm.

Prefer changing one meaningful dimension at a time when the goal is attribution.

## Dependent Variables

Define what outcomes will be measured.

Examples:

* MAE;
* F1;
* Recall@K;
* task success;
* groundedness;
* latency;
* tokens;
* cost;
* memory usage.

Separate primary metrics from diagnostic metrics.

## Controlled Variables

Keep unrelated conditions consistent when possible.

Examples:

* dataset;
* split;
* prompt;
* random seed;
* model parameters;
* retrieval corpus;
* evaluation cases;
* hardware;
* concurrency;
* generation parameters.

A comparison is difficult to interpret when several unrelated variables change
simultaneously.

## Confounding

Identify factors that may explain the observed result besides the intended
change.

Examples:

* different datasets;
* different preprocessing;
* model version changes;
* different evaluation prompts;
* infrastructure changes;
* temporal drift;
* caching;
* different random seeds.

Control or document important confounders.

## Success Criteria

Define success before seeing results.

Success criteria should reflect the actual decision.

They may include:

* minimum quality;
* improvement over baseline;
* non-regression;
* latency ceiling;
* cost ceiling;
* reliability requirement;
* safety constraint.

Example:

```text
Recall@10 must improve over baseline,
while P95 latency must remain below the production limit.
```

Do not invent arbitrary percentage improvements when no meaningful threshold
exists.

If no defensible threshold exists, define how evidence will be interpreted rather
than fabricating one.

## Primary Metric

Choose a primary metric when a single metric can represent the main objective.

This reduces post-hoc metric selection.

Example:

```text
Primary:
Recall@10

Secondary:
MRR
Precision@10

Guardrails:
P95 latency
cost/query
```

Do not select the primary metric after seeing which metric improved.

## Guardrail Metrics

Guardrails prevent optimization of one objective at unacceptable cost elsewhere.

Examples:

```text
quality ↑
while
latency <= limit
cost <= limit
failure rate <= limit
```

Potential guardrails include:

* latency;
* cost;
* memory;
* false-positive rate;
* safety violations;
* token usage;
* failure rate.

## Dataset

Specify the data used in the experiment.

Document when relevant:

* source;
* period;
* sample size;
* filters;
* exclusions;
* preprocessing;
* labels;
* version.

The experimental dataset should represent the intended decision context.

## Split Strategy

Choose a split that reflects future usage.

Possible strategies include:

* random;
* stratified;
* grouped;
* temporal;
* entity-based;
* fixed benchmark.

Do not use random splits automatically.

For temporal problems, preserve chronology when future information would
otherwise leak into training.

For entity-dependent problems, consider whether the same entities may legitimately
appear across splits in production.

## Leakage

Check experimental leakage before interpreting results.

Potential leakage includes:

* target leakage;
* temporal leakage;
* duplicate samples;
* preprocessing leakage;
* evaluation contamination;
* benchmark answers entering training or prompts;
* reference documents entering inappropriate splits.

Use `feature-engineering` or `model-evaluator` when deeper leakage analysis is
needed.

## Sample Size

Ensure the experiment contains enough observations to support the intended
decision.

Small samples may produce unstable results.

Consider:

* expected effect size;
* outcome variance;
* class balance;
* segmentation requirements;
* repeated measurements.

Use `statistician` when formal power or sample-size analysis is required.

## Repeated Runs

Repeat experiments when stochastic variation materially affects conclusions.

Relevant cases include:

* neural network training;
* LLM generation;
* agent execution;
* randomized optimization;
* sampling-based algorithms.

Do not repeat deterministic experiments unnecessarily.

## Random Seeds

Use controlled seeds when reproducibility matters.

But do not rely on one favorable seed.

When randomness materially affects performance, evaluate multiple seeds or runs.

## Ablation Studies

Use ablation to determine which components contribute value.

Example:

```text
baseline
   ↓
+ component A
   ↓
+ component B
   ↓
+ component C
```

Or compare:

```text
full system
vs
without A
vs
without B
```

Ablations are especially useful for:

* feature groups;
* retrieval components;
* agent modules;
* preprocessing;
* architecture components.

Do not create exhaustive ablations when only one decision matters.

## Factorial Experiments

When interactions between multiple factors matter, a factorial design may be more
efficient than changing one variable at a time.

Example factors:

```text
embedding model
×
chunk size
×
reranker
```

Use factorial designs when interactions are plausible and experimental cost
justifies them.

Do not create large combinatorial experiment grids without a clear purpose.

## Machine Learning Experiments

For ML comparisons, keep evaluation conditions consistent.

Control when practical:

* training data;
* validation data;
* test data;
* preprocessing;
* target;
* metrics.

Compare against meaningful baselines.

Use `model-evaluator` for deeper predictive evaluation.

## Feature Experiments

When evaluating new features:

```text
baseline feature set
        ↓
+ candidate feature group
        ↓
same model
same split
same evaluation
```

This isolates feature contribution better than changing features and model
architecture simultaneously.

Use `feature-engineering` for feature design and leakage checks.

## Hyperparameter Experiments

Separate model selection from final evaluation.

Typical structure:

```text
training
   ↓
validation / tuning
   ↓
selected configuration
   ↓
final test
```

Do not repeatedly inspect the final test set while tuning.

## Time-Series Experiments

Define explicitly:

* forecast origin;
* prediction horizon;
* training window;
* validation window;
* test window;
* update frequency.

Possible strategies include:

* fixed holdout;
* expanding window;
* rolling window;
* walk-forward validation.

Never use future information to construct historical predictions.

## NLP Experiments

Control preprocessing and data splits carefully.

Watch for:

* duplicate texts;
* near-duplicates;
* template leakage;
* label leakage;
* entity leakage.

Possible comparisons include:

* lexical features vs embeddings;
* embedding models;
* model architectures;
* preprocessing strategies.

Use `nlp-engineer` when NLP methodology is the central problem.

## Computer Vision Experiments

Control:

* train/validation/test split;
* augmentation;
* image resolution;
* preprocessing;
* model initialization;
* evaluation conditions.

Watch for:

* duplicate images;
* near-duplicates;
* capture-source leakage;
* background shortcuts.

Use `computer-vision-engineer` when CV methodology is the central problem.

## LLM Experiments

LLM experiments require representative evaluation cases.

Potential independent variables include:

* model;
* prompt;
* temperature;
* context;
* structured output strategy.

Measure when relevant:

* task success;
* correctness;
* reliability;
* latency;
* tokens;
* cost.

Use `llm-evaluation` for evaluation methodology.

## Prompt Experiments

When comparing prompts:

1. use the same evaluation dataset;
2. keep model and generation parameters fixed when possible;
3. compare against the existing prompt;
4. inspect regressions;
5. measure token and latency differences.

Do not choose prompts based on a few hand-selected examples.

## RAG Experiments

Separate retrieval and generation.

Possible experiments include:

* chunk size;
* overlap;
* embedding model;
* sparse vs dense retrieval;
* hybrid retrieval;
* Top-K;
* reranking;
* query rewriting;
* contextual retrieval.

Measure retrieval independently when possible.

Example:

```text
retrieval experiment
    ↓
Recall@K / MRR
    ↓
end-to-end experiment
    ↓
answer quality
```

Use `rag-engineer` for RAG architecture and `llm-evaluation` for semantic
evaluation.

## Agent Experiments

Agent experiments may compare:

* deterministic workflow vs agent;
* single agent vs multi-agent;
* tool descriptions;
* routing strategies;
* planning strategies;
* context strategies;
* memory strategies.

Measure more than final answer quality.

Potential metrics include:

* task success;
* tool selection;
* tool-call count;
* trajectory quality;
* termination;
* latency;
* tokens;
* cost.

Use `agent-engineer` and `llm-evaluation` when appropriate.

## Context Experiments

Possible comparisons include:

* full history vs compressed history;
* fixed context vs retrieval-on-demand;
* different memory strategies;
* different context budgets.

Measure both quality and token usage.

Use `context-engineering` for context architecture.

## Tool Experiments

Tool experiments may compare:

* descriptions;
* names;
* schemas;
* tool consolidation;
* routing;
* active tool subsets.

Measure:

* tool-selection accuracy;
* argument correctness;
* unnecessary calls;
* task success.

Use `tool-design` for tool architecture.

## Optimization Experiments

For optimization problems, define:

* objective;
* constraints;
* baseline;
* feasible region;
* evaluation budget.

Compare methods using equivalent budgets when possible.

Do not declare an optimizer better because it used more evaluations or compute.

Use `optimization-engineer` for optimization methodology.

## Performance Experiments

For latency, throughput, or memory experiments, control the execution environment
when possible.

Document:

* hardware;
* software versions;
* concurrency;
* warm-up;
* cache state;
* request size;
* sample count.

Measure distributions, not only averages.

Useful metrics include:

* median;
* P95;
* P99;
* throughput;
* memory;
* CPU/GPU utilization.

## Statistical Analysis

Choose statistical analysis based on the experimental structure.

Potential approaches include:

* confidence intervals;
* bootstrap;
* paired tests;
* permutation tests;
* effect sizes.

Do not automatically run statistical significance tests.

First determine whether the comparison and sample structure justify them.

Use `statistician` for deeper statistical methodology.

## Practical Significance

Statistical significance does not imply useful improvement.

Ask:

* Is the effect large enough to matter?
* Does it change the decision?
* Does it justify added complexity?
* Does it justify increased cost or latency?

Evaluate both statistical and practical significance.

## Multiple Comparisons

Testing many alternatives increases the chance of finding apparent improvements
by chance.

Be cautious with:

* large hyperparameter sweeps;
* many prompts;
* many feature combinations;
* many metrics.

Use appropriate statistical controls when formal inference matters.

More importantly, preserve a final unbiased evaluation when possible.

## Expected Outcomes

Before execution, consider plausible outcomes.

### Positive

Evidence supports the proposed change.

### Neutral

Difference is too small or uncertain to justify the change.

### Negative

The proposed change reduces important performance.

### Mixed

One dimension improves while another degrades.

Define how each outcome affects the decision.

## Decision Rule

Specify how evidence maps to action.

Example:

```text
Adopt B if:
- primary quality metric improves;
- critical segments do not regress materially;
- latency remains within the production limit.

Otherwise retain A.
```

Decision rules should reflect the actual objective.

## Inconclusive Results

An experiment can be inconclusive.

Possible reasons include:

* insufficient sample size;
* high variance;
* weak effect;
* noisy labels;
* unstable evaluation;
* conflicting metrics.

Do not force every experiment into "winner" and "loser".

An inconclusive result is itself information.

## Experiment Tracking

For important experiments, record enough information to reproduce the result.

Consider:

* experiment ID;
* code revision;
* dataset version;
* configuration;
* model;
* prompt;
* random seed;
* metrics;
* artifacts;
* environment.

Do not rely solely on notebook state or memory.

## Experiment Cost

Experiments consume:

* compute;
* API calls;
* engineering time;
* evaluation tokens;
* human review.

Design the smallest experiment capable of resolving the important uncertainty.

Do not run exhaustive experiments when a cheaper discriminating test is
available.

## Sequential Experimentation

For expensive systems, use staged experimentation.

Example:

```text
cheap deterministic checks
        ↓
small representative benchmark
        ↓
larger offline evaluation
        ↓
production experiment
```

Stop early when evidence already rules out an option.

## Production Experiments

When offline evidence is insufficient, controlled production experiments may be
appropriate.

Examples:

* A/B tests;
* shadow mode;
* canary deployment.

Define:

* exposure;
* duration;
* metrics;
* guardrails;
* rollback criteria.

Do not expose users to unnecessary risk merely to collect more data.

## Error Analysis

After measuring results, inspect failures.

Ask:

* Which cases improved?
* Which cases regressed?
* Are failures concentrated?
* Did the change behave according to the proposed mechanism?
* Is the aggregate result hiding important segments?

Experiments should produce understanding, not only a leaderboard.

## Reproducibility

A useful experiment should be reproducible enough for another engineer to
understand:

* what changed;
* what remained fixed;
* what data was used;
* how results were calculated.

Perfect bit-for-bit reproducibility is not always possible, especially with
external LLM APIs.

Document sources of nondeterminism.

## Experiment Failure Modes

Common problems include:

### No Baseline

Improvement cannot be interpreted.

### Multiple Simultaneous Changes

Attribution becomes unclear.

### Undefined Success

Results are interpreted after the fact.

### Leakage

Evaluation becomes invalid.

### Weak Dataset

Cases do not represent intended usage.

### Metric Shopping

Only favorable metrics are reported.

### Test-Set Overfitting

Repeated tuning contaminates final evaluation.

### Uncontrolled Compute

One alternative receives more resources than another.

### Survivorship Bias

Failed experiments disappear from analysis.

### Cherry-Picking

Only favorable examples are shown.

## When Other Skills Should Be Combined

Examples:

* `experiment-designer` + `model-evaluator` for ML comparisons;
* `experiment-designer` + `feature-engineering` for feature ablations;
* `experiment-designer` + `llm-evaluation` for LLM experiments;
* `experiment-designer` + `rag-engineer` for retrieval experiments;
* `experiment-designer` + `agent-engineer` for agent architecture comparisons;
* `experiment-designer` + `context-engineering` for context experiments;
* `experiment-designer` + `tool-design` for tool-routing experiments;
* `experiment-designer` + `statistician` for inference and uncertainty;
* `experiment-designer` + `optimization-engineer` for optimization comparisons.

Do not invoke additional skills unless they materially improve the experiment.

## Required Output

For substantial experiment-design work, report:

### Decision

What decision the experiment should support.

### Question or Hypothesis

What uncertainty is being tested.

### Baseline

What the proposed change is compared against.

### Experimental Design

Data, variables, controls, splits, and execution conditions.

### Metrics

Primary, secondary, and guardrail metrics when relevant.

### Success Criteria

How results will be interpreted.

### Risks

Leakage, confounding, insufficient data, contamination, or other validity risks.

### Analysis Plan

How results and failures will be analyzed.

### Decision Rule

What action follows each meaningful outcome.

For simple experiments, keep the output proportional to the request.

## Rules

* Start from the decision, not the experiment.
* Define the baseline before claiming improvement.
* Use a testable hypothesis when appropriate.
* Allow structured exploratory experiments when the direction is genuinely unknown.
* Define measurements before seeing results.
* Avoid changing multiple unrelated variables simultaneously.
* Prevent leakage and evaluation contamination.
* Keep comparisons fair.
* Do not invent arbitrary success thresholds.
* Distinguish primary metrics from diagnostic metrics.
* Use guardrails when optimization has important tradeoffs.
* Protect final evaluation data from repeated tuning.
* Account for stochastic variation when it matters.
* Distinguish statistical significance from practical significance.
* Treat inconclusive results as valid outcomes.
* Prefer the smallest experiment that resolves the important uncertainty.
* Preserve enough information for reproducibility.
* Analyze failures, not only aggregate scores.
