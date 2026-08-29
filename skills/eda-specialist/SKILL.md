---
name: eda-specialist
description: >
  Perform structured exploratory data analysis to understand datasets, data
  quality, distributions, relationships, temporal behavior, anomalies, leakage
  risks, and modeling implications. Use when exploring a new dataset,
  investigating data quality, understanding variables before modeling,
  validating assumptions about data, identifying suspicious patterns, or
  generating evidence-based hypotheses from tabular, temporal, text, image,
  or multimodal datasets.
  Use feature-engineering instead when the primary goal is to design, transform, select, or productionize predictive features rather than to understand the dataset.
---

# EDA Specialist

Use this skill when understanding the data itself is an important part of the
task.

Typical cases include:

* exploring a new dataset;
* understanding data quality;
* investigating unexpected distributions;
* preparing for modeling;
* validating assumptions;
* identifying leakage risks;
* investigating missingness;
* analyzing target behavior;
* discovering meaningful segments;
* generating hypotheses;
* investigating temporal structure;
* understanding dataset composition.

Do not invoke this skill merely because a DataFrame exists.

Use it when exploratory analysis can materially improve understanding or prevent
incorrect conclusions.

## Main Goal

Build an accurate mental model of the dataset before making important modeling
or analytical decisions.

EDA should answer:

* What does each important variable represent?
* What is the unit of observation?
* How was the data generated?
* What quality problems exist?
* What distributions and relationships matter?
* What information may be missing?
* What patterns are suspicious?
* What assumptions appear valid or invalid?
* What could affect downstream modeling?

EDA is investigation, not a checklist of plots.

## Start With the Dataset Contract

Before computing statistics, understand what one row represents.

Determine:

* unit of observation;
* data source;
* collection process;
* target if one exists;
* entity identifiers;
* timestamps;
* important keys;
* known filters;
* expected granularity.

A dataset with 1 million rows is not necessarily 1 million independent
observations.

For example:

```text
one row = transaction
```

is fundamentally different from:

```text
one row = customer-day
```

or:

```text
one row = sensor-minute
```

Do not interpret distributions before understanding the observation unit.

## Dataset Overview

Start with a compact structural overview.

Inspect:

* number of rows;
* number of columns;
* column names;
* data types;
* approximate memory usage when relevant;
* candidate identifiers;
* target variable;
* timestamp columns;
* categorical variables;
* numerical variables.

Avoid dumping enormous dataframe summaries into the output.

Focus on information that affects the investigation.

## Schema Validation

Compare observed data with expected structure when a schema or contract exists.

Check:

* missing columns;
* unexpected columns;
* incorrect types;
* invalid categories;
* impossible ranges;
* malformed identifiers;
* unexpected nullability.

Schema problems should be identified before deeper statistical analysis.

## Data Types

Do not trust inferred data types blindly.

Examples:

```text
"00123" → identifier, not integer
"2026-01-01" → potentially datetime, not string
1/0 → potentially boolean
```

Determine semantic type in addition to storage type.

Useful semantic types include:

* identifier;
* numerical continuous;
* numerical discrete;
* categorical nominal;
* categorical ordinal;
* boolean;
* datetime;
* text;
* geographic;
* target.

## Uniqueness

Inspect uniqueness where relevant.

Check:

* candidate primary keys;
* duplicate IDs;
* duplicated rows;
* repeated observations;
* unexpected one-to-one relationships.

Duplicates may represent:

* legitimate repeated events;
* ingestion duplication;
* joins gone wrong;
* retries;
* snapshots.

Do not remove duplicates before understanding why they exist.

## Missing Values

Measure missingness by column and, when useful, by row or segment.

Inspect:

* percentage missing;
* patterns of co-missingness;
* temporal changes;
* group-specific missingness;
* relationship with target.

Missingness may represent:

* unavailable information;
* source failure;
* delayed information;
* not applicable;
* optional behavior;
* filtering;
* real absence.

Do not assume missingness is random.

## Missingness as Signal

Investigate whether missingness itself carries information.

For example:

```text
feature missing
        ↓
specific workflow or population
        ↓
different target behavior
```

This may suggest a missingness indicator or reveal a data-generation problem.

Do not immediately turn every missing column into an indicator.

## Constant and Near-Constant Variables

Identify:

* constant columns;
* near-constant columns;
* variables with extremely low variation.

These may represent:

* unused fields;
* default values;
* broken ingestion;
* configuration flags;
* legitimate rare events.

Do not remove them automatically.

Understand their role first.

## Cardinality

For categorical variables, inspect:

* number of unique values;
* frequency distribution;
* rare categories;
* dominant categories;
* unseen or malformed values.

High cardinality may affect:

* encoding;
* memory;
* model behavior;
* generalization.

