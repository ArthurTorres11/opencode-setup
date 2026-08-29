---
name: llm-evaluation
description: >
  Design, implement, analyze, and improve evaluation systems for LLMs, RAG,
  agents, prompts, tool use, structured outputs, and other probabilistic AI
  workflows. Use when the task involves evaluation datasets, golden datasets,
  rubrics, LLM-as-judge, deterministic checks, pairwise comparison, regression
  testing, quality metrics, trajectory evaluation, prompt comparison, retrieval
  evaluation, or measuring whether an LLM system actually improved.
  Do not invoke for evaluation of traditional predictive ML regression or classification models; use model-evaluator instead.
---

# LLM Evaluation

Use this skill when evaluating systems whose behavior depends materially on
LLMs or other probabilistic model outputs.

Typical cases include:

* comparing prompts;
* evaluating LLM responses;
* evaluating RAG systems;
* evaluating AI agents;
* evaluating tool calling;
* evaluating structured outputs;
* creating golden datasets;
* regression testing;
* designing evaluation rubrics;
* LLM-as-judge;
* pairwise comparison;
* human evaluation;
* evaluating agent trajectories;
* measuring retrieval quality;
* comparing models;
* validating context strategies;
* measuring production quality.

Do not invoke this skill merely because an application contains an LLM.

Use it when quality needs to be measured, compared, validated, or monitored.

## Main Goal

Determine whether an LLM system behaves correctly and whether a proposed change
actually improves it.

Evaluation should answer questions such as:

* Is the system solving the intended task?
* Is the new version better than the baseline?
* Where does the system fail?
* Are improvements statistically or practically meaningful?
* Did quality improve at the cost of latency or cost?
* Can the evaluation detect regressions?
* Does the evaluation represent real usage?

Do not optimize systems using subjective impressions from a few examples.

## Define Expected Behavior First

Before choosing metrics, define what good behavior means.

Specify:

* task;
* expected outcome;
* constraints;
* unacceptable behavior;
* important edge cases;
* business or user requirements.

Do not begin with:

> Which metric should we use?

Begin with:

> What behavior are we trying to measure?

Metrics should follow expected behavior.

## Evaluation Layers

Separate evaluation into layers.

Typical layers include:

```text
deterministic correctness
        ↓
component quality
        ↓
end-to-end quality
        ↓
operational quality
```

Not every system requires every layer.

## Deterministic Evaluation

Use deterministic checks whenever the property being tested can be evaluated
reliably without another LLM.

Examples:

* valid JSON;
* schema compliance;
* exact IDs;
* required fields;
* forbidden fields;
* tool name;
* number of tool calls;
* execution success;
* citations present;
* URL validity;
* permissions respected;
* output length;
* latency threshold;
* termination condition;
* exact business rule.

Prefer deterministic checks because they are:

* cheaper;
* faster;
* reproducible;
* easier to debug;
* easier to interpret.

Do not use LLM-as-judge for properties that normal code can test reliably.

## Semantic Evaluation

Use model-based or human evaluation when correctness depends on meaning,
interpretation, relevance, or quality.

Examples:

* factual support;
* answer completeness;
* relevance;
* helpfulness;
* instruction following;
* reasoning quality;
* faithfulness;
* groundedness;
* tone;
* semantic equivalence.

Clearly separate semantic evaluation from deterministic validation.

## Evaluation Dataset

Build a representative dataset of evaluation cases.

Each case may contain:

* input;
* relevant context;
* expected behavior;
* expected answer;
* reference documents;
* required tool;
* expected structured fields;
* metadata;
* evaluation criteria.

The exact schema depends on the system.

Avoid creating fields that have no evaluation purpose.

## Golden Dataset

A golden dataset is a curated collection of representative cases used to detect
quality changes and regressions.

It does not necessarily require one exact correct response.

Different cases may define expected behavior using:

### Exact Reference

Useful when only one output is correct.

### Reference Answer

Useful when semantic similarity matters.

### Required Facts

Specify facts that must appear.

### Forbidden Claims

Specify claims that must not appear.

### Rubric

Describe what a high-quality response should accomplish.

### Tool Expectation

Specify expected tool usage.

### Retrieval Expectation

Specify expected documents or evidence.

Choose the representation appropriate to the task.

Do not force free-form LLM tasks into exact string matching.

## Dataset Quality

An evaluation dataset should contain representative variation.

Include when relevant:

* common cases;
* difficult cases;
* ambiguous cases;
* edge cases;
* failure cases;
* adversarial cases;
* previously observed production failures.

Do not create a dataset containing only easy examples.

