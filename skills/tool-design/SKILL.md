---
name: tool-design
description: >
  Design, review, debug, and optimize tools exposed to LLMs and AI agents. Use
  when the task involves function calling, MCP tools, tool schemas, tool
  descriptions, tool selection, tool arguments, tool outputs, tool errors, tool
  permissions, overlapping tools, excessive tool surfaces, or improving how
  reliably and efficiently an agent interacts with external capabilities. In
  this skill, "tool" means a function or capability explicitly exposed to an
  LLM or agent through function calling, MCP, or a similar interface. Do not
  invoke for generic developer tools, build tools, CLI utilities, helper
  functions, or unrelated uses of "tool" in application code.
  For broader agent architecture, orchestration, planning, state, or multi-agent coordination where tool interfaces are not the primary problem, prefer agent-engineer.
---

# Tool Design

Use this skill when the design of tools exposed to an LLM or AI agent materially
affects system behavior.

Typical cases include:

* function calling;
* agent tools;
* MCP tools;
* tool schemas;
* tool descriptions;
* tool selection failures;
* incorrect tool arguments;
* overlapping tools;
* excessive tool surfaces;
* unclear tool outputs;
* tool error handling;
* permission boundaries;
* tool routing;
* tool consolidation;
* token-heavy tool definitions.

Do not invoke this skill merely because application code contains functions or
APIs.

Use it when those capabilities are exposed to a model or agent as tools.

## Main Goal

Design tool interfaces that make the correct action easy for the model to select
and execute.

A good tool should be:

* understandable;
* specific;
* predictable;
* composable;
* safe;
* efficient;
* easy to validate.

Tool design is part of agent behavior.

Poor tool interfaces can cause failures even when the underlying model and tool
implementation are correct.

## Start With Capabilities

Before designing tools, determine what capabilities the agent actually needs.

Ask:

* What actions must the agent perform?
* What information must it retrieve?
* Which operations change external state?
* Which operations are sensitive?
* Which operations can fail?
* Which capabilities overlap?
* Which operations should remain deterministic application logic?

Do not expose every backend function as an agent tool.

Expose capabilities based on agent needs, not internal implementation structure.

## Tool Contract

Treat every tool as a contract between the model and the external system.

A tool contract includes:

```text
name
description
input schema
output contract
error behavior
permissions
side effects
```

All of these influence agent reliability.

## Tool Names

Tool names should communicate the action clearly.

Prefer verb-oriented names when the tool performs an action.

Good:

```text
search_documents
get_customer
create_ticket
update_ticket
list_orders
```

Avoid ambiguous names such as:

```text
process
execute
handle
manager
utility
run
```

when the actual behavior cannot be inferred.

Names should distinguish tools from one another.

## Tool Descriptions

Descriptions are routing instructions for the model.

A useful description should explain:

* what the tool does;
* when to use it;
* when not to use it;
* important constraints;
* meaningful differences from similar tools.

Example:

```text
Search internal product documentation using semantic retrieval.
Use when the user asks about documented product behavior or configuration.
Do not use for customer account information.
```

This is better than:

```text
Search documentation.
```

Descriptions should help the model make decisions.

Do not write descriptions as marketing copy.

## Trigger Language

When multiple tools are available, descriptions should contain enough semantic
information for the model to distinguish their responsibilities.

If tool selection is unreliable, inspect:

1. tool name;
2. description;
3. overlapping responsibilities;
4. schema complexity;
5. available alternatives;

before assuming the model needs a larger prompt.

## Tool Granularity

Tools should represent meaningful capabilities.

Avoid tools that are too broad.

Example:

```text
manage_customer()
```

may hide many unrelated operations.

But also avoid unnecessary fragmentation such as:

```text
get_customer_name()
get_customer_email()
get_customer_phone()
get_customer_address()
```

when:

```text
get_customer()
```

provides a coherent resource.

Choose granularity based on agent decisions, not arbitrary function boundaries.

## Overlapping Tools