Very high cardinality may also indicate that the column is actually an
identifier.

## Numerical Distributions

For important numerical variables, inspect:

* count;
* mean;
* median;
* standard deviation;
* quantiles;
* minimum;
* maximum;
* skewness when useful;
* tails.

Use robust summaries when extreme values distort averages.

Do not generate every possible statistic for every column without purpose.

## Categorical Distributions

Inspect:

* frequency;
* proportion;
* rare categories;
* dominant categories;
* unexpected values.

Compare distributions across relevant segments when useful.

## Target Analysis

When a target exists, understand it before analyzing predictors.

For regression:

* distribution;
* range;
* central tendency;
* variance;
* tails;
* extreme values;
* missing labels.

For classification:

* class counts;
* class proportions;
* imbalance;
* rare classes;
* missing labels.

For temporal targets:

* evolution over time;
* regime changes;
* seasonal behavior;
* label availability.

Do not start feature-target analysis without understanding the target itself.

## Outliers

Treat outliers as observations requiring explanation, not automatic deletion.

Possible causes include:

* valid rare events;
* measurement error;
* ingestion error;
* unit mismatch;
* population differences;
* unusual but important behavior.

Investigate:

```text
What is unusual?
Why is it unusual?
Is it valid?
Does it matter?
```

Use domain constraints when available.

## Range Validation

Check whether values respect known logical or physical boundaries.

Examples:

```text
probability ∈ [0, 1]
age >= 0
quantity >= 0
```

Unexpected ranges may reveal:

* data corruption;
* unit mismatch;
* sentinel values;
* preprocessing errors.

Do not infer domain boundaries without evidence.

## Sentinel Values

Look for special values representing missing or invalid data.

Examples:

```text
-1
999
9999
"unknown"
"N/A"
"?"
```

These may not be encoded as null.

Treat them according to their actual semantics.

## Relationships Between Variables

Investigate relationships when they help answer a concrete question.

Possible techniques include:

* scatter plots;
* grouped statistics;
* correlations;
* contingency tables;
* conditional distributions.

Do not calculate every pairwise relationship automatically.

Prioritize relationships relevant to:

* target;
* suspected leakage;
* domain hypotheses;
* modeling decisions.

## Correlation

Correlation can identify useful relationships but must be interpreted carefully.

Check:

* Pearson when linear association is relevant;
* Spearman when monotonic association is relevant;
* categorical associations when appropriate.

Remember:

```text
correlation ≠ causation
```

and:

```text
low pairwise correlation ≠ no predictive value
```

Nonlinear relationships and interactions may still exist.

## Multicollinearity

Investigate multicollinearity when it matters for the intended model.

Potential indicators include:

* highly correlated predictors;
* unstable coefficients;
* redundant features;
* variance inflation.

Multicollinearity is particularly relevant for models where coefficient
interpretation or numerical stability matters.

Do not treat correlated predictors as automatically problematic for every model.

## Feature-Target Relationships

Inspect relationships between candidate predictors and target.

For numerical features:

* conditional distributions;
* scatter plots;
* grouped target statistics;
* nonlinear trends.

For categorical features:

* target distribution by category;
* category support;
* rare-category behavior.

Strong relationships should trigger both interest and skepticism.

Extremely predictive variables may indicate leakage.

## Leakage Investigation

EDA should actively search for leakage.

Potential indicators include:

* suspiciously strong target relationships;
* variables created after the outcome;
* target-derived fields;
* duplicated labels;
* future timestamps;
* IDs encoding the target;
* post-event text;
* dataset construction artifacts.

Ask:

> Would this information actually exist when the prediction is made?

Use `feature-engineering` or `model-evaluator` when deeper leakage validation is
required.

## Group Analysis

Important patterns may exist only within groups.

Possible groups include:

* customer;
* product;
* geography;
* source;
* device;
* cohort;
* class;
* document type;
* time period.

Compare:

* counts;
* distributions;
* missingness;
* target behavior;
* data quality.

Aggregate statistics can hide severe group-specific problems.

## Dataset Imbalance

Imbalance can occur beyond classification labels.

Examples:

* one customer dominates observations;
* one device generates most data;
* one time period dominates the dataset;
* one document type dominates a corpus.

Inspect whether the dataset composition represents intended usage.

## Sampling Bias

Ask how observations entered the dataset.

Potential issues include:

* selection bias;
* survivorship bias;
* convenience sampling;
* logging only successful events;
* missing failed cases;
* incomplete historical coverage.

A clean dataset can still be systematically unrepresentative.

## Temporal Data

When timestamps exist, inspect temporal structure even if the task is not
explicitly forecasting.

Check:

* minimum timestamp;
* maximum timestamp;
* ordering;
* duplicated timestamps;
* timezone;
* frequency;
* gaps;
* changes in volume;
* distribution shifts.