The dataset should resemble actual usage.

## Dataset Sources

Evaluation cases may come from:

* production examples;
* support tickets;
* observed failures;
* manually authored cases;
* domain experts;
* synthetic generation;
* historical conversations;
* incident reports.

Synthetic data can increase coverage but should not be the only source when real
usage data exists.

## Evaluation Splits

When tuning prompts, systems, or judges, separate development cases from final
evaluation cases when practical.

For example:

```text
development set
    ↓
iterate
    ↓
validation set
    ↓
final evaluation
```

Do not repeatedly optimize against the exact same small benchmark and then treat
its score as unbiased evidence.

Evaluation contamination is possible even without traditional ML training.

## Baseline

Always establish a baseline when measuring improvement.

Possible baselines include:

* current production system;
* previous prompt;
* previous model;
* simple deterministic approach;
* previous retrieval strategy;
* single-agent architecture;
* no-reranking retrieval.

Without a baseline, improvement claims are difficult to interpret.

## Regression Evaluation

Evaluation should detect whether a change breaks previously working behavior.

When fixing an LLM-system bug:

1. preserve the failing example;
2. add it to the evaluation suite when appropriate;
3. verify the old system fails;
4. verify the new system passes;
5. confirm unrelated cases remain stable.

Treat important production failures as candidates for permanent regression tests.

## Metrics

Choose metrics based on the behavior being evaluated.

Possible metrics include:

* task success rate;
* exact-match rate;
* schema-validity rate;
* groundedness;
* correctness;
* completeness;
* retrieval recall;
* retrieval precision;
* tool-selection accuracy;
* tool-argument accuracy;
* trajectory success;
* latency;
* input tokens;
* output tokens;
* total tokens;
* cost;
* failure rate.

Do not collapse all behavior into one metric unless the aggregation has a clear
meaning.

## Aggregate Metrics

Aggregate metrics can hide important failure modes.

Always consider segmentation.

Possible segments include:

* task type;
* difficulty;
* customer group;
* document category;
* language;
* tool;
* query type;
* model;
* failure type.

A system may improve globally while becoming worse for an important subset.

## Rubrics

Use rubrics for semantic properties that require judgment.

A good rubric should define observable criteria.

Bad:

```text
Is the answer good?
```

Better:

```text
Score 1:
The answer fails to address the user's main question.

Score 3:
The answer addresses the main question but misses important supporting details.

Score 5:
The answer directly answers the question, includes the required evidence, and
does not introduce unsupported claims.
```

Avoid vague adjectives without operational definitions.

## Direct Scoring

A judge may assign a score directly.

Example:

```text
1 - incorrect
2 - mostly incorrect
3 - partially correct
4 - mostly correct
5 - fully correct
```

Define each score clearly.

Do not assume numerical precision that the rubric cannot support.

## Pairwise Comparison

Use pairwise evaluation when relative preference is easier to judge than absolute
scoring.

Example:

```text
Response A
vs
Response B
```

Judge:

* A better;
* B better;
* tie.

Pairwise comparison is useful for:

* prompt comparison;
* model comparison;
* answer-quality comparison;
* context strategy comparison.

Randomize answer ordering when possible to reduce positional bias.

## LLM-as-Judge

LLM-as-judge can evaluate semantic qualities at scale.

Use it when:

* deterministic evaluation is insufficient;
* human evaluation is too expensive for every case;
* a clear rubric exists.

Do not treat judge output as unquestionable ground truth.

## Judge Design

A judge should receive only the information necessary to make the evaluation.

Typical judge inputs include:

* task;
* user input;
* expected behavior;
* reference evidence;
* candidate output;
* rubric.

Avoid unnecessary contextual information that may bias the judgment.

Require structured judge outputs when practical.

Example:

```json
{
  "score": 4,
  "label": "mostly_correct",
  "reason": "..."
}
```

Validate the structure deterministically.

## Judge Calibration

Before trusting an automated judge, compare it with human judgments.

Measure whether the judge behaves consistently on representative cases.

Inspect disagreements.

Possible problems include:

* overly generous scores;
* overly harsh scores;
* preference for verbose answers;
* position bias;
* model-family bias;
* sensitivity to formatting;
* reference-answer imitation.

Do not deploy an uncalibrated judge as the sole quality gate for important
systems.

## Judge Bias

Common judge biases include:

* verbosity bias;
* position bias;
* self-preference;
* style preference;
* authority bias;
* reference anchoring.

Reduce bias through:

* explicit rubrics;
* randomized ordering;
* independent evidence;
* pairwise evaluation;
* human calibration;
* multiple judges when justified.

