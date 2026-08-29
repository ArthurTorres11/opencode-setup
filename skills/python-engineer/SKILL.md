---
name: python-engineer
description: >
  Design, implement, review, debug, and improve production-quality Python software. Use when the task involves Python architecture, APIs, services, packages, scripts, data processing, async code, typing, testing, performance, dependency management, error handling, maintainability, or Python best practices.
  Do not invoke when the task is primarily about ML modeling, feature engineering, agent design, RAG implementation, mathematical optimization, or root-cause investigation where Python is merely the implementation language; prefer the specialized skill.
---

# Python Engineer

Use this skill when Python implementation quality materially affects the task.

Typical cases include:

* implementing Python features;
* reviewing Python code;
* designing modules or packages;
* building APIs or services;
* writing data-processing code;
* improving maintainability;
* adding or improving tests;
* debugging Python-specific problems;
* improving performance;
* working with async code;
* defining typed interfaces;
* refactoring poorly structured Python;
* packaging or dependency-management issues.

Do not invoke this skill for trivial syntax questions or simple edits that do not
require specialized Python engineering guidance.

## Main Goal

Produce Python code that is:

* correct;
* readable;
* maintainable;
* testable;
* appropriately typed;
* efficient enough for its requirements;
* consistent with the existing codebase.

Prefer simple and explicit solutions over unnecessary abstraction.

## Repository First

Before implementing substantial changes, inspect the repository to understand:

* project structure;
* Python version;
* dependency manager;
* existing architecture;
* coding conventions;
* testing framework;
* linting and formatting tools;
* typing configuration;
* relevant abstractions;
* existing utilities.

Do not impose a new project structure when the existing one is reasonable.

Prefer existing project conventions over personal preferences.

## Implementation Principles

### Correctness First

Prioritize:

1. correctness;
2. clarity;
3. maintainability;
4. performance.

Do not optimize code before establishing that it is correct unless performance is
the problem being solved.

### Keep Changes Focused

Make the smallest change that fully solves the task.

Avoid:

* unrelated refactors;
* speculative abstractions;
* unnecessary dependencies;
* broad formatting changes;
* rewriting working code without evidence of benefit.

### Prefer Explicit Code

Prefer code whose behavior is easy to understand.

Avoid cleverness that reduces readability.

Use abstractions when they remove meaningful duplication or establish useful
boundaries, not merely to reduce line count.

## Architecture

When designing Python components, consider:

* responsibilities;
* module boundaries;
* dependencies;
* interfaces;
* state ownership;
* side effects;
* error boundaries;
* testability.

Separate concerns when it materially improves the design.

Typical boundaries may include:

* domain logic;
* infrastructure;
* external APIs;
* persistence;
* model providers;
* data processing;
* orchestration;
* configuration.

Do not create architectural layers that the project does not need.

## Functions

Prefer functions with:

* a clear responsibility;
* explicit inputs;
* predictable outputs;
* limited side effects;
* meaningful names.

Avoid functions that:

* modify unrelated global state;
* perform many unrelated operations;
* depend on hidden assumptions;
* mix business logic with infrastructure unnecessarily.

Extract functions when doing so creates a meaningful reusable or testable
boundary.

Do not split code into tiny functions without a clear benefit.

## Classes

Use classes when they provide a meaningful representation of:

* state;
* behavior;
* lifecycle;
* interfaces;
* domain concepts.

Do not introduce classes merely to group unrelated functions.

Prefer composition over deep inheritance hierarchies.

Keep object state explicit and understandable.

## Typing

Use type hints when they improve:

* interface clarity;
* static analysis;
* maintainability;
* IDE support;
* correctness.

Prefer precise types over `Any` when practical.

Use modern Python typing appropriate to the project's supported Python version.

Do not introduce advanced type machinery when a simpler annotation communicates
the same intent.

For external or untrusted data, distinguish static typing from runtime
validation.

## Data Models and Validation

When structured runtime validation is required, use the project's existing
validation approach.

