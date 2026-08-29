---

## description: Review code or technical changes for correctness, regressions, maintainability, and risk

Review the following code, change, branch, diff, pull request, or implementation:

$ARGUMENTS

## Objective

Determine whether the change is correct, safe, maintainable, and consistent with the existing system.

Prioritize findings that can cause incorrect behavior, regressions, security issues, data problems, operational failures, or significant maintenance cost.

Do not manufacture findings to make the review appear thorough.

## 1. Establish review scope

Determine what is being reviewed:

* diff;
* commit;
* branch;
* pull request;
* files;
* feature;
* implementation;
* architecture change.

Identify the intended behavior and acceptance criteria when available.

Understand what changed before judging how it was implemented.

## 2. Inspect surrounding context

Read enough surrounding code to understand:

* callers;
* dependencies;
* interfaces;
* contracts;
* data flow;
* state;
* configuration;
* tests;
* existing conventions.

Do not evaluate isolated lines without understanding their execution context.

Avoid exploring unrelated parts of the repository.

## 3. Load relevant skills

Inspect the available skills and load only those materially relevant to the review.

Use domain-specific skills when the change involves specialized ML, LLM, RAG, agent, optimization, NLP, vision, statistical, or other technical behavior.

Prefer the smallest useful combination.

## 4. Review correctness

Check whether the implementation actually satisfies its intended behavior.

Look for:

* incorrect assumptions;
* broken control flow;
* invalid state transitions;
* incorrect calculations;
* wrong data handling;
* boundary errors;
* missing validation;
* stale state;
* race conditions;
* incorrect async behavior;
* invalid contracts;
* serialization issues;
* error handling problems.

Trace important execution paths rather than relying only on surface-level style inspection.

## 5. Review regression risk

Identify existing behavior that could be unintentionally affected.

Check:

* callers;
* shared components;
* public interfaces;
* schemas;
* persisted data;
* configuration;
* migrations;
* caches;
* integrations;
* backwards compatibility.

Consider blast radius proportional to the change.

## 6. Review failure behavior

Inspect plausible failure paths.

Consider:

* invalid input;
* empty input;
* missing data;
* exceptions;
* network failures;
* timeouts;
* retries;
* partial failures;
* concurrency;
* external dependencies;
* unavailable services.

Ensure errors fail safely and observably.

## 7. Review security and data exposure

When relevant, check:

* secrets;
* credentials;
* authorization;
* permissions;
* injection;
* unsafe deserialization;
* filesystem access;
* command execution;
* sensitive data;
* logging;
* external calls;
* user-controlled input.

Do not perform a generic security audit unless the change warrants it.

## 8. Review maintainability

Check whether the change:

* follows repository conventions;
* uses existing abstractions appropriately;
* duplicates logic;
* introduces unnecessary coupling;
* creates hidden dependencies;
* adds unnecessary complexity;
* uses clear naming;
* preserves separation of responsibilities.

Do not flag personal style preferences unless they materially affect maintainability.

## 9. Review tests and validation

Determine whether tests prove the behavior that changed.

Check:

* happy path;
* relevant edge cases;
* regression coverage;
* failure paths;
* mocks and fixtures;
* assertions;
* test isolation.

Watch for tests that merely reproduce implementation details without validating useful behavior.

When practical, run focused tests and relevant static checks.

## 10. Validate findings

Before reporting an issue:

* confirm the code path is reachable;
* confirm the behavior is actually problematic;
* check whether another layer already handles it;
* distinguish a real defect from a theoretical concern.

Prefer fewer high-confidence findings over many speculative findings.

## 11. Classify findings

Use:

### Critical

Likely to cause severe production, security, data, or correctness failure and should block the change.

### High

Material bug or regression likely enough to require correction before merge.

### Medium

Real issue with limited impact, maintainability cost, or edge-case failure.

### Low

Minor improvement with concrete value.

Do not inflate severity.

Do not report stylistic preferences as defects.

## 12. Report

Start with the highest-severity findings.

For each finding provide:

* severity;
* affected file/location;
* problem;
* why it matters;
* evidence or execution path;
* smallest recommended correction.

Then provide:

### Validation

Checks or tests performed.

### Overall Assessment

One of:

* APPROVE
* APPROVE WITH MINOR CHANGES
* REQUEST CHANGES
* BLOCK

### Remaining Uncertainty

Anything that could not be validated.

If no meaningful issues are found, say so explicitly rather than inventing findings.
