---
name: agent-engineer
description: >
  Design, implement, debug, evaluate, and improve AI agent systems involving tool
  calling, planning, orchestration, state, memory, workflows, delegation,
  multi-agent coordination, human-in-the-loop, termination conditions, retries,
  fallbacks, and agent reliability. Use when the task involves autonomous or
  semi-autonomous LLM workflows, tool-using agents, agent loops, multi-agent
  systems, agent architecture, or unexpected agent behavior. In this skill,
  "agent" means an LLM-driven system that dynamically decides actions or invokes
  tools at runtime. Do not invoke when "agent" merely appears as a class name,
  filename, variable, identifier, or unrelated software concept.
  When the primary problem is context selection, prompt assembly, context compression, memory retrieval, or token efficiency rather than agent orchestration, prefer context-engineering.
---

# Agent Engineer

Use this skill when designing, implementing, reviewing, debugging, or improving
AI agent systems.

Typical cases include:

* tool-using agents;
* agentic workflows;
* agent orchestration;
* planning and execution loops;
* stateful agents;
* agent memory;
* multi-agent systems;
* delegation;
* routing;
* human-in-the-loop workflows;
* agent reliability;
* termination conditions;
* retries and fallbacks;
* structured agent outputs;
* agent evaluation;
* production agent architectures.

Do not invoke this skill merely because an application uses an LLM.

Use it when agent behavior, orchestration, tools, state, decisions, or autonomy
materially affect the task.

## Main Goal

Build agent systems that are:

* understandable;
* controllable;
* reliable;
* observable;
* testable;
* efficient;
* safe to operate;
* appropriate to the task.

Prefer the simplest architecture that satisfies the required behavior.

Do not introduce agents when a deterministic workflow or single model call would
solve the problem more reliably.

## Start With the Task

Before designing an agent, define:

* objective;
* inputs;
* expected outputs;
* available tools;
* constraints;
* success criteria;
* failure conditions;
* required autonomy;
* human approval requirements.

Ask whether an agent is actually necessary.

Consider simpler alternatives:

1. deterministic code;
2. single LLM call;
3. structured LLM call;
4. deterministic workflow with LLM steps;
5. single tool-using agent;
6. multi-agent system.

Move toward greater autonomy only when the task requires it.

## System Map

For substantial agent systems, map the execution path.

Typical components include:

1. user or system input;
2. preprocessing;
3. context construction;
4. state retrieval;
5. model invocation;
6. reasoning or decision step;
7. tool selection;
8. tool execution;
9. observation;
10. state update;
11. next action;
12. termination;
13. final response.

Additional systems may include:

* retrieval;
* memory;
* routing;
* delegation;
* approval gates;
* background jobs;
* external services;
* evaluators;
* guardrails.

Do not assume every agent needs every component.

## Agent Architecture

Choose architecture based on the task.

Common patterns include:

### Single-Step LLM

Use when one model invocation can reliably solve the task.

Do not build an agent unnecessarily.

### Deterministic Workflow

Use when the sequence of operations is known in advance.

LLMs may be used inside individual steps without controlling the workflow.

### Tool-Using Agent

Use when the model must dynamically decide which tools to call.

### Router

Use when the system must select among known workflows, models, tools, or
specialists.

Prefer deterministic routing when reliable rules are available.

Use model-based routing when semantic interpretation is genuinely required.

### Planner-Executor

Use when the task benefits from separating planning from execution.

Validate whether planning actually improves outcomes before adding the additional
model calls.

### Multi-Agent System

Use when responsibilities are meaningfully separable and specialized agents
provide measurable benefit.

Do not create multiple agents merely to simulate organizational roles.

Multi-agent architectures introduce:

* coordination cost;
* additional latency;
* more model calls;
* larger failure surfaces;
* harder debugging;
* state synchronization problems.

Require a clear benefit.

## Agent Loop

A tool-using agent commonly follows:

```text
input
  ↓
context/state
  ↓
model
  ↓
decision
  ↓
tool call
  ↓
observation
  ↓
state update
  ↓
continue or terminate
```

Every loop should have explicit termination behavior.

Avoid unbounded execution.

Consider:

* maximum iterations;
* maximum tool calls;
* token budget;
* time budget;
* cost budget;
* completion criteria;
* failure criteria.