Time can reveal dataset-generation changes invisible in aggregate statistics.

## Temporal Coverage

Understand whether all relevant periods are represented.

Check:

* start and end dates;
* missing periods;
* unusually dense periods;
* changes in collection behavior.

Do not compare periods without considering different observation counts.

## Sampling Frequency

For regularly sampled data, verify whether sampling is actually regular.

Inspect time deltas.

Potential issues include:

* missing intervals;
* duplicated timestamps;
* variable frequency;
* batching;
* delayed ingestion.

Do not assume row spacing represents elapsed time.

## Temporal Trends

Inspect whether important variables or targets change over time.

Possible patterns include:

* long-term trend;
* seasonality;
* regime changes;
* sudden shifts;
* gradual drift.

These patterns may affect:

* splitting;
* validation;
* feature engineering;
* model stability.

## Lagged Relationships

For temporal problems, explore lag relationships when justified.

Possible techniques include:

* lagged correlations;
* cross-correlation;
* grouped lag analysis.

Do not interpret cross-correlation as causal evidence.

Watch for:

* autocorrelation;
* shared trends;
* resampling artifacts.

## Time-Series Data Quality

Potential problems include:

* flatlined values;
* frozen measurements;
* sudden jumps;
* repeated values;
* unit changes;
* timestamp shifts;
* clock drift;
* delayed records.

These checks apply to any temporal measurement system, not only industrial data.

## Drift During EDA

Compare distributions across time when dataset evolution matters.

Potential changes include:

* feature distribution;
* missingness;
* category frequency;
* target distribution;
* observation volume.

EDA can identify possible drift but should not automatically conclude concept
drift.

Use `model-evaluator` or `ml-engineer` for deeper model-impact analysis.

## Text Data

For NLP datasets, inspect:

* text length;
* empty texts;
* duplicates;
* near-duplicates;
* language distribution;
* encoding problems;
* templates;
* special tokens;
* label distribution;
* source distribution.

Look for leakage through:

* filenames;
* headers;
* templates;
* metadata;
* post-outcome text.

Use `nlp-engineer` when modeling methodology becomes central.

## Image Data

For computer vision datasets, inspect:

* image count;
* dimensions;
* aspect ratios;
* file formats;
* corrupted images;
* duplicates;
* near-duplicates;
* class distribution;
* source/device distribution.

Look for shortcuts such as:

* watermarks;
* backgrounds;
* borders;
* acquisition-device artifacts;
* filenames.

Use `computer-vision-engineer` when CV methodology becomes central.

## Multimodal Data

For multimodal datasets, inspect both individual modalities and their
relationships.

Check:

* missing modalities;
* mismatched identifiers;
* timestamp alignment;
* duplicate associations;
* inconsistent labels.

Do not assume modalities are correctly aligned merely because they appear in the
same dataset.

## Join Validation

Many data-quality problems originate before the final dataset is created.

For important joins, inspect:

* join keys;
* key uniqueness;
* expected cardinality;
* unmatched rows;
* row-count changes.

Watch for accidental:

```text
one-to-many
many-to-many
```

joins that duplicate observations.

## Dataset Construction

When possible, understand how the dataset was produced.

Map:

```text
raw source
   ↓
filters
   ↓
joins
   ↓
aggregations
   ↓
transformations
   ↓
final dataset
```

EDA of only the final table may miss upstream problems.

Use `debugging` when dataset construction itself appears incorrect.

## Hypothesis Generation

EDA should produce testable hypotheses.

Examples:

```text
Missingness increased after a source-system change.
```

```text
Most classification errors may occur in one rare category.
```

```text
The target distribution changed after a specific date.
```

Separate:

```text
observation
hypothesis
conclusion
```

Do not present exploratory patterns as confirmed explanations.

## Visualization Strategy

Use plots to answer questions.

Examples:

### Distribution

Histogram, ECDF, box plot.

### Relationship

Scatter plot, grouped distribution.

### Category

Count or proportion plot.

### Time

Line plot or temporal aggregation.

### Missingness

Missingness summary or pattern visualization.

Avoid creating plots without a question they are intended to answer.

## Visualization Scale

Consider transformations when distributions are highly skewed.

Examples:

* log scale;
* normalized frequencies;
* quantile views.

Make transformations explicit.

Do not visually manipulate axes in ways that exaggerate differences.

## Efficient EDA

Large datasets may not require full scans for every analysis.

Consider:

* column pruning;
* sampling;
* aggregated statistics;
* efficient dataframe engines.

Sampling is acceptable for exploratory visualization when the limitation is
understood.

Use the full dataset for conclusions requiring exact counts or rare-event
analysis.

## EDA and Modeling

EDA should inform modeling decisions.