Heavy overlap makes tool selection harder.

Example:

```text
search_documents
find_documents
query_documents
retrieve_documents
```

may represent an unnecessarily ambiguous surface.

When tools substantially overlap, consider:

* clearer responsibility boundaries;
* consolidation;
* deterministic routing;
* removing unnecessary tools.

Do not rely solely on descriptions to compensate for fundamentally overlapping
interfaces.

## Tool Consolidation

Consolidation can reduce:

* tool-selection ambiguity;
* context usage;
* maintenance;
* duplicated logic.

But consolidation should not create one giant tool with many unrelated modes.

Good consolidation groups operations that belong to one coherent resource or
capability.

## Input Schemas

Tool inputs should be explicit and easy for the model to construct.

Prefer:

* meaningful field names;
* precise types;
* required fields when genuinely required;
* constrained enums when the domain is closed;
* clear descriptions.

Avoid large schemas containing many unrelated optional parameters.

## Required vs Optional Inputs

Make a field required when the tool cannot reliably execute without it.

Do not mark fields optional merely to make calls easier.

Excessive optionality creates ambiguous behavior.

When sensible defaults exist, document them.

## Enums

Use enums when valid values come from a known closed set.

Example:

```text
status:
- open
- closed
- pending
```

Do not use an enum when the domain is naturally open-ended.

Enum values should be understandable without hidden translation.

## Tool Arguments

Validate tool arguments before performing operations.

Check when relevant:

* types;
* ranges;
* formats;
* identifiers;
* permissions;
* business constraints.

Do not assume structured generation guarantees valid semantic arguments.

A syntactically valid tool call can still request an invalid operation.

## Context-Dependent Arguments

Avoid requiring the model to provide information the application already knows.

For example, if authenticated user identity exists outside the model, avoid asking
the model to generate the user identifier unless there is a specific reason.

Inject deterministic context at the application boundary when appropriate.

This reduces hallucination and unnecessary tokens.

## Tool Outputs

Return information useful for the agent's next decision.

Prefer outputs that clearly communicate:

* status;
* result;
* identifiers;
* relevant data;
* next-action information.

Avoid returning large internal objects merely because they are available.

## Structured Outputs

Prefer structured outputs when the agent needs to reason over specific fields.

Example:

```json
{
  "status": "success",
  "ticket_id": "T-123",
  "priority": "high"
}
```

is often easier to use reliably than an unstructured paragraph.

Do not over-structure simple outputs unnecessarily.

## Output Size

Tool outputs consume context.

Large outputs can degrade agent behavior.

For large result sets, consider:

* filtering;
* pagination;
* limiting;
* targeted fields;
* summaries;
* external artifact references;
* retrieval-on-demand.

Do not return thousands of records to the model when it needs three.

Use `context-engineering` when tool outputs create broader context-management
problems.

## Tool Errors

Errors are part of the tool contract.

An error should help the agent decide what to do next.

Distinguish when useful:

### Validation Error

The request itself is invalid.

### Not Found

The requested resource does not exist.

### Permission Error

The operation is not authorized.

### Conflict

The requested change conflicts with current state.

### Rate Limit

Execution should be delayed or retried appropriately.

### Temporary Failure

The dependency may recover.

### Permanent Failure

Repeating the same call is unlikely to help.

Avoid returning only:

```text
Something went wrong.
```

when the agent needs actionable information.

## Actionable Errors

Good errors may include:

* stable error category;
* concise explanation;
* whether retry is appropriate;
* which field is invalid;
* what information is missing.

Do not expose sensitive internal implementation details.

## Retry Semantics

Tool design should make retry behavior understandable.

For each mutating operation, consider whether retry is safe.

A retryable read operation is different from a potentially duplicated write.

For important mutations, consider idempotency.

Do not allow the agent to blindly retry destructive or non-idempotent operations.

## Side Effects

Clearly distinguish tools that:

* only read;
* create;
* update;
* delete;
* send;
* publish;
* execute.

