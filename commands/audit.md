---

## description: Audit a system, repository, component, or technical area for evidence-based risks, weaknesses, and improvement priorities

Audit the following system, repository, component, or technical area:

$ARGUMENTS

## Objective

Identify meaningful technical risks, weaknesses, gaps, and improvement opportunities using evidence from the actual system.

Prioritize issues that can materially affect correctness, reliability, security, maintainability, data quality, model quality, operational behavior, cost, or user impact.

Do not generate a generic checklist of best practices.

Do not invent findings merely to make the audit appear comprehensive.

## 1. Define the audit scope

Establish:

* system or component being audited;
* audit objective;
* relevant users or consumers;
* expected behavior;
* critical workflows;
* important constraints;
* production or non-production status;
* known incidents or concerns;
* areas explicitly in scope;
* areas explicitly out of scope.

If the requested scope is broad, organize it into a small number of meaningful audit dimensions before inspecting deeply.

Do not silently expand the scope.

## 2. Understand the system

Inspect the smallest amount of repository and runtime context necessary to build an accurate system map.

Identify when relevant:

* entry points;
* major components;
* execution paths;
* interfaces;
* dependencies;
* data flow;
* state;
* configuration;
* external services;
* infrastructure;
* storage;
* tests;
* deployment;
* observability;
* security boundaries;
* AI or ML components.

Do not assume architecture from filenames alone.

Verify important paths in code or configuration.

## 3. Load relevant skills

Inspect the available skills and load only those materially relevant to the audit.

Use specialized skills when the audit contains domain-specific concerns.

Possible examples include:

* python-engineer;
* debugging;
* ml-engineer;
* model-evaluator;
* rag-engineer;
* agent-engineer;
* context-engineering;
* tool-design;
* llm-evaluation;
* statistician;
* optimization-engineer;
* nlp-engineer;
* computer-vision-engineer.

Do not automatically load every related skill.

Prefer the smallest useful combination.

## 4. Build an evidence map

For each audit dimension, identify the evidence sources that can actually support conclusions.

Examples:

* source code;
* configuration;
* tests;
* schemas;
* pipelines;
* prompts;
* model artifacts;
* experiment results;
* logs;
* traces;
* metrics;
* deployment files;
* infrastructure definitions;
* documentation;
* version history;
* runtime behavior.

Distinguish clearly between:

* verified fact;
* observation;
* inferred risk;
* unsupported possibility.

Do not report unsupported possibilities as findings.

## 5. Identify critical paths

Focus first on paths where failure would matter most.

Examples:

* user-facing requests;
* authentication or authorization;
* financial or irreversible operations;
* data ingestion;
* model inference;
* retrieval;
* agent tool execution;
* state mutation;
* persistence;
* external integrations;
* deployment;
* recovery paths.

Trace these paths end to end when practical.

## 6. Audit correctness

Look for evidence of:

* incorrect logic;
* invalid assumptions;
* inconsistent contracts;
* bad state transitions;
* incorrect calculations;
* unsafe defaults;
* silent failures;
* missing validation;
* stale state;
* schema mismatch;
* train-inference inconsistencies;
* incorrect fallback behavior;
* invalid error handling.

Prefer concrete execution paths over theoretical concerns.

## 7. Audit reliability

When relevant, assess:

* timeouts;
* retries;
* idempotency;
* concurrency;
* partial failure;
* external dependency failures;
* resource exhaustion;
* degraded-mode behavior;
* fallback paths;
* startup and shutdown behavior;
* state recovery;
* failure isolation.

Check whether failures are observable and diagnosable.

## 8. Audit security and permissions

Only when relevant to the scope, inspect:

* authentication;
* authorization;
* least privilege;
* secrets;
* credential handling;
* injection;
* command execution;
* unsafe deserialization;
* filesystem access;
* data exposure;
* logging of sensitive information;
* external calls;
* user-controlled input;
* tool permissions;
* high-impact actions.

Do not turn every audit into a full security assessment unless requested.

## 9. Audit data and AI behavior

When data, ML, LLM, RAG, or agents are involved, assess the appropriate layers separately.

Potential dimensions include:

### Data

* source quality;
* schema;
* missingness;
* duplicates;
* freshness;
* lineage;
* leakage;
* representativeness;
* train-serving consistency.

### Traditional ML

* baseline;
* split strategy;
* leakage;
* calibration;
* robustness;
* drift;
* reproducibility;
* inference consistency;
* monitoring.

### LLM systems