Do not rely solely on the model deciding that it is finished.

## Tools

Treat tools as contracts between the model and the external world.

A good tool should have:

* a clear name;
* a narrow responsibility;
* an accurate description;
* explicit inputs;
* understandable outputs;
* predictable failure behavior.

Avoid ambiguous tools whose responsibilities overlap heavily.

Bad:

```text
manage_everything()
```

Better:

```text
search_documents()
get_customer()
create_ticket()
update_ticket()
```

Do not expose tools merely because they exist.

Expose only tools relevant to the agent's responsibility.

Use `tool-design` when deeper tool-interface design is required.

## Tool Descriptions

Tool descriptions influence model behavior.

Descriptions should communicate:

* what the tool does;
* when it should be used;
* when it should not be used;
* important constraints.

Avoid descriptions that are too generic.

When tool selection is unreliable, evaluate the descriptions before assuming the
model itself is incapable.

## Tool Inputs

Prefer structured schemas.

Make required and optional fields explicit.

Use meaningful field names.

Avoid schemas containing many loosely related optional fields when several
smaller tools would create clearer contracts.

Validate untrusted tool arguments before performing sensitive actions.

## Tool Outputs

Return information that helps the model decide what to do next.

Prefer:

* structured results;
* explicit status;
* relevant identifiers;
* actionable errors.

Avoid dumping unnecessary payloads into the model context.

Large tool outputs should be filtered, summarized, paginated, or stored
externally when practical.

## Tool Errors

Errors should help the agent decide the next action.

Distinguish:

* retryable errors;
* validation errors;
* permission errors;
* missing resources;
* permanent failures.

Avoid returning raw implementation details when they do not help recovery.

Do not allow repeated retries without a stopping condition.

## State

Define what the agent needs to remember during execution.

State may include:

* current objective;
* completed steps;
* intermediate results;
* tool outputs;
* decisions;
* unresolved questions;
* approvals;
* errors;
* execution metadata.

Keep state minimal.

Do not store information merely because it may possibly be useful later.

Separate durable state from temporary execution state.

## Memory

Do not confuse agent state with long-term memory.

### Execution State

Exists for the current workflow or run.

### Conversation Memory

Preserves relevant information across conversational turns.

### Long-Term Memory

Persists information across sessions or tasks.

Long-term memory should have explicit:

* write criteria;
* retrieval criteria;
* update behavior;
* deletion behavior;
* scope.

Do not automatically store every interaction.

Use specialized memory or context-engineering guidance when memory architecture
becomes a central concern.

## Context Management

Context is a limited resource.

Provide the agent with the information needed for the current decision.

Avoid repeatedly injecting:

* full conversation history;
* entire documents;
* irrelevant tool outputs;
* duplicate instructions;
* obsolete intermediate reasoning.

Prefer progressive disclosure.

Load specialized information when it becomes necessary.

Use `context-engineering` when context architecture, compression, degradation, or
retrieval becomes a central concern.

## Planning

Planning is useful when tasks require multiple dependent decisions.

Plans should remain adaptable.

Do not force detailed plans for simple tasks.

When using a planner:

* define the objective;
* preserve constraints;
* make dependencies explicit;
* allow replanning when evidence changes.

Do not blindly execute a stale plan when observations contradict it.

## Routing

Routing should send work to the smallest capable component.

Possible routing signals include:

* deterministic rules;
* metadata;
* task type;
* semantic classification;
* model judgment.

Prefer cheap and deterministic routing when it performs adequately.

Evaluate routers using representative cases.

Do not assume routing accuracy from a few successful examples.

## Delegation

When one agent delegates work to another, make the contract explicit.

Specify:

* task;
* relevant context;
* constraints;
* expected output;
* completion criteria.

Avoid passing the entire parent context when the child needs only a small subset.

Delegated agents should receive enough context to work independently without
receiving irrelevant history.

## Multi-Agent Systems

Use multiple agents only when specialization or parallelism provides measurable
value.

Define:

* agent responsibilities;
* communication contracts;
* shared state;
* ownership;
* delegation rules;
* termination;
* conflict resolution.

Avoid circular delegation.

Avoid agents repeatedly asking each other to solve the same task.

