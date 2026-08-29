---

## description: Systematically investigate a bug, failure, regression, or unexpected behavior

Investigate the following problem:

$ARGUMENTS

## Objective

Determine the most likely root cause using evidence, not intuition, and identify the smallest safe corrective action.

Do not immediately modify code.

## 1. Understand the problem

Establish:

* expected behavior;
* observed behavior;
* when the problem occurs;
* known reproduction conditions;
* affected components;
* recent relevant changes, if available;
* impact and scope.

Separate clearly:

* facts;
* observations;
* assumptions;
* hypotheses;
* unknowns.

Do not treat the user's suspected cause as established fact.

## 2. Inspect the environment

Before forming strong conclusions, inspect the smallest relevant portion of the repository and runtime context.

Identify:

* entry points;
* execution path;
* relevant components;
* dependencies;
* configuration;
* data flow;
* state;
* integrations;
* tests;
* logs or errors when available.

Avoid broad repository exploration unless evidence requires it.

## 3. Load relevant skills

Inspect the available skills and load only those that materially help investigate this specific problem.

Do not load skills merely because their domain is mentioned.

Prefer the smallest useful combination.

## 4. Reproduce or establish evidence

When practical:

* reproduce the failure;
* inspect logs;
* run existing tests;
* inspect relevant data;
* compare expected and actual state;
* inspect recent changes;
* verify configuration and environment assumptions.

Prefer deterministic evidence over model judgment.

If reproduction is impossible, state why and continue with the strongest available evidence.

## 5. Build hypotheses

Generate a small ranked set of plausible hypotheses.

For each hypothesis identify:

* supporting evidence;
* contradicting evidence;
* missing evidence;
* a discriminating check that could confirm or reject it.

Avoid generating large speculative lists.

## 6. Test hypotheses

Run the cheapest and most discriminating checks first.

Update the hypothesis ranking as evidence changes.

Do not keep disproven hypotheses alive without new evidence.

If the investigation reveals a different failure path, update the scope explicitly.

## 7. Determine root cause

Distinguish between:

* symptom;
* immediate cause;
* root cause;
* contributing factors.

Do not claim root cause unless evidence supports it.

If evidence is insufficient, report the leading hypothesis and what remains necessary to confirm it.

## 8. Propose the smallest safe fix

Once the cause is sufficiently established:

* identify the smallest corrective change;
* preserve existing architecture and behavior where possible;
* avoid unrelated refactoring;
* identify regression risk;
* identify affected tests.

Do not implement the fix yet unless the user explicitly requested implementation as part of the investigation.

## 9. Validate

If a fix is implemented:

* reproduce the original failure before the fix when practical;
* verify the failure no longer occurs;
* run relevant existing tests;
* add a regression test when appropriate;
* check for nearby regressions;
* inspect the final diff.

Use the strongest practical validation available.

## 10. Report

Return a concise technical report containing:

### Problem

Expected vs observed behavior.

### Evidence

The strongest evidence collected.

### Root Cause

Confirmed root cause, or leading hypothesis if not fully confirmed.

### Fix

Smallest recommended or implemented corrective action.

### Validation

Checks and tests performed.

### Remaining Risk

Uncertainty, unresolved questions, or follow-up work.

Do not hide uncertainty.

Do not report a hypothesis as a confirmed root cause.
