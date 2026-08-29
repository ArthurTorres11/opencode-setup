---
name: statistician
description: >
  Apply rigorous statistical reasoning to estimation, uncertainty, hypothesis
  testing, confidence intervals, effect sizes, sampling, distributions,
  dependence, power analysis, multiple comparisons, and interpretation of
  empirical results. Use when statistical validity matters, when uncertainty
  must be quantified, when groups or methods must be compared statistically,
  when sample size or significance is questioned, or when deciding what
  conclusions are justified by observed data.
---

# Statistician

Use this skill when statistical reasoning materially affects the validity of a
conclusion.

Typical cases include:

* estimating uncertainty;
* confidence intervals;
* hypothesis testing;
* effect-size estimation;
* comparing groups;
* comparing models statistically;
* sample-size analysis;
* power analysis;
* distribution analysis;
* bootstrap analysis;
* repeated measurements;
* multiple comparisons;
* correlation analysis;
* statistical assumptions;
* interpretation of experimental results.

Do not invoke this skill merely because numbers or metrics appear in the task.

Use it when the question is about what conclusions the available evidence
actually supports.

## Main Goal

Produce statistically defensible conclusions while making assumptions,
uncertainty, limitations, and practical relevance explicit.

The goal is not to obtain a small p-value.

The goal is to answer:

> What does the available evidence allow us to conclude?

## Start With the Statistical Question

Translate the practical question into a statistical question.

Examples:

```text
Practical:
Is model B actually better than model A?

Statistical:
What is the estimated difference in performance between A and B, and how
uncertain is that estimate?
```

```text
Practical:
Did the new strategy improve conversion?

Statistical:
What is the estimated treatment effect and its uncertainty?
```

Avoid selecting a statistical test before understanding the question.

## Define the Quantity of Interest

Identify what is actually being estimated.

Examples:

* mean;
* median;
* proportion;
* difference in means;
* difference in proportions;
* correlation;
* odds ratio;
* relative risk;
* model metric difference;
* treatment effect;
* quantile;
* variance.

This quantity is the estimand.

Different estimands answer different questions.

Do not choose a method without understanding what quantity it estimates.

## Population and Sample

Distinguish:

```text
population
   ↓
sampling process
   ↓
observed sample
```

Ask:

* What population do we want to understand?
* How was the sample obtained?
* Is the sample representative?
* Are observations independent?
* Are important groups missing?

Large samples do not automatically eliminate sampling bias.

## Unit of Analysis

Identify the correct unit of analysis.

Examples:

* user;
* transaction;
* patient;
* document;
* query;
* image;
* session;
* time period;
* machine;
* model run.

Thousands of rows from a small number of entities do not necessarily represent
thousands of independent observations.

Incorrect units of analysis can severely underestimate uncertainty.

## Descriptive vs Inferential Statistics

Distinguish:

### Descriptive Statistics

Describe observed data.

Examples:

* mean;
* median;
* standard deviation;
* quantiles;
* proportions.

### Inferential Statistics

Reason beyond the observed sample.

Examples:

* confidence intervals;
* hypothesis tests;
* effect estimates;
* uncertainty estimates.

Do not present descriptive patterns as population-level conclusions without
justification.

## Central Tendency

Choose summaries appropriate to the distribution.

### Mean

Useful for expected values but sensitive to extreme observations.

### Median

More robust to extreme values and skewed distributions.

### Quantiles

Useful when tails or asymmetric behavior matter.

Do not automatically report only the mean.

## Variability

Possible measures include:

* variance;
* standard deviation;
* interquartile range;
* median absolute deviation;
* quantile ranges.

Choose based on the distribution and question.

Variability is often as important as central tendency.

## Distribution Analysis

Understand relevant distribution characteristics.

Consider:

* symmetry;
* skewness;
* heavy tails;
* multimodality;
* truncation;
* zero inflation;
* discrete structure;
* bounded variables.