Ensure that one component ultimately owns completion.

## Human-in-the-Loop

Use human approval when actions are:

* destructive;
* irreversible;
* financially meaningful;
* externally visible;
* security-sensitive;
* legally sensitive;
* high impact.

Separate:

```text
propose
   ↓
approve
   ↓
execute
```

Do not treat human approval as implicit.

The system should preserve enough context for the human to understand what is
being approved.

## Deterministic vs Model Decisions

Do not use an LLM for decisions that deterministic logic can reliably perform.

Prefer deterministic checks for:

* schema validation;
* permissions;
* numeric constraints;
* existence checks;
* formatting;
* known business rules;
* exact comparisons.

Use model judgment where semantic interpretation is necessary.

This reduces:

* latency;
* cost;
* nondeterminism;
* failure surface.

## Structured Outputs

Prefer structured outputs when downstream code depends on model results.

Validate model outputs before using them in critical operations.

Handle:

* missing fields;
* invalid enum values;
* malformed output;
* schema violations;
* unexpected nulls.

Do not assume structured generation eliminates the need for validation.

## Retries

Retries should have a reason.

Consider retries for:

* transient network failures;
* rate limits;
* temporary provider failures.

Do not blindly retry:

* invalid requests;
* permission errors;
* deterministic validation failures;
* repeated agent mistakes.

Use bounded retries.

Consider backoff when appropriate.

## Fallbacks

Fallbacks should preserve useful behavior when the preferred path fails.

Possible fallbacks include:

* alternative model;
* simpler workflow;
* deterministic response;
* cached result;
* human escalation;
* safe failure.

Do not hide persistent system failures behind unlimited fallback chains.

Track when fallback behavior occurs.

## Termination

Every autonomous loop needs termination conditions.

Possible conditions include:

* objective completed;
* required output produced;
* no valid next action;
* iteration limit reached;
* tool budget reached;
* time budget reached;
* cost budget reached;
* unrecoverable failure;
* human intervention required.

Termination behavior should be testable.

## Reliability

Agent reliability depends on the complete system, not only the model.

Evaluate:

* prompts;
* context;
* tools;
* schemas;
* routing;
* state;
* memory;
* orchestration;
* model behavior;
* error handling;
* termination.

Do not attribute every failure to the LLM.

## Evaluation

Define expected behavior before optimizing the agent.

Create representative evaluation cases.

Depending on the system, evaluate:

* task completion;
* answer correctness;
* tool selection;
* tool argument correctness;
* number of tool calls;
* unnecessary tool calls;
* trajectory quality;
* constraint compliance;
* latency;
* token usage;
* cost;
* failure recovery;
* termination behavior.

Separate deterministic assertions from model-based evaluation.

Use `llm-evaluation` when designing deeper agent evaluation systems.

## Trajectory Evaluation

Final-answer quality alone may hide agent failures.

When useful, inspect the trajectory:

```text
input
→ decision
→ tool
→ observation
→ decision
→ tool
→ final result
```

Evaluate whether:

* the correct tools were selected;
* calls occurred in a valid order;
* unnecessary calls were avoided;
* tool outputs were used correctly;
* the agent recovered from errors;
* termination occurred correctly.

Do not require an exact trajectory when multiple valid paths exist unless the
workflow requires one.

## Observability

For production agent systems, capture enough information to understand failures.

Useful signals may include:

* run identifier;
* model;
* latency;
* token usage;
* cost;
* tool calls;
* tool latency;
* tool errors;
* state transitions;
* routing decisions;
* termination reason;
* fallback usage.

Avoid logging secrets or unnecessarily sensitive content.

Observability should support debugging and evaluation.

## Performance and Cost

Agent systems can multiply model and tool calls quickly.

Measure:

* model calls per task;
* tool calls per task;
* tokens per task;
* latency per step;
* total latency;
* cost per successful task.

Common optimization opportunities include:

* removing unnecessary agent steps;
* reducing repeated context;
* using deterministic routing;
* using smaller models for simpler decisions;
* parallelizing independent operations;
* caching safe repeated work;
* reducing unnecessary tool exposure.

Optimize cost per successful outcome rather than cost per individual model call.

## Security