For libraries such as Pydantic:

* follow the installed version;
* verify current APIs when necessary;
* define clear schemas;
* validate data at appropriate boundaries;
* avoid unnecessary repeated validation.

Use `context7` when current library documentation is needed.

Do not assume APIs from older library versions.

## Error Handling

Handle errors at the boundary where meaningful recovery or context can be added.

Prefer specific exceptions over broad exception handling.

Avoid:

```python
try:
    ...
except Exception:
    pass
```

unless there is a documented and justified reason.

When catching an exception:

* preserve useful diagnostic information;
* add context when necessary;
* recover only when recovery is meaningful;
* otherwise propagate appropriately.

Do not expose secrets or sensitive implementation details in user-facing errors.

## Logging and Observability

Use logging when operational visibility is required.

Prefer structured, actionable logs.

Useful logs may include:

* operation;
* relevant identifiers;
* duration;
* outcome;
* error category.

Avoid:

* excessive logging;
* logging secrets;
* logging entire sensitive payloads;
* duplicate logs at multiple layers.

Distinguish logs intended for developers from errors intended for end users.

## Configuration

Keep environment-specific configuration outside business logic.

Use the project's established configuration mechanism.

Do not hardcode:

* credentials;
* tokens;
* environment-specific URLs;
* secrets;
* machine-specific paths.

Validate required configuration early when practical.

## Async and Concurrency

Use asynchronous code when the workload benefits from concurrent I/O.

Do not use `async` merely because a framework supports it.

Avoid blocking operations inside asynchronous execution paths.

When working with concurrency, consider:

* shared mutable state;
* race conditions;
* cancellation;
* timeouts;
* retries;
* resource limits;
* exception propagation.

Prefer bounded concurrency when interacting with rate-limited or expensive
services.

## APIs and Services

When working with FastAPI, Flask, Django, or similar frameworks:

* respect existing architecture;
* keep transport logic separate from core logic when useful;
* validate external inputs;
* return appropriate errors;
* avoid unnecessary work during requests;
* manage external resources correctly;
* consider timeouts for remote dependencies.

For AI APIs, also consider:

* model latency;
* rate limits;
* retries;
* token usage;
* streaming;
* structured outputs;
* provider failures.

Do not tightly couple core application logic to a provider SDK when a simple
boundary would materially improve maintainability or testability.

## External Integrations

For databases, model providers, vector stores, cloud services, or external APIs:

* define clear boundaries;
* handle timeouts;
* handle expected failures;
* avoid unnecessary repeated calls;
* validate responses when needed;
* make expensive dependencies replaceable in tests when practical.

Do not invent retry behavior blindly.

Consider whether an operation is safe to retry before adding retries.

## Data Processing

When using Pandas, Polars, NumPy, or similar libraries, check:

* data types;
* null behavior;
* index assumptions;
* alignment;
* joins;
* duplicate keys;
* sorting assumptions;
* copies versus mutation;
* memory usage;
* unnecessary loops.

Prefer vectorized or library-native operations when they improve clarity and
performance.

Do not sacrifice correctness for vectorization.

For large datasets, consider whether the chosen in-memory approach is
appropriate.

## Performance

Measure before optimizing when practical.

Identify the actual bottleneck.

Common Python bottlenecks include:

* unnecessary repeated computation;
* excessive serialization;
* repeated network calls;
* inefficient loops;
* unnecessary copies;
* repeated model inference;
* inefficient database access;
* excessive object creation;
* blocking I/O.

Prefer algorithmic improvements over micro-optimizations.

Do not introduce caching without considering:

* invalidation;
* memory;
* consistency;
* lifetime;
* concurrency.

## Dependencies

Before adding a dependency, determine whether:

* the project already has an appropriate dependency;
* the standard library is sufficient;
* the new dependency materially simplifies the implementation;
* maintenance cost is justified.

Follow the project's existing dependency manager.

Examples may include:

* `uv`;
* `pip`;
* `poetry`;
* `pip-tools`.

Do not assume which one is used.

## Testing

Use the project's existing testing framework.

For Python projects this will often be `pytest`, but inspect the repository
before assuming.

Test behavior rather than implementation details.

Prioritize tests for:

* important business logic;
* bug regressions;
* edge cases;
* failure behavior;
* external boundaries;
* transformations;
* parsing and validation.

Mock external systems when isolation is useful.

Do not mock the behavior actually being tested.

For bug fixes, add a regression test when practical.

## Test Quality

A useful test should fail when the behavior is wrong and pass when it is correct.

Avoid tests that:

* merely execute code without meaningful assertions;
* reproduce implementation details;
* rely unnecessarily on external services;
* depend on execution order;
* are nondeterministic without reason.

Use representative fixtures.

Keep test setup proportional to the behavior being tested.

## AI and ML Python Systems

When implementing AI or ML applications, distinguish:

* model logic;
* application logic;
* orchestration;
* external providers;
* data processing;
* evaluation;
* persistence.

Avoid embedding large prompts, schemas, provider-specific behavior, and
application logic into a single function.

For LLM and agent applications, consider:

* structured outputs;
* tool schemas;
* context size;
* model configuration;
* state management;
* retries;
* timeouts;
* fallback behavior;
* evaluation hooks;
* observability.

Use specialized skills such as `agent-engineer`, `rag-engineer`, or
`ml-engineer` when domain-specific reasoning is required.

## Security

Never hardcode or expose secrets.

Treat external input as untrusted.

Consider:

* input validation;
* path traversal;
* injection;
* unsafe deserialization;
* command execution;
* dependency risks;
* authorization boundaries.

Do not weaken security controls merely to make an implementation easier.

## Refactoring

Refactor when it directly supports the requested change or resolves a demonstrated
maintainability problem.

Before refactoring:

1. understand existing behavior;
2. identify the concrete problem;
3. preserve observable behavior unless change is intentional;
4. ensure validation exists.

Prefer incremental refactors over large rewrites.

## Code Review

When reviewing Python code, prioritize findings by impact.

Focus on:

1. correctness;
2. security;
3. data loss or corruption;
4. concurrency;
5. error handling;
6. performance;
7. maintainability;
8. style.

Do not flood the review with low-value style comments when more important issues
exist.

Explain why a finding matters.

## When Other Skills Should Be Combined

Combine this skill with specialized skills only when useful.

Examples:

* `python-engineer` + `debugging` for difficult Python failures;
* `python-engineer` + `agent-engineer` for Python agent applications;
* `python-engineer` + `rag-engineer` for RAG implementations;
* `python-engineer` + `ml-engineer` for ML systems;
* `python-engineer` + `model-evaluator` for evaluation pipelines;
* `python-engineer` + `optimization-engineer` for optimization software.

This skill owns Python engineering concerns.

The specialized skill owns domain-specific methodology.

## Validation

Never claim implementation success solely because code was written.

Use the strongest practical validation available:

* existing tests;
* targeted tests;
* type checking;
* linting;
* static analysis;
* execution;
* integration tests;
* behavioral comparison.

Use the tools already configured by the project when possible.

If full validation cannot be performed, state what was and was not validated.

## Required Output

For substantial work, report:

### Changes

What was implemented or modified.

### Rationale

Important engineering decisions when they are not obvious.

### Validation

What was executed or checked.

### Remaining Issues

Relevant limitations, risks, or follow-up work.

For simple tasks, keep the response concise rather than forcing this full
structure.

## Rules

* Prefer correctness over cleverness.
* Preserve existing project conventions when reasonable.
* Keep changes focused.
* Avoid unnecessary abstractions.
* Avoid unrelated refactors.
* Do not add dependencies without justification.
* Do not assume library APIs when current documentation is available.
* Validate significant changes.
* Report incomplete validation explicitly.
* Keep implementation complexity proportional to the problem.