Do not assume a named theoretical distribution merely because it is convenient.

## Normality

Do not automatically require normal data for statistical inference.

Many methods rely on assumptions about:

* residuals;
* sampling distributions;
* model errors;

rather than requiring the raw variable itself to be normally distributed.

Large-sample approximations may also reduce sensitivity to non-normality.

Use normality tests only when normality is relevant to the selected method.

Do not run tests such as Shapiro-Wilk mechanically on every variable.

## Independence

Many statistical methods assume independent observations.

Potential violations include:

* repeated measurements;
* multiple events from the same user;
* temporal autocorrelation;
* clustered data;
* multiple observations from the same device;
* spatial dependence.

Ignoring dependence can underestimate uncertainty.

Consider:

* grouped analysis;
* cluster-robust methods;
* hierarchical models;
* paired methods;
* block bootstrap;
* time-series methods.

## Paired Data

When two methods are evaluated on the same observations, preserve the pairing.

Example:

```text
case 1 → model A error, model B error
case 2 → model A error, model B error
case 3 → model A error, model B error
```

The relevant quantity is often:

```text
difference_i = metric_B_i - metric_A_i
```

rather than comparing two independent samples.

Paired comparisons usually provide more informative estimates when pairing is
valid.

## Missing Data

Before statistical inference, understand missingness.

Potential mechanisms include:

### MCAR

Missing Completely At Random.

### MAR

Missing At Random conditional on observed information.

### MNAR

Missing Not At Random.

These mechanisms have different implications.

Do not automatically discard incomplete observations.

Do not automatically assume missingness is random.

Use `eda-specialist` when the missing-data pattern itself requires deeper
investigation.

## Outliers

Outliers may strongly affect:

* means;
* variance;
* regression coefficients;
* correlation;
* statistical tests.

Determine whether they represent:

* valid extreme observations;
* data errors;
* separate populations;
* measurement failures.

Do not remove observations solely because they are statistically unusual.

Consider robust estimators when appropriate.

## Estimation Before Testing

Whenever possible, start by estimating the magnitude of the effect.

Prefer thinking in terms of:

```text
estimated effect
+
uncertainty
+
practical meaning
```

before:

```text
significant / not significant
```

Hypothesis testing should complement estimation, not replace it.

## Confidence Intervals

Use confidence intervals to communicate uncertainty around estimates.

Possible targets include:

* means;
* proportions;
* metric differences;
* regression coefficients;
* effect sizes.

Interpret intervals in the context of the method used.

A frequentist 95% confidence interval does not literally mean there is a 95%
probability that the fixed parameter lies inside the observed interval.

Avoid overly technical explanations unless they matter to the task.

## Effect Size

Statistical significance does not indicate magnitude.

Report effect sizes when meaningful.

Examples include:

* absolute difference;
* relative difference;
* standardized mean difference;
* odds ratio;
* risk ratio;
* correlation;
* model metric improvement.

Prefer domain-interpretable effects when available.

## Practical Significance

Ask whether the estimated effect matters in practice.

Example:

```text
p < 0.001
```

may coexist with:

```text
effect = operationally negligible
```

Large datasets can make tiny differences statistically detectable.

Always separate:

```text
statistical evidence
```

from:

```text
practical importance
```

## Hypothesis Testing

When hypothesis testing is appropriate, define:

### Null Hypothesis

The reference claim being tested.

### Alternative Hypothesis

The competing claim.

Example:

```text
H0: μ_A = μ_B
H1: μ_A ≠ μ_B
```

or, for directional questions:

```text
H0: μ_B <= μ_A
H1: μ_B > μ_A
```

Choose one-sided tests only when the directional hypothesis was justified before
observing results.

## P-Values

A p-value measures compatibility between observed data and the null model under
the assumptions of the test.

It is not:

* the probability that the null hypothesis is true;
* the probability that the result happened by chance;
* the size of the effect;
* proof of practical relevance.

Do not report p-values without context.