Do not add multiple judges automatically.

Additional judges increase cost and complexity.

## Confidence

Judges may report confidence, but confidence should not automatically be trusted.

If confidence is useful, calibrate it against observed judge accuracy.

For uncertain cases, consider:

* human review;
* second evaluation;
* deterministic checks;
* conservative classification.

## Retrieval Evaluation

Evaluate retrieval independently from answer generation.

Possible retrieval metrics include:

### Recall@K

Did the retrieval results contain the required evidence?

### Precision@K

How much of the retrieved context was actually relevant?

### MRR

How early did the first relevant result appear?

### NDCG

How well were relevant results ranked?

Evaluation may also inspect:

* metadata filtering;
* duplicate chunks;
* source diversity;
* document authority;
* freshness.

Use `rag-engineer` when retrieval design itself needs improvement.

## RAG Evaluation

Separate at least:

```text
retrieval quality
        ↓
context quality
        ↓
answer quality
```

A bad answer may result from:

* missing evidence;
* poor ranking;
* noisy context;
* context truncation;
* generation failure.

Do not label every RAG failure as hallucination.

## Groundedness

Groundedness asks whether claims are supported by the provided evidence.

Evaluate:

* whether important claims have evidence;
* whether evidence actually supports the claim;
* whether the system introduces unsupported facts.

Do not confuse groundedness with factual truth.

A response may faithfully reproduce an incorrect source.

## Agent Evaluation

Agents require more than final-answer evaluation.

Evaluate:

* task completion;
* correct tool selection;
* correct tool arguments;
* valid sequencing;
* constraint compliance;
* recovery from tool errors;
* unnecessary actions;
* termination;
* cost;
* latency.

Use `agent-engineer` for agent architecture problems.

## Trajectory Evaluation

Inspect the agent execution path when intermediate decisions matter.

Example:

```text
input
→ route
→ tool A
→ observation
→ tool B
→ final answer
```

Possible checks include:

* correct tool used;
* required tool used;
* forbidden tool avoided;
* tool ordering;
* repeated calls;
* invalid loops;
* recovery behavior;
* termination reason.

Do not require an exact trajectory when multiple valid approaches exist.

Evaluate invariants instead.

## Tool Evaluation

For tool-using systems, measure:

* tool-selection accuracy;
* argument correctness;
* schema validity;
* unnecessary tool calls;
* recovery from tool failure;
* tool latency.

Separate tool-selection failures from tool implementation failures.

Use `tool-design` when tool descriptions or schemas are the primary issue.

## Structured Output Evaluation

Use deterministic validation first.

Check:

* parse success;
* schema validity;
* required fields;
* enum values;
* data types;
* business constraints.

Semantic evaluation may then inspect whether values are meaningful.

Do not rely on a judge to validate syntax that a parser can check exactly.

## Prompt Evaluation

When comparing prompts:

1. use the same evaluation cases;
2. keep unrelated variables fixed;
3. compare against a baseline;
4. inspect aggregate and segmented results;
5. inspect regressions;
6. measure cost and latency.

Do not choose a prompt because several hand-picked examples look better.

Use `experiment-designer` when designing broader experiments.

## Model Comparison

When comparing models, control relevant variables.

Keep consistent when possible:

* prompts;
* tools;
* context;
* dataset;
* temperature;
* evaluation criteria.

Measure:

* quality;
* latency;
* token usage;
* cost;
* reliability.

The highest-scoring model is not automatically the best production choice.

Evaluate tradeoffs.

## Repeated Runs

LLM outputs may vary.

When nondeterminism matters, run repeated evaluations.

Use repeated runs when you need to estimate:

* reliability;
* variance;
* pass probability;
* intermittent failure rate.

Do not repeat every evaluation unnecessarily.

Use repetition where output variability affects decisions.

## Statistical Interpretation

Evaluation scores are estimates derived from a finite dataset.

When comparing systems, consider:

* sample size;
* variance;
* paired comparisons;
* confidence intervals;
* practical significance.

Use `statistician` for deeper statistical analysis.

Do not report tiny score differences as meaningful without considering
uncertainty.

## Error Analysis

Aggregate scores tell you whether a problem exists.

Error analysis explains what to improve.

Create useful failure categories.

Examples:

* missing evidence;
* wrong retrieval;
* incomplete answer;
* unsupported claim;
* incorrect tool;
* invalid tool arguments;
* instruction violation;
* malformed output;
* agent loop;
* premature termination.

Review representative failures from each category.

Prioritize frequent or high-impact failures.

## Evaluation-Driven Development

