---
name: debugging
description: >
  Systematically investigate bugs, regressions, failures, degraded performance,
  unexpected behavior, production incidents, and unclear failure modes.
  Use when something is broken, behaving unexpectedly, producing inconsistent
  results, or when the root cause is unknown and evidence-based investigation
  is required.
  Do not invoke for routine feature implementation, new feature design, or general questions about how to build something when there is no failure or unexpected behavior to investigate.
---

# Debugging

Use this skill when a system, application, model, pipeline, API, agent, workflow,
or integration is broken, degraded, inconsistent, or behaving unexpectedly.

Typical cases include:

* tests that suddenly fail;
* APIs becoming slow or unreliable;
* ML model quality degrading;
* agent loops or tool-calling failures;
* RAG quality dropping unexpectedly;
* pipeline or job failures;
* inconsistent outputs;
* unexpected changes after a deployment;
* behavior that cannot yet be explained.

## Main Goal

Identify the actual failure mode and root cause using evidence.

Do not confuse:

* symptoms with causes;
* correlation with causation;
* the first suspicious line with the actual problem;
* a workaround with a root-cause fix.

The goal is to explain why the issue occurs and provide the smallest reliable fix.

## Investigation Principles

Separate:

* observed facts;
* hypotheses;
* evidence;
* conclusions.

Prefer evidence from:

* reproducible execution;
* tests;
* logs;
* traces;
* metrics;
* code;
* configuration;
* data;
* dependency changes;
* version history.

Do not modify code prematurely when the failure mode is still unclear.

Start with the smallest reproducible scope and expand only when necessary.

Prefer tests that discriminate between competing hypotheses.

Always try to disprove the leading hypothesis before accepting it.

## Investigation Framework

### Step 1 - Define the Problem

Establish exactly what is wrong.

Determine:

* What is the observed behavior?
* What is the expected behavior?
* When did the issue start?
* Is it deterministic or intermittent?
* What systems or users are affected?
* What changed recently?
* Can the issue be reproduced?

Create a concise problem statement.

Example:

> Requests using tool X fail after the latest deployment while requests that do
> not use the tool continue to work normally.

Avoid vague statements such as:

> The system is broken.

### Step 2 - Establish a Reproduction

Whenever practical, reproduce the problem before attempting a fix.

Reduce the problem to the smallest case that still demonstrates the failure.

Identify:

* required inputs;
* environment;
* configuration;
* sequence of actions;
* expected result;
* observed result.

If the issue cannot be reproduced, identify what evidence can substitute for a
reproduction, such as logs, traces, historical metrics, or recorded inputs.

### Step 3 - Gather Evidence

Inspect only the evidence relevant to the failure.

Potential sources include:

* logs;
* traces;
* metrics;
* stack traces;
* failing tests;
* code paths;
* configurations;
* environment variables;
* dependency versions;
* data samples;
* schemas;
* recent commits;
* deployments;
* API responses;
* network behavior;
* resource usage.

Do not read the entire repository unless necessary.

Use targeted search to identify the relevant execution path.

Do not begin with a preferred explanation.

Begin with observable evidence.

### Step 4 - Map the Failure Path

Determine the path from input to failure.

For software systems, identify:

1. entry point;
2. relevant processing steps;
3. dependencies;
4. external calls;
5. state changes;
6. failure location;
7. output or observed symptom.

For AI systems, also inspect:

1. model input;
2. prompt or context construction;
3. retrieval or tool calls;
4. model response;
5. parsing or validation;
6. state transitions;
7. final output.

This prevents debugging only the visible symptom.

### Step 5 - Generate Hypotheses

Generate plausible explanations supported by the available evidence.

Do not force a fixed number of hypotheses.

Generate enough alternatives to avoid premature convergence.

Rank them using:

| Hypothesis | Likelihood | Supporting Evidence | Contradicting Evidence | Test |
| ---------- | ---------- | ------------------- | ---------------------- | ---- |

Prefer specific hypotheses.

Bad:

> The model is bad.

Better:

> Retrieval returns irrelevant documents because the metadata filter excludes the
> document category required by this query.

### Step 6 - Test and Eliminate Hypotheses

For each relevant hypothesis:

* identify supporting evidence;
* identify contradicting evidence;
* define the cheapest discriminating test;
* run the test when safe and practical;
* update the hypothesis ranking.

Prefer tests that eliminate several hypotheses at once.

Do not repeatedly change multiple variables during investigation.

When practical, change one relevant factor at a time.

### Step 7 - Identify the Root Cause

A valid root-cause explanation should account for:

* why the problem occurs;
* why the observed symptoms match;
* why it started when it did, if timing is known;
* why unaffected cases continue to work;
* why the evidence supports this explanation.

Distinguish clearly between:

* confirmed root cause;
* probable root cause;
* unresolved hypothesis.

Do not present uncertainty as certainty.

### Step 8 - Design the Fix

Prefer the smallest change that removes the root cause.

Consider:

1. minimal corrective change;
2. regression risk;
3. backward compatibility;
4. affected components;
5. rollback path;
6. tests required.

Avoid unrelated refactoring during a bug fix.

If a temporary mitigation is needed, distinguish it from the permanent fix.

### Step 9 - Validate the Fix

Never consider the problem solved only because code was changed.