## Significance Thresholds

Thresholds such as:

```text
alpha = 0.05
```

are conventions, not laws of nature.

When formal decision thresholds matter, define them before examining results.

Avoid treating:

```text
p = 0.049
```

and:

```text
p = 0.051
```

as fundamentally different scientific outcomes.

Interpret the complete evidence.

## Choosing a Statistical Test

Select methods based on:

* question;
* estimand;
* data type;
* number of groups;
* pairing;
* dependence;
* distribution;
* sample size;
* assumptions.

Do not choose tests from a memorized lookup table without understanding the
experimental structure.

## Comparing Two Independent Groups

Potential methods include:

* Welch's t-test;
* Student's t-test when equal-variance assumptions are justified;
* Mann-Whitney U when its interpretation matches the question;
* permutation tests;
* bootstrap methods.

Welch's t-test is often preferable to automatically assuming equal variances.

## Comparing Paired Groups

Potential methods include:

* paired t-test;
* Wilcoxon signed-rank test;
* paired permutation tests;
* bootstrap of paired differences.

Preserve pairing whenever the design supports it.

## Comparing More Than Two Groups

Potential methods include:

* ANOVA;
* Welch's ANOVA;
* Kruskal-Wallis;
* regression models;
* permutation methods.

If a global test indicates differences, follow-up comparisons may require
multiple-comparison control.

Do not run many independent pairwise tests without considering multiplicity.

## Categorical Data

Possible methods include:

* chi-square tests;
* Fisher's exact test;
* proportion tests;
* logistic regression.

Choose based on:

* table size;
* expected counts;
* sample size;
* dependence structure.

## Correlation

Possible measures include:

### Pearson

Measures linear association.

### Spearman

Measures monotonic association based on ranks.

### Kendall

Useful for ordinal/rank relationships and some small-sample settings.

Correlation does not imply causation.

Strong correlation may result from:

* confounding;
* shared trends;
* selection effects;
* common causes;
* temporal structure.

## Spurious Correlation in Time Series

Trending variables can produce high correlations even without meaningful
relationships.

Before interpreting temporal correlation, consider:

* trend;
* seasonality;
* autocorrelation;
* non-stationarity;
* shared external drivers.

Do not interpret raw correlation between trending time series as causal evidence.

## Regression Inference

When statistical inference from regression coefficients matters, inspect
assumptions relevant to the model.

Potential issues include:

* nonlinearity;
* heteroskedasticity;
* correlated errors;
* multicollinearity;
* influential observations;
* misspecification.

Predictive performance and inferential validity are different goals.

A model can predict well while individual coefficient interpretations are
unreliable.

## Heteroskedasticity

When error variance changes across observations, standard uncertainty estimates
may be inappropriate.

Possible responses include:

* robust standard errors;
* transformations;
* alternative models;
* explicit variance modeling.

Do not automatically transform data solely because heteroskedasticity exists.

## Bootstrap

Bootstrap methods can estimate uncertainty when analytical formulas are difficult
or unreliable.

Typical process:

```text
observed sample
      ↓
resample with replacement
      ↓
compute statistic
      ↓
repeat
      ↓
bootstrap distribution
```

Possible uses include:

* confidence intervals;
* metric uncertainty;
* model comparison;
* effect-size uncertainty.

The resampling scheme must preserve relevant dependence.

For clustered or temporal data, naive row-level bootstrap may be invalid.

## Permutation Tests

Permutation tests can evaluate whether observed differences are unusual under an
appropriate null hypothesis.

They are useful when:

* analytical distributions are inconvenient;
* paired comparisons exist;
* custom metrics are used.

The permutation mechanism must respect the experimental design.

Do not shuffle data in ways that destroy relevant dependence.

## Power

Statistical power is the probability of detecting an effect of a specified size
under the assumed design.

Power depends on:

* sample size;
* effect size;
* variability;
* significance threshold;
* experimental design.

Low-powered studies can produce highly uncertain conclusions.