* deterministic validation;
* prompt and context construction;
* structured outputs;
* fallbacks;
* refusal behavior;
* evaluation datasets;
* regression evaluation;
* cost and latency.

### RAG

* ingestion;
* parsing;
* chunking;
* metadata;
* retrieval;
* filtering;
* reranking;
* context assembly;
* grounding;
* citations;
* retrieval evaluation.

### Agents

* tool contracts;
* permissions;
* loop bounds;
* termination;
* retries;
* state;
* memory;
* delegation;
* human approval;
* trajectory evaluation.

Do not collapse these layers into a single "AI quality" judgment.

## 10. Audit observability

Check whether important behavior can be understood after failure.

When relevant assess:

* structured logs;
* meaningful errors;
* metrics;
* traces;
* correlation IDs;
* model or prompt version;
* tool calls;
* latency;
* token or compute usage;
* deployment version;
* input and output metadata;
* failure classification.

Observability should support diagnosis, not merely produce logs.

## 11. Audit tests and validation

Assess whether current validation provides confidence in critical behavior.

Check:

* test coverage of important workflows;
* regression tests;
* edge cases;
* failure behavior;
* integration tests;
* deterministic checks;
* evaluation datasets;
* data validation;
* static analysis;
* deployment checks.

Do not judge quality by test count alone.

Determine what important behavior is currently unproven.

## 12. Audit maintainability

Look for issues with concrete maintenance impact:

* excessive coupling;
* duplicated logic;
* hidden dependencies;
* unclear ownership;
* overly large modules;
* inconsistent abstractions;
* fragile configuration;
* hard-coded assumptions;
* undocumented critical behavior;
* dead code;
* dependency risk.

Do not report subjective style preferences.

## 13. Audit performance and cost

Only when relevant, inspect:

* latency;
* throughput;
* memory;
* CPU or GPU usage;
* network calls;
* database queries;
* external API calls;
* token usage;
* model cost;
* unnecessary retries;
* redundant computation;
* caching.

Do not label something a performance issue without evidence or a plausible critical path.

## 14. Validate each finding

Before including a finding, verify:

* the code path or behavior actually exists;
* the risk is reachable;
* the issue is not already mitigated elsewhere;
* evidence supports the claim;
* impact is meaningful enough to report.

Prefer a smaller set of high-confidence findings over a long speculative list.

## 15. Classify findings

Use:

### Critical

Immediate or highly probable severe correctness, security, data, production, or business risk.

### High

Material problem that should be addressed before relying on the affected behavior.

### Medium

Real weakness with meaningful but limited impact.

### Low

Concrete improvement with limited risk.

### Observation

Useful architectural or operational note that is not itself a defect.

Do not inflate severity.

## 16. Prioritize remediation

For every actionable finding, consider:

* impact;
* likelihood;
* confidence;
* implementation effort;
* blast radius;
* dependency on other fixes.

Prefer fixes that reduce the most risk with the least unnecessary change.

Identify dependencies between remediation items.

## 17. Propose remediation

For each finding provide:

* smallest useful correction;
* files or components likely affected;
* validation required;
* regression risk;
* whether the fix can be incremental;
* whether architecture changes are actually necessary.

Do not recommend major rewrites unless evidence shows incremental correction is insufficient.

## 18. Produce an execution order

Group recommendations into a practical sequence.

Prefer:

1. immediate correctness or security risks;
2. missing validation that blocks confidence;
3. reliability and observability gaps;
4. architectural improvements;
5. performance or maintainability improvements.

Avoid arbitrary roadmaps based only on perceived elegance.

## 19. Report

Return the audit in this structure:

# Audit Summary

Brief system description, scope, and overall assessment.

# System Map

Relevant components and critical execution paths.

# Findings

For each finding:

## [Severity] Finding title

### Evidence

Exact supporting evidence.

### Impact

What can realistically go wrong.

### Root Cause or Mechanism

Why the issue exists.

### Recommendation

Smallest useful corrective action.

### Validation

How the correction should be proven.

### Confidence

HIGH / MEDIUM / LOW.

# Strengths

Important things the system already does correctly.

Do not omit this section when supported by evidence.

# Prioritized Remediation Plan

Ordered actions based on risk and dependency.

# Unverified Areas

Areas that could not be assessed due to missing evidence.

# Final Assessment

State whether the system is:

* HEALTHY;
* HEALTHY WITH IMPROVEMENTS;
* NEEDS ATTENTION;
* HIGH RISK.

Explain the rating briefly.

Do not claim certainty beyond the available evidence.