Validate using the strongest practical evidence available:

* reproduce the original failure;
* verify that it no longer occurs;
* run targeted tests;
* run relevant existing tests;
* inspect logs or traces;
* compare behavior before and after;
* verify unaffected behavior remains correct.

For bugs, add a regression test when practical.

### Step 10 - Prevent Recurrence

When useful, identify why the system allowed the failure to reach production or
remain undetected.

Possible improvements include:

* regression tests;
* validation checks;
* better error handling;
* observability;
* alerts;
* stronger schemas;
* static analysis;
* clearer contracts;
* dependency pinning;
* evaluation datasets;
* deployment checks.

Do not propose preventive infrastructure unless the benefit justifies the
additional complexity.

## Legacy Systems

When working with unfamiliar or legacy systems, investigate before modifying.

First determine:

* what the system does;
* where execution starts;
* major components and dependencies;
* important data flows;
* external integrations;
* configuration sources;
* persistence or state;
* deployment assumptions;
* tests and validation mechanisms;
* known constraints.

Trace the smallest relevant execution path before reading the entire repository.

Prefer evidence from:

* code;
* tests;
* configuration;
* logs;
* version history;
* documentation;
* runtime behavior.

Do not assume unusual code is incorrect merely because it is old or unfamiliar.

Identify whether strange behavior exists because of:

* compatibility requirements;
* historical migrations;
* external contracts;
* undocumented business rules;
* previous incident fixes;
* deployment constraints.

Before changing legacy code, determine the blast radius.

Check:

* callers;
* consumers;
* shared state;
* schemas;
* public interfaces;
* downstream systems;
* migration implications.

Prefer incremental changes with strong validation over large rewrites.

When architecture is unclear, create a lightweight system map before modifying
important behavior.

## Common Failure Categories

### Software

Check for:

* incorrect control flow;
* invalid assumptions;
* state mutation;
* race conditions;
* concurrency issues;
* exception handling;
* serialization problems;
* dependency changes;
* API contract changes;
* configuration errors.

### Data

Check for:

* missing data;
* duplicates;
* bad joins;
* schema changes;
* type changes;
* invalid transformations;
* unexpected distributions;
* stale data;
* leakage;
* train/inference inconsistencies.

### Machine Learning

Check for:

* data drift;
* concept drift;
* validation errors;
* leakage;
* preprocessing mismatch;
* training/inference skew;
* feature changes;
* model version changes;
* threshold changes;
* calibration issues.

Do not assume that a quality drop automatically implies model drift.

### LLM and Agent Systems

Check for:

* prompt changes;
* context degradation;
* incorrect tool schemas;
* tool selection failures;
* malformed structured output;
* state-management bugs;
* agent loops;
* incorrect termination conditions;
* model version changes;
* retrieval failures;
* context truncation;
* memory contamination;
* fallback logic;
* permission failures;
* nondeterministic behavior.

Separate model behavior from orchestration bugs.

### RAG

Check the pipeline independently:

1. ingestion;
2. parsing;
3. chunking;
4. indexing;
5. query construction;
6. retrieval;
7. filtering;
8. reranking;
9. context assembly;
10. generation.

Do not attempt to fix generation before confirming retrieval quality.

### Infrastructure

Check for:

* resource exhaustion;
* memory pressure;
* disk pressure;
* network failures;
* timeouts;
* rate limits;
* authentication;
* permission changes;
* secrets;
* environment differences;
* deployment configuration;
* service dependencies.

### Performance

Check:

* latency by component;
* blocking I/O;
* repeated work;
* unnecessary model calls;
* inefficient database queries;
* excessive serialization;
* large payloads;
* context size;
* network latency;
* resource saturation.

Measure before optimizing.

## When Other Skills Should Be Combined

Use this skill as the investigation framework.

Combine it with specialized skills when domain knowledge materially improves the
investigation.

Examples:

* `debugging` + `agent-engineer` for agent orchestration failures;
* `debugging` + `rag-engineer` for retrieval-related failures;
* `debugging` + `ml-engineer` for ML pipeline failures;
* `debugging` + `model-evaluator` for unexplained model-quality degradation;
* `debugging` + `python-engineer` for Python implementation problems;
* `debugging` + `statistician` when the issue may be statistical rather than
  software-related.

Do not invoke additional skills without a clear need.

## Required Output

For substantial investigations, report:

### Problem

What is happening and what should happen instead.

### Evidence

Relevant observations and measurements.

### Hypotheses

The plausible explanations considered.

### Investigation

What was inspected or tested and what the results showed.

### Root Cause

The confirmed cause, probable cause, or remaining uncertainty.

### Fix

The smallest reliable corrective action.

### Validation

How the fix was or should be verified.

### Prevention

Relevant measures to reduce recurrence, when justified.

For small bugs, keep the output proportional to the task rather than forcing the
full structure.

## Rules

* Do not jump directly to a fix when the cause is unclear.
* Prefer evidence over intuition.
* Do not stop at the first plausible explanation.
* Try to disprove the leading hypothesis.
* Do not change multiple unrelated variables during diagnosis.
* Do not perform unrelated refactors.
* Distinguish confirmed facts from hypotheses.
* Report uncertainty explicitly.
* Prefer reproducible and measurable evidence.
* Keep investigation proportional to the severity and complexity of the problem.
