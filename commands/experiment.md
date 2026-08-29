---

## description: Design and execute a controlled technical experiment to compare alternatives and support a decision

Design and, when possible, execute an experiment for:

$ARGUMENTS

## Objective

Produce evidence that supports a concrete technical decision.

Do not begin by selecting metrics, models, tools, or methods before defining the decision being made.

## 1. Define the decision

State clearly:

* what decision must be made;
* alternatives being considered;
* what would change depending on the result;
* constraints;
* relevant business or system impact.

If the experiment cannot affect a decision, question whether the experiment is necessary.

## 2. Define the hypothesis

When appropriate, express:

* null or baseline expectation;
* alternative hypothesis;
* expected mechanism for improvement;
* conditions under which the hypothesis should hold.

For exploratory experiments where a formal hypothesis is inappropriate, state the exploratory question instead.

Do not invent an expected improvement percentage without evidence.

## 3. Load relevant skills

Inspect the available skills and load only those that materially help design, execute, or evaluate this experiment.

Use experiment-designer when experimental methodology matters.

Combine domain skills only when needed.

Examples may include:

* model-evaluator;
* llm-evaluation;
* statistician;
* rag-engineer;
* feature-engineering;
* ml-engineer;
* nlp-engineer;
* computer-vision-engineer;
* optimization-engineer.

Do not automatically load this entire set.

## 4. Establish the baseline

Identify the strongest meaningful baseline.

The baseline may be:

* current production behavior;
* persistence;
* heuristic;
* existing model;
* existing prompt;
* existing retrieval strategy;
* simpler architecture;
* previous version.

Avoid comparing only against weak baselines.

Record baseline configuration sufficiently for reproduction.

## 5. Define the experimental unit

Determine what constitutes one independent observation.

Examples:

* user request;
* document;
* customer;
* machine;
* prediction timestamp;
* conversation;
* query;
* image;
* session;
* entity.

Account for dependence between observations.

Do not assume rows are independent when they share users, entities, time periods, documents, sources, or environments.

## 6. Define datasets and splits

Determine:

* development data;
* validation data;
* final evaluation data;
* temporal boundaries;
* grouping constraints;
* representative slices;
* leakage risks;
* contamination risks.

Do not repeatedly optimize against the final evaluation set.

Preserve a final holdout when practical.

## 7. Define metrics

Use a small metric set tied to the decision.

Separate when relevant:

### Primary metric

The metric that drives the decision.

### Secondary metrics

Metrics that explain behavior.

### Guardrails

Metrics that must not regress materially.

Possible dimensions include:

* quality;
* error;
* calibration;
* latency;
* cost;
* reliability;
* safety;
* throughput;
* resource usage;
* user impact.

Do not optimize one aggregate metric while ignoring important failure modes.

## 8. Define success criteria

Specify what evidence would justify:

* accepting the change;
* rejecting the change;
* continuing investigation.

Consider both statistical and practical significance when relevant.

Avoid arbitrary percentage thresholds without domain justification.

## 9. Control variables

Identify factors that must remain constant across alternatives.

Examples:

* dataset;
* split;
* seed;
* prompt;
* temperature;
* retrieval corpus;
* hardware;
* preprocessing;
* feature set;
* runtime configuration;
* evaluation rubric.

Change one meaningful factor at a time unless factorial experimentation is intentional.

## 10. Design the experiment

Choose the smallest design capable of answering the question.

Possible designs include:

* controlled offline comparison;
* ablation;
* repeated runs;
* paired comparison;
* bootstrap;
* permutation test;
* A/B test;
* shadow deployment;
* canary;
* temporal backtest;
* cross-validation;
* factorial design.

Prefer cheap offline evidence before expensive online experimentation when appropriate.

## 11. Implement reproducibly

Record:

* code version;
* configuration;
* dataset version;
* seeds;
* environment;
* model or service version;
* prompt version;
* dependencies;
* relevant external services.

Separate experiment configuration from implementation when practical.

Avoid manual undocumented steps.

## 12. Execute

Run the experiment as designed.

Do not change evaluation criteria after observing results unless explicitly documenting a new exploratory phase.

Capture failures and anomalous runs rather than silently discarding them.

For stochastic systems, run enough repetitions to understand variability when practical.

## 13. Analyze results

Compare alternatives using:

* primary metric;
* secondary metrics;
* guardrails;
* uncertainty;
* relevant statistical analysis;
* distributional behavior;
* representative slices.

Inspect averages and tails when operational behavior matters.

Do not interpret statistical significance as practical importance.

Do not interpret non-significance as proof that alternatives are equivalent.

## 14. Perform error analysis

Investigate where each alternative succeeds and fails.

Create useful categories or slices.

Look for:

* systematic failure modes;
* regressions hidden by averages;
* distribution shift;
* leakage;
* unstable results;
* unexpected interactions.

Use examples to explain metrics, not replace them.

## 15. Make the decision

Conclude one of:

* adopt alternative;
* keep baseline;
* collect more evidence;
* redesign experiment;
* investigate an unexpected failure mode.

Tie the conclusion directly to predefined decision criteria.

Do not select a winner simply because one aggregate number is slightly larger.

## 16. Report

Return:

### Decision

What decision the experiment supports.

### Hypothesis

What was tested.

### Baseline

Reference behavior.

### Experimental Design

How the comparison was performed.

### Metrics

Primary, secondary, and guardrails.

### Results

Observed outcomes and uncertainty.

### Error Analysis

Important failure modes and segments.

### Conclusion

Recommended action and why.

### Limitations

What the experiment does not establish.

### Reproduction

Important commands, configuration, datasets, seeds, or artifacts required to reproduce the result.

Do not claim causal conclusions unless the experimental design supports them.