Potential implications include:

```text
high cardinality
→ encoding strategy

temporal drift
→ temporal validation

class imbalance
→ appropriate metrics

duplicate entities
→ grouped split

heavy tails
→ robust loss or transformation

missingness pattern
→ missingness strategy
```

Do not turn every observation into a modeling change.

Prioritize patterns with plausible impact.

## EDA and Feature Engineering

EDA often identifies candidate features.

Examples:

* useful interactions;
* temporal behavior;
* missingness signal;
* group structure;
* transformations.

Use `feature-engineering` when moving from observations to production-quality
feature design.

## EDA and Evaluation

EDA can reveal how evaluation should be structured.

Examples:

```text
strong temporal change
→ temporal split

repeated users
→ grouped validation

rare critical class
→ segmented metrics
```

Use `model-evaluator` for formal model evaluation.

## EDA and Statistics

EDA identifies patterns.

Statistical analysis determines how strongly evidence supports conclusions.

Use `statistician` when questions require:

* inference;
* uncertainty;
* hypothesis testing;
* confidence intervals;
* effect estimation.

Do not use statistical tests merely because an exploratory pattern looks
interesting.

## Reproducibility

Important EDA steps should be reproducible.

Prefer:

* deterministic transformations;
* clear filtering;
* explicit assumptions;
* named intermediate variables;
* reusable functions when repeated logic exists.

Do not mutate the original dataset unnecessarily.

## Code Style

When writing EDA code:

* keep analysis steps focused;
* avoid giant cells or scripts;
* use descriptive variable names;
* separate transformation from visualization when useful;
* avoid repeated expensive computation;
* comment reasoning, not obvious syntax.

Prefer simple analysis code over premature abstraction.

Reusable functions are useful for repeated operations, not mandatory for every
EDA step.

## Investigation Order

A useful default sequence is:

```text
understand dataset
        ↓
validate structure
        ↓
inspect quality
        ↓
understand target
        ↓
inspect distributions
        ↓
investigate relevant relationships
        ↓
check leakage
        ↓
inspect segments/time
        ↓
generate hypotheses
        ↓
define next analyses
```

Adapt the order to the problem.

Do not mechanically execute every step when it is irrelevant.

## Common Failure Modes

### Plot-First EDA

Generating many charts without understanding the dataset.

### Metric Dumping

Producing every descriptive statistic without interpretation.

### Automatic Cleaning

Removing outliers, duplicates, or missing values before understanding them.

### Leakage Blindness

Finding strong relationships without checking whether they are valid.

### Aggregate-Only Analysis

Missing important temporal or segment behavior.

### Causal Overinterpretation

Treating correlation as explanation.

### Dataset Isolation

Ignoring how upstream transformations created the final data.

## When Other Skills Should Be Combined

Examples:

* `eda-specialist` + `feature-engineering` for feature discovery;
* `eda-specialist` + `model-evaluator` for evaluation design;
* `eda-specialist` + `statistician` for statistical inference;
* `eda-specialist` + `experiment-designer` for hypothesis testing;
* `eda-specialist` + `debugging` for suspicious data pipelines;
* `eda-specialist` + `ml-engineer` for production data problems;
* `eda-specialist` + `nlp-engineer` for text datasets;
* `eda-specialist` + `computer-vision-engineer` for image datasets.

Do not invoke additional skills unless they materially improve the investigation.

## Required Output

For substantial EDA work, report:

### Dataset Overview

Observation unit, structure, important variables, target, and coverage.

### Data Quality

Missingness, duplicates, invalid values, schema issues, and other important
problems.

### Distribution Findings

Important characteristics of targets and predictors.

### Relationships

Relevant associations, segments, or temporal patterns.

### Leakage Risks

Potential invalid information or evaluation contamination.

### Hypotheses

Evidence-based explanations or questions worth testing.

### Modeling Implications

How findings may affect features, splits, metrics, or model design.

### Next Analyses

The smallest useful analyses or experiments needed to resolve remaining
uncertainty.

For simple EDA tasks, keep output proportional to the request.

## Rules

* Understand the observation unit before interpreting statistics.
* Do not treat EDA as a fixed checklist.
* Do not generate plots without a question.
* Do not remove outliers without understanding them.
* Do not fill missing values automatically.
* Do not assume missingness is random.
* Do not remove duplicates without understanding their origin.
* Do not trust inferred data types blindly.
* Investigate suspiciously strong target relationships for leakage.
* Distinguish observations from hypotheses and conclusions.
* Do not interpret correlation as causation.
* Inspect meaningful segments when aggregates may hide problems.
* Inspect temporal structure when timestamps exist.
* Consider how the dataset was constructed.
* Prefer targeted investigation over exhaustive analysis.
* Connect findings to concrete downstream decisions.