## Minimum Detectable Effect

The minimum detectable effect should represent the smallest effect worth
detecting or the smallest effect the design can reliably detect.

Do not choose it arbitrarily merely to obtain a convenient sample size.

Connect it to practical significance whenever possible.

## Sample Size

Sample-size calculations require assumptions.

Possible inputs include:

* expected effect;
* variance;
* desired power;
* significance threshold;
* allocation ratio.

Document assumptions.

A mathematically precise sample-size calculation based on unrealistic assumptions
is not useful.

## Multiple Comparisons

When many hypotheses are tested, false-positive risk increases.

Potential controls include:

* Bonferroni;
* Holm;
* Benjamini-Hochberg false discovery rate.

Choose based on whether controlling family-wise error or false discovery rate is
more appropriate.

Avoid unnecessary testing in the first place.

A focused analysis is often better than correcting hundreds of weak hypotheses.

## Metric Selection Bias

If many metrics are examined and only favorable ones are reported, conclusions
become biased.

Define primary outcomes before analysis when possible.

Report important regressions even when the primary metric improves.

## Repeated Peeking

Repeatedly checking results and stopping when significance appears can inflate
false-positive rates.

When sequential monitoring is necessary, use methods designed for sequential
analysis or define appropriate stopping rules.

Do not treat ordinary fixed-sample p-values as valid under arbitrary optional
stopping.

## Bayesian Reasoning

Bayesian methods may be useful when the task benefits from directly reasoning
about uncertainty in parameters or hypotheses.

Core structure:

```text
prior
  +
likelihood
  ↓
posterior
```

Possible outputs include:

* posterior distributions;
* credible intervals;
* posterior probabilities;
* posterior predictive distributions.

Use priors that are explicit and defensible.

Do not introduce Bayesian methods merely because they are more sophisticated.

## Frequentist vs Bayesian Methods

Choose the framework that best matches:

* question;
* available assumptions;
* decision process;
* team expertise;
* interpretability requirements.

Do not treat either framework as universally superior.

## Robust Statistics

When data contains heavy tails, contamination, or extreme observations, consider
robust approaches.

Examples:

* median;
* trimmed mean;
* MAD;
* robust regression;
* rank-based methods.

Use robustness because the data requires it, not as a default replacement for
standard methods.

## Nonparametric Methods

Nonparametric methods can reduce reliance on specific distributional assumptions.

They are not assumption-free.

For example, rank-based tests still depend on assumptions about sampling,
independence, and the interpretation of distribution differences.

Do not select nonparametric tests automatically whenever normality is rejected.

## Time-Series Statistics

Temporal data often violates independence assumptions.

Inspect when relevant:

* autocorrelation;
* trend;
* seasonality;
* stationarity;
* structural breaks.

Statistical procedures should respect temporal ordering and dependence.

Possible approaches include:

* blocked resampling;
* rolling estimates;
* time-aware comparisons;
* explicit time-series models.

Do not apply ordinary IID methods blindly to temporally dependent observations.

## Model Evaluation

When statistical reasoning is needed for model comparison, focus on uncertainty
around performance differences.

Possible analyses include:

* confidence intervals for metrics;
* paired prediction comparisons;
* bootstrap of metric differences;
* stability across folds;
* stability across time;
* subgroup uncertainty.

Use `model-evaluator` for the overall evaluation framework.

Use `statistician` when the central question is whether the observed difference
is statistically credible or how uncertain it is.

## Cross-Validation Results

Cross-validation folds are not always independent samples.

Do not automatically apply ordinary t-tests to fold scores.

When formal comparison matters, consider the dependence introduced by repeated
training data and validation structure.

Prefer case-level paired comparisons when available and appropriate.

## LLM and Agent Evaluation

LLM outputs may be stochastic and evaluation cases may have heterogeneous
difficulty.

Statistical analysis may help estimate:

* task success rate;
* quality differences;
* judge agreement;
* variance across runs;
* confidence intervals;
* pairwise win rates.

Use `llm-evaluation` for evaluation design.

Use `statistician` for uncertainty and statistical comparison.

## Proportions and Rates

For outcomes such as:

```text
success / failure
correct / incorrect
conversion / no conversion
```

estimate both the observed rate and uncertainty.

For small samples or extreme proportions, simple normal approximations may be
poor.

Choose interval methods appropriate to sample size and data structure.

## Ratio Metrics

Metrics defined as ratios can have unusual statistical behavior.

Examples:

* conversion rate;
* revenue per user;
* click-through rate.

Understand whether the denominator is fixed, random, or correlated with the
numerator.

Avoid treating complex ratio metrics as simple means without checking the
structure.

## Segmented Analysis

When comparing subgroups, inspect:

* sample size;
* uncertainty;
* multiple comparisons;
* interaction effects.

A dramatic subgroup difference based on very few observations may be unstable.

Do not interpret every subgroup fluctuation as meaningful.

## Interaction Effects

If the effect may differ by group, model or test the interaction directly when
appropriate.

Do not infer interaction merely because:

```text
group A: significant
group B: not significant
```

The relevant question is whether the effects themselves differ.

## Simpson's Paradox

Aggregate relationships may reverse after conditioning on important groups.

When aggregate and segmented results conflict, investigate the data-generating
structure.

Do not automatically choose whichever result supports the preferred conclusion.

## Confounding

An observed association may be explained by another variable influencing both
the predictor and outcome.

Statistical adjustment may reduce some confounding, but observational adjustment
does not automatically establish causality.

Be explicit about what assumptions are required.

## Causal Claims

Use causal language carefully.

Statements such as:

```text
X caused Y
```

require stronger assumptions or designs than:

```text
X is associated with Y
```

Randomized experiments provide strong causal identification when properly
designed.

Observational causal inference requires explicit assumptions.

Do not infer causality from:

* correlation;
* feature importance;
* SHAP values;
* predictive performance;
* regression coefficients alone.

## Randomized Experiments

When analyzing randomized experiments, preserve the randomization structure.

Identify:

* treatment assignment;
* randomization unit;
* outcome;
* exclusions;
* noncompliance;
* missing outcomes.

Use `experiment-designer` for experiment architecture.

Use `statistician` for estimation, uncertainty, and inference.

## Observational Studies

For observational data, consider:

* confounding;
* selection bias;
* measurement error;
* reverse causality;
* missing variables.

Regression adjustment alone does not guarantee causal identification.

State limitations clearly.

## Sensitivity Analysis

When conclusions depend strongly on assumptions, test plausible alternatives.

Examples:

* alternative outlier handling;
* alternative missing-data assumptions;
* alternative statistical models;
* alternative inclusion criteria.

Robust conclusions should not depend entirely on one arbitrary analytical choice.

## Assumption Checking

Check assumptions that materially affect the selected method.

Do not mechanically test every possible assumption.

Ask:

```text
Which assumption could invalidate this conclusion?
```

Then investigate that assumption.

## Statistical vs Data Problems

Not every uncertainty problem requires more sophisticated statistics.

Sometimes the real issue is:

* bad data;
* wrong labels;
* leakage;
* sampling bias;
* insufficient coverage.

Use `eda-specialist` or `debugging` when the evidence points to data or system
problems rather than statistical methodology.

## Statistical vs Experimental Problems

Statistics cannot rescue an uncontrolled experiment.

If multiple important variables changed simultaneously, the result may be
impossible to attribute regardless of the statistical test used.

Use `experiment-designer` when experimental validity is the central issue.

## Statistical vs Modeling Problems

Statistical significance does not determine whether a predictive model is good.

Use `model-evaluator` for:

* predictive quality;
* baseline comparison;
* error analysis;
* calibration;
* operational usefulness.

Use `statistician` for:

* uncertainty;
* inference;
* significance;
* effect estimation.