The model should not have to infer whether a tool has external side effects.

Sensitive side effects should require appropriate authorization or approval.

## Read vs Write Tools

Separating read and write capabilities often improves safety and clarity.

Example:

```text
get_invoice
update_invoice
```

is clearer than a single tool whose behavior changes based on optional arguments.

However, use judgment.

Do not fragment coherent operations without benefit.

## Permissions

Apply least privilege.

An agent should receive only the tools and permissions required for its task.

Do not expose:

* administrative capabilities;
* destructive operations;
* unrelated systems;
* broad credentials;

without a clear need.

Tool availability is itself a security boundary.

## Human Approval

For high-impact actions, separate proposing from executing.

Example:

```text
agent proposes action
        ↓
human reviews
        ↓
authorized tool executes
```

Do not use prompt instructions as the only protection around sensitive actions.

Authorization should be enforced outside the model.

## Untrusted Inputs

Tool arguments may originate from:

* user input;
* retrieved documents;
* websites;
* emails;
* model reasoning.

Treat them as untrusted until validated.

Consider:

* injection;
* command execution;
* path traversal;
* SQL injection;
* unsafe URLs;
* data exfiltration;
* malformed identifiers.

Do not assume the LLM sanitizes inputs.

## Untrusted Tool Outputs

Tool outputs can also contain malicious or misleading content.

Examples include:

* web pages;
* retrieved documents;
* emails;
* external API content.

Treat external content as data, not system instructions.

Do not allow tool output to redefine permissions or system policies.

## MCP Tool Design

For MCP servers, the same principles apply.

Inspect:

* number of exposed tools;
* descriptions;
* schema size;
* overlapping capabilities;
* output size;
* permission scope.

Do not expose an entire external platform when the agent requires only a few
capabilities.

Large MCP surfaces can increase:

* context usage;
* routing ambiguity;
* latency;
* security exposure.

## Tool Surface Size

More tools are not automatically better.

As the number of tools grows, the agent must distinguish among more possible
actions.

Measure whether additional tools improve actual task completion.

Consider dynamically exposing subsets of tools based on:

* task;
* workflow stage;
* permissions;
* routing.

Do not optimize solely for the maximum number of available integrations.

## Tool Routing

When many tools exist, routing can reduce the active tool surface.

Possible strategies include:

* deterministic routing;
* domain routing;
* semantic routing;
* staged tool exposure.

Prefer deterministic routing when reliable rules exist.

Evaluate model-based routers before depending on them.

## Tool Discovery

For very large capability spaces, consider discovery rather than exposing every
tool simultaneously.

Example:

```text
task
 ↓
discover relevant capability
 ↓
load relevant tools
 ↓
execute
```

This can reduce context usage and selection ambiguity.

Do not add discovery layers for small tool sets.

## Tool Composition

Prefer small coherent tools that can be combined when necessary.

But consider whether a sequence of tools creates unnecessary model decisions.

If an operation is always deterministic:

```text
A → B → C
```

consider whether application code should perform the sequence rather than asking
the model to independently select all three steps.

Use models for decisions that require semantic judgment.

Use code for deterministic orchestration.

## Tool vs Workflow

A tool represents a capability.

A workflow represents a sequence or policy.

Do not encode complex orchestration accidentally inside vague tool interfaces.

Similarly, do not force the model to orchestrate steps that are always known in
advance.

## Tool vs Agent

Not every capability requires another agent.

Prefer a normal tool when the operation has a clear deterministic interface.

Use a sub-agent when the delegated task genuinely requires independent semantic
reasoning, context, or multi-step decisions.

Agents are more expensive and less predictable than deterministic tools.

## Tool Documentation

Tool documentation should be written for the model that selects the tool, not
only for the developer implementing it.

Keep descriptions:

* precise;
* concise;
* discriminative;
* operational.

Examples can help when the decision boundary is difficult.

Do not add examples merely to make descriptions longer.

## Tool Testing