Use evaluation as part of the development loop.

```text
define expected behavior
        ↓
build evaluation cases
        ↓
measure baseline
        ↓
make change
        ↓
run evaluation
        ↓
analyze regressions
        ↓
iterate
```

Do not wait until the end of development to design evaluation.

## Offline Evaluation

Offline evaluation uses fixed historical or curated cases.

Advantages:

* reproducible;
* fast;
* safe;
* useful for regression testing.

Limitations:

* may not represent changing production behavior;
* may be vulnerable to benchmark overfitting.

Use offline evaluation before production changes when practical.

## Online Evaluation

Production evaluation may include:

* user feedback;
* task completion;
* escalation rate;
* acceptance rate;
* corrections;
* latency;
* cost;
* A/B testing.

Do not rely solely on explicit thumbs-up/down feedback.

User behavior may provide stronger signals.

## Human Evaluation

Use human evaluation when:

* stakes are high;
* rubrics are difficult to automate;
* judge calibration is required;
* semantic nuance matters;
* automated evaluators disagree.

Provide evaluators with:

* clear instructions;
* consistent rubrics;
* representative examples when necessary.

Avoid unnecessarily exposing model identity if it could bias judgment.

## Production Monitoring

Production evaluation should track both quality and operations.

Potential metrics include:

```text
quality
reliability
latency
tokens
cost
tool failures
fallback rate
human escalation
```

Do not confuse system uptime with AI quality.

A perfectly available system can still produce poor responses.

## Evaluation Gates

Quality gates may be used before deployment.

Example:

```text
deterministic tests pass
        ↓
evaluation benchmark passes
        ↓
critical regressions = 0
        ↓
latency/cost acceptable
        ↓
promotion
```

Do not use one aggregate judge score as the only production gate for important
systems.

## Evaluation Reproducibility

Record enough information to reproduce important evaluations.

Consider tracking:

* evaluation dataset version;
* system version;
* model;
* prompt version;
* retrieval configuration;
* judge model;
* rubric version;
* generation parameters;
* code revision.

Evaluation infrastructure itself must be versioned when results influence
deployment decisions.

## Evaluation Contamination

Watch for cases where benchmark information leaks into the system being tested.

Examples:

* evaluation answers included in prompts;
* tuning directly against a small fixed test set;
* reference answers entering RAG;
* judge seeing metadata that reveals which candidate is expected to win.

Keep final evaluation cases appropriately isolated.

## Cost

Evaluation can become expensive.

Optimize by:

* running deterministic checks first;
* evaluating changed components selectively;
* using smaller judges when validated;
* caching deterministic results;
* running expensive benchmarks at meaningful milestones;
* sampling production traffic appropriately.

Do not reduce evaluation cost by removing the cases most likely to reveal
failures.

## When Other Skills Should Be Combined

Examples:

* `llm-evaluation` + `agent-engineer` for agent evaluation;
* `llm-evaluation` + `rag-engineer` for RAG benchmarks;
* `llm-evaluation` + `context-engineering` for context experiments;
* `llm-evaluation` + `tool-design` for tool-use evaluation;
* `llm-evaluation` + `experiment-designer` for controlled comparisons;
* `llm-evaluation` + `statistician` for uncertainty and significance analysis;
* `llm-evaluation` + `debugging` for unexplained benchmark regressions.

Do not invoke additional skills unless they materially improve the evaluation.

## Required Output

For substantial evaluation work, report:

### Objective

What behavior is being evaluated.

### Dataset

Cases, coverage, sources, and important limitations.

### Evaluators

Deterministic checks, metrics, rubrics, judges, or human evaluation.

### Baseline

System or version used for comparison.

### Results

Aggregate and important segmented results.

### Error Analysis

Important failure patterns and regressions.

### Tradeoffs

Quality, latency, tokens, cost, and complexity when relevant.

### Recommendation

Whether evidence supports the proposed change.

For simple evaluation questions, keep output proportional to the task.

## Rules

* Define expected behavior before choosing metrics.
* Prefer deterministic evaluation when possible.
* Never trust a single aggregate metric blindly.
* Use representative evaluation cases.
* Include important failures and edge cases.
* Establish a baseline before claiming improvement.
* Separate retrieval, generation, and orchestration failures.
* Calibrate LLM judges before relying on them for important decisions.
* Do not treat LLM-as-judge as ground truth.
* Preserve regression cases.
* Watch for evaluation contamination.
* Analyze failures, not only scores.
* Measure operational tradeoffs when relevant.
* Report uncertainty explicitly.
* Keep evaluation complexity proportional to system risk.