## Communication

Report statistical results in decision-relevant terms.

Prefer:

```text
Model B reduced MAE by an estimated 0.018 compared with Model A.
The uncertainty interval includes improvements close to zero, so the available
evidence is not strong enough to conclude that the improvement is reliable.
```

over:

```text
p > 0.05, therefore there is no difference.
```

Absence of statistical significance is not proof of equality.

## Evidence Strength

Use calibrated language.

Possible conclusions include:

* strong evidence;
* moderate evidence;
* weak evidence;
* inconclusive;
* evidence against the proposed effect.

Avoid binary thinking when the evidence is genuinely uncertain.

## Reproducibility

For important statistical analyses, record:

* dataset;
* filters;
* sample definition;
* estimand;
* method;
* assumptions;
* random seed when relevant;
* confidence level;
* multiple-testing correction;
* software implementation.

The analysis should be reproducible enough for another analyst to verify.

## Common Failure Modes

### P-Value Hunting

Running many analyses until something becomes significant.

### Significance Without Effect Size

Reporting statistical evidence without magnitude.

### Ignoring Dependence

Treating repeated observations as independent.

### Wrong Unit of Analysis

Using rows when users, sessions, or groups are the real units.

### Automatic Normality Testing

Using normality tests without considering whether normality matters.

### Automatic Nonparametric Testing

Switching methods solely because a normality test rejected.

### Ignoring Multiple Comparisons

Testing many hypotheses without accounting for multiplicity.

### Optional Stopping

Stopping data collection when a favorable result appears.

### Causal Overclaiming

Interpreting association as causation.

### Significance Dichotomy

Treating 0.049 and 0.051 as fundamentally different.

## When Other Skills Should Be Combined

Examples:

* `statistician` + `eda-specialist` for distribution and sampling investigation;
* `statistician` + `experiment-designer` for experimental inference;
* `statistician` + `model-evaluator` for ML model comparison;
* `statistician` + `llm-evaluation` for uncertainty in LLM evaluation;
* `statistician` + `feature-engineering` for statistical feature relationships;
* `statistician` + `debugging` when surprising results require investigation;
* `statistician` + `optimization-engineer` for uncertainty around optimization
  results.

Do not invoke additional skills unless they materially improve the analysis.

## Required Output

For substantial statistical analysis, report:

### Statistical Question

What quantity or relationship is being estimated or tested.

### Data Structure

Sample, unit of analysis, groups, pairing, and relevant dependence.

### Assumptions

Assumptions that materially affect the conclusion.

### Method

Recommended statistical method and why it fits the question.

### Estimate

Effect magnitude or quantity of interest when applicable.

### Uncertainty

Confidence interval, credible interval, bootstrap distribution, or other relevant
uncertainty measure.

### Statistical Evidence

Hypothesis-test results when appropriate.

### Practical Interpretation

Whether the estimated effect is large enough to matter.

### Limitations

Sampling, dependence, confounding, missingness, power, or other important risks.

### Recommendation

What the available evidence supports doing or concluding.

For simple statistical questions, keep the output proportional to the request.

## Rules

* Start from the statistical question, not from a statistical test.
* Identify the unit of analysis.
* Distinguish descriptive statistics from inference.
* Estimate effect magnitude before focusing on significance.
* Quantify uncertainty when it matters.
* Do not equate statistical significance with practical importance.
* Do not interpret non-significance as proof of no effect.
* Do not use p-values alone.
* Do not assume normality without reason.
* Do not automatically switch to nonparametric methods after a normality test.
* Preserve pairing and dependence structures.
* Account for multiple comparisons when necessary.
* Do not use ordinary IID methods blindly on temporal or clustered data.
* Do not infer causality from correlation or predictive importance.
* Make assumptions explicit.
* Report uncertainty honestly.
* Prefer confidence intervals and effect sizes when they answer the question.
* Use the simplest statistically valid method that answers the decision.
