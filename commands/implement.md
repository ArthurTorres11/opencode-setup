---

## description: Implement a feature, change, integration, or technical requirement safely and incrementally

Implement the following change:

$ARGUMENTS

## Objective

Deliver the smallest sound implementation that satisfies the requirement while preserving existing architecture, behavior, and maintainability.

Do not begin modifying code before understanding the relevant execution path and constraints.

## 1. Understand the requirement

Establish:

* what must change;
* expected behavior;
* acceptance criteria;
* affected users or systems;
* inputs and outputs;
* known constraints;
* compatibility requirements;
* failure behavior;
* what explicitly should not change.

Separate confirmed requirements from assumptions.

Do not invent product requirements when they are not provided.

If a minor ambiguity can be safely resolved from repository conventions or surrounding code, inspect the repository instead of stopping unnecessarily.

## 2. Inspect the existing system

Read the smallest relevant portion of the repository.

Identify:

* entry points;
* current execution path;
* relevant modules;
* interfaces and contracts;
* data flow;
* configuration;
* dependencies;
* existing abstractions;
* tests;
* similar implementations already present.

Prefer extending established patterns over introducing new architecture.

Do not perform broad repository exploration unless necessary.

## 3. Load relevant skills

Inspect the available skills and load only those that materially help with this implementation.

Use specialized skills when the task clearly belongs to their domain.

Do not load skills merely because a technology or domain word appears in the request.

Prefer the smallest useful combination.

## 4. Design the smallest sound solution

---

## description: Implement a feature, change, integration, or technical requirement safely and incrementally

Implement the following change:

$ARGUMENTS

## Objective

Deliver the smallest sound implementation that satisfies the requirement while preserving existing architecture, behavior, and maintainability.

Do not begin modifying code before understanding the relevant execution path and constraints.

## 1. Understand the requirement

Establish:

* what must change;
* expected behavior;
* acceptance criteria;
* affected users or systems;
* inputs and outputs;
* known constraints;
* compatibility requirements;
* failure behavior;
* what explicitly should not change.

Separate confirmed requirements from assumptions.

Do not invent product requirements when they are not provided.

If a minor ambiguity can be safely resolved from repository conventions or surrounding code, inspect the repository instead of stopping unnecessarily.

## 2. Inspect the existing system

Read the smallest relevant portion of the repository.

Identify:

* entry points;
* current execution path;
* relevant modules;
* interfaces and contracts;
* data flow;
* configuration;
* dependencies;
* existing abstractions;
* tests;
* similar implementations already present.

Prefer extending established patterns over introducing new architecture.

Do not perform broad repository exploration unless necessary.

## 3. Load relevant skills

Inspect the available skills and load only those that materially help with this implementation.

Use specialized skills when the task clearly belongs to their domain.

Do not load skills merely because a technology or domain word appears in the request.

Prefer the smallest useful combination.

## 4. Design the smallest sound solution

Before editing, determine:

* files that need modification;
* interfaces affected;
* data or state changes;
* validation required;
* compatibility implications;
* expected failure modes;
* tests needed.

Prefer:

* existing abstractions;
* small cohesive changes;
* explicit contracts;
* deterministic behavior where possible;
* reversible changes.

Avoid:

* unrelated refactoring;
* premature abstractions;
* unnecessary dependencies;
* speculative extensibility;
* broad architectural changes without evidence they are required.

## 5. Establish a validation plan

Determine how the implementation will be proven correct.

Use the strongest practical combination of:

* existing tests;
* new focused tests;
* type checking;
* linting;
* static analysis;
* compilation;
* schema validation;
* runtime checks;
* integration tests;
* representative examples;
* AI or ML evaluations when deterministic testing is insufficient.

For probabilistic AI behavior, use deterministic checks before model-based evaluation whenever possible.

## 6. Implement incrementally

Make the smallest coherent change first.

After each meaningful change:

* verify syntax and structure;
* check affected contracts;
* run focused validation when practical;
* inspect unexpected side effects before continuing.

Do not accumulate a large unvalidated patch when smaller checkpoints are possible.

Preserve existing public behavior unless the requirement explicitly changes it.

