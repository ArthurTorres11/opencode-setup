---

## description: Configure OpenCode for the current repository by adding only project-specific instructions, integrations, MCP servers, skills, and workflows that are not already covered globally

Configure OpenCode for the current repository:

$ARGUMENTS

## Objective

Prepare the current repository for effective OpenCode usage while preserving and reusing the existing global OpenCode configuration.

The project configuration must contain only the project-specific delta.

Never duplicate, overwrite, copy, or recreate global skills, commands, or global instructions unless explicitly requested.

Prefer the smallest configuration that materially improves work on this repository.

---

# 1. Inspect the existing OpenCode environment

Before recommending or creating anything, inspect the existing configuration when accessible.

Check:

* global `AGENTS.md`;
* global skills;
* global commands;
* global MCP configuration;
* project `AGENTS.md`;
* project OpenCode configuration;
* project `.opencode/` directory;
* existing project-specific skills;
* existing project-specific commands;
* existing MCP servers.

Relevant global locations may include:

```text
~/.config/opencode/
├── AGENTS.md
├── skills/
├── commands/
└── opencode.json
```

Relevant project locations may include:

```text
<project>/
├── AGENTS.md
├── opencode.json
└── .opencode/
    ├── skills/
    └── commands/
```

Treat existing configuration as authoritative unless there is evidence that it is incorrect.

Do not overwrite existing configuration blindly.

---

# 2. Establish the global baseline

Determine which capabilities are already available globally.

Identify:

* reusable engineering instructions;
* available global skills;
* available global workflows;
* external knowledge MCP servers;
* existing conventions for validation and tool usage.

The goal is to answer:

> What does this project need that the global configuration does not already provide?

Do not create project copies of global capabilities.

For example, if these already exist globally:

```text
debugging
python-engineer
ml-engineer
model-evaluator
rag-engineer
agent-engineer
```

do not recreate them under:

```text
.opencode/skills/
```

Project-specific configuration must complement global configuration, not replace it.

---

# 3. Inspect the repository

Inspect the smallest relevant set of files necessary to understand the project.

Identify when relevant:

* language;
* runtime;
* package manager;
* frameworks;
* application type;
* architecture;
* entry points;
* source directories;
* tests;
* build commands;
* linting;
* type checking;
* infrastructure;
* databases;
* external APIs;
* cloud services;
* ML infrastructure;
* model serving;
* experiment tracking;
* vector databases;
* observability;
* CI/CD;
* deployment;
* documentation;
* repository-specific conventions.

Prefer evidence from:

* dependency files;
* configuration;
* source code;
* Docker files;
* CI files;
* infrastructure files;
* tests;
* documentation.

Do not infer the entire stack from filenames alone.

---

# 4. Inspect project instructions

If `AGENTS.md` exists, inspect it.

If it does not exist, report that `/init` should normally be used to generate project-specific repository instructions.

Do not recreate the purpose of `/init`.

The responsibility of this workflow is:

```text
/init
→ repository instructions

/setup-project
→ OpenCode project environment
```

If `AGENTS.md` already exists, determine whether important project-specific operational information is missing.

Only propose additions that are durable and repository-specific.

Do not put temporary task state into `AGENTS.md`.

---

# 5. Detect deterministic local tools

Identify capabilities that are already available through normal CLI tools.

Examples:

```text
git
uv
pip
poetry
pytest
ruff
mypy
npm
pnpm
docker
terraform
kubectl
make
```

Prefer direct CLI usage when it provides reliable local access.

Do not recommend an MCP server merely because an equivalent tool exists.

Use this principle:

```text
Local deterministic operation
→ CLI

External knowledge
→ global MCP when broadly reusable

External project state or services
→ project MCP when useful
```

---

# 6. Detect project integrations

Identify external systems that materially affect development or diagnosis.

Examples:

### ML

* MLflow;
* model registry;
* feature store;
* experiment tracking;
* model serving;
* training platforms.

### LLM / Agents

* Langfuse;
* Phoenix;
* tracing platforms;
* prompt management;
* evaluation systems;
* vector databases.

### Data

* PostgreSQL;
* BigQuery;
* Snowflake;
* Databricks;
* Elasticsearch;
* Redis.

### Infrastructure

* Kubernetes;
* AWS;
* Azure;
* GCP;
* Terraform platforms.

### Development

* issue trackers;
* CI systems;
* deployment platforms.

Determine whether an MCP integration would provide capabilities that repository inspection and CLI tools cannot provide conveniently.

---

# 7. Classify potential MCP servers

For every potential MCP, classify it as:

```text
RECOMMENDED
OPTIONAL
UNNECESSARY
```

Use:

## RECOMMENDED

The integration provides important project-specific information or operations that would materially improve investigation, implementation, validation, or observability.

## OPTIONAL

Useful for some workflows but not required for normal work.

## UNNECESSARY

Repository context, CLI, existing MCP servers, or other tools already provide sufficient functionality.

Do not recommend an MCP because it is popular.

---

# 8. Evaluate MCP security

Before recommending project MCP access, consider:

* read-only versus write access;
* production versus development access;
* secrets;
* credentials;
* destructive operations;
* database mutation;
* infrastructure mutation;
* deployment operations;
* access to sensitive data.

Prefer read-only access by default.