Treat tool-using agents as systems capable of performing actions.

Consider:

* prompt injection;
* untrusted tool output;
* permission boundaries;
* credential exposure;
* data exfiltration;
* unsafe tool arguments;
* excessive tool permissions;
* destructive operations.

Apply least privilege.

Do not allow model instructions to override system-level authorization.

Treat external retrieved content as untrusted input.

## Prompt Injection

Agents consuming external content should distinguish data from instructions.

Retrieved documents, websites, emails, files, and tool outputs may contain
malicious instructions.

Do not allow untrusted content to redefine:

* system behavior;
* permissions;
* tool policies;
* approval requirements;
* security constraints.

Validate sensitive actions independently from model-generated reasoning.

## Debugging Agents

When an agent behaves incorrectly, separate the layers.

Check:

1. input;
2. system instructions;
3. context;
4. model;
5. routing;
6. tool definitions;
7. tool arguments;
8. tool implementation;
9. observations;
10. state;
11. memory;
12. termination;
13. final generation.

Identify the first point where actual behavior diverges from expected behavior.

Use `debugging` for systematic root-cause investigation.

Do not change prompts blindly before identifying the failing layer.

## RAG and Agents

When an agent uses retrieval, treat retrieval as a separate subsystem.

Separate:

```text
agent decision
      ↓
retrieval request
      ↓
retrieval pipeline
      ↓
context
      ↓
agent decision
```

Use `rag-engineer` when retrieval quality or RAG architecture is the primary
problem.

Do not compensate for poor retrieval solely through prompt engineering.

## Frameworks

Agent frameworks may include technologies such as:

* LangGraph;
* LangChain;
* OpenAI Agents SDK;
* PydanticAI;
* Semantic Kernel;
* AutoGen;
* custom orchestration.

Do not choose architecture based solely on framework popularity.

Understand the problem before choosing the framework.

Follow the version actually installed by the project.

Use `context7` when current framework documentation is required.

## Framework Independence

Reason about the underlying system before framework-specific APIs.

Concepts such as:

* state;
* tools;
* routing;
* loops;
* delegation;
* termination;
* memory;
* evaluation;

exist independently of any framework.

Prefer architectures that remain understandable even when framework abstractions
are removed.

## Testing

Test deterministic agent components normally.

Examples:

* routing rules;
* schemas;
* state transitions;
* tool implementations;
* permission checks;
* termination conditions.

For probabilistic behavior, use representative evaluation cases.

Do not rely exclusively on unit tests for model behavior.

Do not rely exclusively on LLM-as-judge for deterministic properties.

## When Other Skills Should Be Combined

Combine this skill with specialized skills when necessary.

Examples:

* `agent-engineer` + `debugging` for unexplained agent failures;
* `agent-engineer` + `rag-engineer` for retrieval-enabled agents;
* `agent-engineer` + `context-engineering` for complex context or memory systems;
* `agent-engineer` + `llm-evaluation` for agent benchmarks and evaluation;
* `agent-engineer` + `tool-design` for complex tool interfaces;
* `agent-engineer` + `python-engineer` for Python implementation;
* `agent-engineer` + `ml-engineer` when agents interact with ML systems.

Do not invoke additional skills without a clear need.

## Required Output

For substantial agent work, report:

### Architecture

Relevant components and execution flow.

### Findings

Important design issues, failure modes, or constraints.

### Changes

What was implemented or should change.

### Evaluation

How expected behavior was or should be validated.

### Risks

Remaining reliability, cost, security, or operational concerns.

For simple tasks, keep the output proportional to the request.

## Rules

* Do not use an agent when a simpler workflow is sufficient.
* Prefer deterministic logic for deterministic problems.
* Keep tool interfaces clear and narrow.
* Keep agent state minimal.
* Bound autonomous loops.
* Define explicit termination behavior.
* Treat external content as untrusted.
* Require explicit approval for sensitive actions.
* Separate retrieval failures from generation failures.
* Separate model failures from orchestration failures.
* Evaluate the complete agent system, not only the final answer.
* Measure latency, tokens, and cost when operational efficiency matters.
* Avoid multi-agent complexity without measurable benefit.
* Validate significant changes before claiming success.
* Report uncertainty and incomplete validation explicitly.
