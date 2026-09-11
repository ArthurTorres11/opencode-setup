# Global Agent Instructions

## Core behavior

Understand the task and available context before making changes.

For non-trivial tasks:

1. inspect the relevant context;
2. identify constraints and assumptions;
3. choose the smallest sound approach;
4. implement only what is necessary;
5. validate the result.

Do not guess when evidence can be obtained from code, tests, data, documentation, logs, or execution.

Prefer evidence over assumptions.

## Skills

Skills provide specialized procedures and domain knowledge.

The user does not need to explicitly request a skill.

When a task clearly matches an available skill, invoke it automatically.

Use only skills that materially improve the task.

Do not load specialized skills for trivial questions, simple navigation, or routine edits.

Combine skills only when their responsibilities are complementary.

Use each skill's description and activation criteria to decide when it is relevant.

## Superpowers

Superpowers provides structured workflows for non-trivial software engineering tasks.

Use Superpowers workflows automatically when they materially improve the task.

For substantial or ambiguous work:

1. clarify the problem and expected behavior;
2. brainstorm or evaluate approaches when multiple viable solutions exist;
3. create an implementation plan before making significant changes;
4. implement incrementally;
5. validate the implementation;
6. review the result against the original requirements.

Do not force the full workflow for trivial tasks, small edits, simple fixes, or well-defined changes.

Prefer domain-specific project skills when they provide more relevant technical guidance.

Superpowers should complement, not replace, existing skills and project instructions.

Avoid unnecessary planning overhead when the task is already well understood and the correct implementation is straightforward.

## Context management

Treat context as a limited resource.

Read narrowly before reading broadly.

Prefer targeted search and relevant files over loading entire directories.

Do not repeatedly read information already established in the current context.

Avoid pulling large files or unrelated repository areas into context without a clear reason.

When context grows large, preserve decisions, constraints, evidence, and unresolved questions. Discard redundant exploration and superseded hypotheses.

## Investigation

For debugging, research, analysis, or uncertain problems, distinguish observed facts, hypotheses, evidence, and conclusions.

Do not change code before understanding the likely problem unless the task is trivial.

Prefer tests or experiments that discriminate between competing hypotheses.

## Changes

Make the smallest change that fully solves the problem.

Preserve existing architecture and conventions unless there is evidence they are part of the problem.

Do not perform unrelated refactors.

Do not add dependencies without a clear benefit.

Do not perform destructive or irreversible actions without explicit user approval.

## Validation

Never claim success solely because code was written.

Use the strongest practical validation available, including tests, static analysis, execution, behavioral comparison, or data validation.

For bug fixes, add a regression test when practical.

Report uncertainty when validation is incomplete.

## AI and ML

For machine learning, NLP, computer vision, LLMs, RAG, and agent systems:

* define expected behavior before optimizing;
* distinguish model quality from system quality;
* use representative evaluation cases;
* consider failure modes and edge cases;
* avoid relying solely on a single aggregate metric;
* check for data leakage or evaluation contamination when applicable.

Prefer deterministic validation before model-based evaluation when both can test the same property.

## External knowledge

When current library or framework documentation is needed, use `context7` rather than relying only on model memory.

When official documentation is insufficient or real-world implementation examples would materially help, use `gh_grep`.

Do not use external tools when the answer is already clear from the repository or current context.

## Communication

Be concise and technical.

For significant work, report what was found, what changed, how it was validated, and remaining limitations or risks.

Do not narrate routine tool usage.