High-impact or destructive capabilities should require explicit user approval.

Never expose credentials in generated configuration.

Use environment variables or the project's existing secret mechanism.

---

# 9. Determine whether project-specific skills are needed

Project-specific skills should be rare.

Create or recommend one only when the repository contains durable specialized knowledge that is:

1. important for repeated work;
2. not already covered by global skills;
3. specific to this project or domain;
4. complex enough to justify reusable instructions.

Examples:

```text
company-event-schema
industrial-process-domain
internal-agent-protocol
project-specific-evaluation-method
proprietary-data-contracts
```

Do not create project skills for:

* Python;
* generic debugging;
* generic ML;
* generic RAG;
* generic agents;
* generic statistics;
* generic feature engineering;
* common frameworks.

These belong in global skills.

---

# 10. Determine whether project-specific commands are needed

Project-specific commands should also be rare.

Create or recommend one only for a repeatable workflow unique to this repository.

Examples:

```text
/rebuild-index
/run-offline-eval
/deploy-shadow-model
/validate-data-contracts
```

Do not duplicate global workflows such as:

```text
/investigate
/implement
/review
/experiment
/audit
```

Prefer passing project-specific requirements to global workflows when possible.

---

# 11. Determine project configuration changes

Build the smallest required configuration.

Potential changes may include:

```text
opencode.json
.opencode/skills/
.opencode/commands/
AGENTS.md additions
```

Only create directories that will actually contain project-specific configuration.

Do not create empty structures for completeness.

---

# 12. Preserve existing configuration

When `opencode.json` already exists:

* inspect it first;
* preserve unrelated settings;
* preserve providers and models;
* preserve existing MCP servers;
* preserve permissions;
* preserve formatting when practical;
* modify only necessary keys.

Never replace the entire file with a generated template unless the existing file is empty or explicitly disposable.

---

# 13. Produce the project setup proposal

Before making changes, present a concise plan using:

```text
Detected Stack
Existing Global Coverage
Project-Specific Needs
Recommended MCPs
Optional MCPs
Unnecessary MCPs
Project Skills Needed
Project Commands Needed
Configuration Changes
Security Considerations
```

Example:

```text
Detected Stack
- Python 3.12
- uv
- FastAPI
- PostgreSQL
- MLflow
- Docker

Existing Global Coverage
- python-engineer
- debugging
- ml-engineer
- model-evaluator
- /implement
- /investigate
- /review

Recommended MCPs
- MLflow: access to experiment runs and model metadata

Optional MCPs
- PostgreSQL: useful for data investigation

Unnecessary MCPs
- Docker: CLI already provides sufficient functionality
- Git: CLI already provides sufficient functionality

Project Skills Needed
- None

Project Commands Needed
- None

Configuration Changes
- Add project MLflow MCP to opencode.json
```

---

# 14. Require approval before external access changes

Do not automatically add MCP servers, credentials, permissions, or integrations that provide access to external systems.

Present the proposed integration first.

Only configure external access when explicitly approved.

Safe local project structure and configuration inspection may proceed without approval.

Do not install packages, MCP servers, plugins, or external software automatically unless explicitly requested.

---

# 15. Apply approved changes

After approval, apply only the approved delta.

Examples:

```text
project/
├── AGENTS.md
├── opencode.json
└── .opencode/
    ├── skills/
    │   └── only-if-project-specific/
    └── commands/
        └── only-if-project-specific.md
```

Do not copy anything from:

```text
~/.config/opencode/skills/
~/.config/opencode/commands/
```

into the repository.

---

# 16. Validate the configuration

After making changes:

* verify configuration syntax;
* verify existing configuration still works;
* verify MCP definitions are valid;
* verify referenced environment variables are documented;
* verify no secrets were committed;
* verify project commands can be discovered;
* verify project skills can be discovered;
* check for accidental duplication with global resources.

If practical, run the relevant OpenCode configuration or MCP listing command to confirm discovery.

Do not claim an integration works unless it was actually validated.

---

# 17. Report

Return:

# Project Setup Summary

## Detected Stack

What was identified.

## Existing Global Coverage

Which global capabilities already cover the project.

## Added Project Configuration

Only the delta that was created.

## MCP Integrations

Configured, recommended, optional, and rejected integrations.

## Project-Specific Skills

Created or explicitly state `None`.

## Project-Specific Commands

Created or explicitly state `None`.

## Validation

What was verified.

## Remaining Manual Actions

Credentials, authentication, approvals, or external setup still required.

## Final Structure

Show only the relevant OpenCode structure.

---

# Core Rules

Always follow these principles:

1. Global capabilities are the reusable baseline.
2. Project configuration contains only the delta.
3. Never duplicate global skills.
4. Never duplicate global commands.
5. Do not overwrite global configuration.
6. Do not overwrite project configuration blindly.
7. CLI before MCP for deterministic local operations.
8. MCP only when external project state or capabilities justify it.
9. Prefer read-only MCP access.
10. Never expose secrets.
11. Project-specific skills must encode genuinely project-specific reusable knowledge.
12. Project-specific commands must represent genuinely project-specific repeated workflows.
13. Do not create configuration merely to make the project look complete.
14. Every addition must have a concrete reason.
15. Keep the setup minimal.