## 7. Handle errors and edge cases

Consider relevant:

* invalid input;
* missing data;
* empty states;
* timeouts;
* retries;
* partial failures;
* external dependency failures;
* concurrency;
* idempotency;
* backwards compatibility;
* security and permissions.

Only handle edge cases that are plausible for the system.

Do not add speculative complexity for unrealistic scenarios.

## 8. Test the implementation

Run the most focused tests first.

Then, when practical:

* run related test suites;
* run static checks;
* test representative success cases;
* test relevant failure cases;
* verify integrations;
* verify configuration;
* verify backward compatibility.

If tests fail, investigate the cause rather than weakening tests to make them pass.

Do not modify unrelated tests simply because they expose an existing issue.

## 9. Review the final diff

Before considering the implementation complete, inspect all changes.

Check for:

* accidental edits;
* unnecessary files;
* duplicated logic;
* dead code;
* debug output;
* exposed secrets;
* inconsistent naming;
* missing error handling;
* missing tests;
* unintended API changes;
* excessive complexity.

Remove changes that are not necessary for the requirement.

## 10. Report

Return a concise technical summary containing:

### Implemented

What behavior changed.

### Files Changed

Important files and their responsibilities.

### Design

Key implementation decisions and why they were chosen.

### Validation

Tests and checks executed and their results.

### Remaining Risk

Known limitations, assumptions, or unresolved concerns.

Do not claim validation that was not actually performed.
Before editing, determine:

* files that need modification;
* interfaces affected;
* data or state changes;
* validation required;
* compatibility implications;
* expected failure modes;
* tests needed.

Prefer:

* existing abstractions;
* small cohesive changes;
* explicit contracts;
* deterministic behavior where possible;
* reversible changes.

Avoid:

* unrelated refactoring;
* premature abstractions;
* unnecessary dependencies;
* speculative extensibility;
* broad architectural changes without evidence they are required.

## 5. Establish a validation plan

Determine how the implementation will be proven correct.

Use the strongest practical combination of:

* existing tests;
* new focused tests;
* type checking;
* linting;
* static analysis;
* compilation;
* schema validation;
* runtime checks;
* integration tests;
* representative examples;
* AI or ML evaluations when deterministic testing is insufficient.

For probabilistic AI behavior, use deterministic checks before model-based evaluation whenever possible.

## 6. Implement incrementally

Make the smallest coherent change first.

After each meaningful change:

* verify syntax and structure;
* check affected contracts;
* run focused validation when practical;
* inspect unexpected side effects before continuing.

Do not accumulate a large unvalidated patch when smaller checkpoints are possible.

Preserve existing public behavior unless the requirement explicitly changes it.

## 7. Handle errors and edge cases

Consider relevant:

* invalid input;
* missing data;
* empty states;
* timeouts;
* retries;
* partial failures;
* external dependency failures;
* concurrency;
* idempotency;
* backwards compatibility;
* security and permissions.

Only handle edge cases that are plausible for the system.

Do not add speculative complexity for unrealistic scenarios.

## 8. Test the implementation

Run the most focused tests first.

Then, when practical:

* run related test suites;
* run static checks;
* test representative success cases;
* test relevant failure cases;
* verify integrations;
* verify configuration;
* verify backward compatibility.

If tests fail, investigate the cause rather than weakening tests to make them pass.

Do not modify unrelated tests simply because they expose an existing issue.

## 9. Review the final diff

Before considering the implementation complete, inspect all changes.

Check for:

* accidental edits;
* unnecessary files;
* duplicated logic;
* dead code;
* debug output;
* exposed secrets;
* inconsistent naming;
* missing error handling;
* missing tests;
* unintended API changes;
* excessive complexity.

Remove changes that are not necessary for the requirement.

## 10. Report

Return a concise technical summary containing:

### Implemented

What behavior changed.

### Files Changed

Important files and their responsibilities.

### Design

Key implementation decisions and why they were chosen.

### Validation

Tests and checks executed and their results.

### Remaining Risk

Known limitations, assumptions, or unresolved concerns.

Do not claim validation that was not actually performed.