Test deterministic tool behavior normally.

Validate:

* input parsing;
* schema validation;
* success behavior;
* error behavior;
* permission checks;
* side effects;
* idempotency where relevant.

Do not rely on an LLM evaluation to verify deterministic tool implementation.

## Tool Selection Evaluation

Separately evaluate whether the model selects tools correctly.

Create representative cases containing:

* tool-required tasks;
* no-tool tasks;
* ambiguous tasks;
* similar-tool cases;
* invalid requests.

Measure when useful:

* correct tool;
* unnecessary tool usage;
* missed tool usage;
* argument correctness.

Use `llm-evaluation` for deeper evaluation design.

## Tool Description Experiments

When improving descriptions, compare alternatives using the same evaluation
dataset.

Example:

```text
description A
vs
description B
```

Measure tool-selection accuracy and unnecessary calls.

Do not choose descriptions based only on intuition.

## Tool Observability

For production systems, capture enough information to understand tool behavior.

Useful signals may include:

* tool name;
* call count;
* latency;
* success;
* error category;
* retries;
* argument-validation failures.

Be careful when logging arguments or outputs that may contain sensitive data.

## Performance

Tool architecture affects total system latency.

Measure:

* tool-selection latency;
* execution latency;
* repeated calls;
* sequential dependencies;
* payload size.

Parallelize independent read operations when safe and useful.

Do not parallelize operations with ordering dependencies merely to reduce latency.

## Cost

Tool calls may indirectly increase LLM cost through:

* larger tool definitions;
* large outputs;
* repeated reasoning;
* retry loops;
* extra orchestration steps.

Optimize the complete task rather than individual tool calls.

A slightly more capable deterministic tool may reduce several model decisions.

## Debugging Tool Use

When an agent uses the wrong tool, determine where the failure occurred.

Inspect:

```text
user intent
   ↓
available tools
   ↓
tool descriptions
   ↓
model decision
   ↓
arguments
   ↓
tool execution
   ↓
output
   ↓
agent interpretation
```

Possible failure categories include:

* missing capability;
* wrong tool selection;
* ambiguous description;
* overlapping tools;
* invalid arguments;
* implementation bug;
* misleading output;
* permission failure;
* agent misinterpretation.

Do not rewrite prompts before identifying the failing layer.

Use `debugging` when systematic investigation is required.

## When Other Skills Should Be Combined

Examples:

* `tool-design` + `agent-engineer` for agent tool architecture;
* `tool-design` + `context-engineering` for large tool surfaces;
* `tool-design` + `llm-evaluation` for tool-selection benchmarks;
* `tool-design` + `debugging` for unreliable tool use;
* `tool-design` + `python-engineer` for tool implementation;
* `tool-design` + `rag-engineer` for retrieval tools.

Do not invoke additional skills unless they materially improve the task.

## Required Output

For substantial tool-design work, report:

### Capability

What the agent needs to accomplish.

### Tool Surface

Relevant tools and responsibility boundaries.

### Findings

Ambiguity, overlap, schema, output, error, permission, or efficiency problems.

### Design

Recommended or implemented tool contracts.

### Validation

How deterministic behavior and model tool selection were or should be tested.

### Risks

Remaining reliability, security, latency, or cost concerns.

For simple tasks, keep output proportional to the request.

## Rules

* Design tools around agent capabilities, not backend functions.
* Use clear and discriminative names.
* Treat descriptions as routing instructions.
* Avoid heavily overlapping tools.
* Keep schemas explicit and focused.
* Do not ask the model for information the application already knows.
* Keep tool outputs relevant and appropriately sized.
* Make errors actionable.
* Make side effects explicit.
* Apply least privilege.
* Enforce authorization outside the model.
* Treat tool inputs and external outputs as untrusted.
* Prefer deterministic workflows for deterministic sequences.
* Evaluate tool selection separately from tool implementation.
* Keep the active tool surface proportional to the task.
* Optimize reliability before maximizing capability.
