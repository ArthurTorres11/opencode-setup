---
name: rag-engineer
description: >
  Design, implement, debug, evaluate, and improve retrieval-augmented generation
  systems involving document ingestion, parsing, chunking, metadata, embeddings,
  sparse and dense retrieval, hybrid search, filtering, reranking, query
  transformation, contextual retrieval, context assembly, citations, grounded
  generation, and retrieval evaluation. Use when the task involves RAG, document
  QA, enterprise search, knowledge bases, semantic search, retrieval failures,
  poor grounding, missing evidence, irrelevant context, or improving how external
  knowledge is retrieved and provided to an LLM.
---

# RAG Engineer

Use this skill when retrieval is a material part of an LLM or AI system.

Typical cases include:

* retrieval-augmented generation;
* document question answering;
* enterprise search;
* knowledge assistants;
* semantic search;
* hybrid retrieval;
* vector databases;
* embeddings;
* chunking;
* reranking;
* metadata filtering;
* citations;
* grounded generation;
* retrieval evaluation;
* agentic retrieval;
* multi-hop retrieval;
* context assembly.

Do not invoke this skill merely because an application contains documents or an
LLM.

Use it when external knowledge must be retrieved, ranked, filtered, assembled, or
used as evidence.

## Main Goal

Provide the model with the smallest set of high-quality evidence necessary to
answer the query correctly.

Separate:

```text
retrieval quality
        ↓
context quality
        ↓
generation quality
```

Do not treat RAG as one opaque component.

A poor answer can originate from:

* ingestion;
* parsing;
* chunking;
* metadata;
* embedding;
* query construction;
* retrieval;
* filtering;
* reranking;
* context assembly;
* generation.

Identify the failing layer before modifying the system.

## Start With Expected Behavior

Before optimizing RAG, define what the system should retrieve and answer.

Determine:

* user query type;
* relevant source type;
* expected evidence;
* acceptable sources;
* freshness requirements;
* citation requirements;
* refusal behavior;
* latency requirements;
* cost constraints.

Do not optimize retrieval without representative queries and expected behavior.

## RAG Pipeline

Map the actual pipeline.

A typical architecture may contain:

```text
documents
   ↓
ingestion
   ↓
parsing
   ↓
normalization
   ↓
chunking
   ↓
metadata
   ↓
indexing
   ↓
query processing
   ↓
candidate retrieval
   ↓
filtering
   ↓
reranking
   ↓
context assembly
   ↓
generation
   ↓
citations
```

Not every system requires every stage.

Do not add components merely because they are common in RAG architectures.

## Ingestion

Verify that expected source data enters the system correctly.

Check:

* missing documents;
* duplicate documents;
* stale documents;
* unsupported formats;
* failed ingestion jobs;
* access permissions;
* source identifiers;
* version handling;
* deletion propagation.

A retrieval system cannot recover documents that were never indexed correctly.

## Parsing

Inspect whether documents are represented correctly before embedding.

Potential problems include:

* missing sections;
* broken tables;
* lost headings;
* repeated headers or footers;
* incorrect page ordering;
* OCR errors;
* malformed encoding;
* merged unrelated text;
* missing structured fields.

Always inspect representative parsed output before blaming embeddings or the
retriever.

## Document Structure

Preserve useful document structure when possible.

Useful signals may include:

* title;
* section;
* subsection;
* page;
* document type;
* date;
* author;
* product;
* jurisdiction;
* access level;
* parent-child relationships.

Structure can improve both retrieval and citation quality.

Do not discard useful structure during preprocessing without evidence that it is
unnecessary.

## Chunking

Chunking defines the retrieval unit.

Evaluate whether chunks preserve enough semantic context while remaining
retrievable.

Consider:

* semantic boundaries;
* headings;
* paragraphs;
* sections;
* tables;
* lists;
* code;
* document type.

Do not choose chunk size based only on a generic token number.

The appropriate chunking strategy depends on the content and query patterns.

## Chunk Size

Chunks that are too small may:

* lose semantic context;
* split definitions from explanations;
* separate references from entities.

Chunks that are too large may:

* reduce retrieval precision;
* introduce irrelevant content;
* consume excessive context;
* bury relevant evidence.

Evaluate chunk size empirically.

Do not assume larger chunks automatically improve answer quality.

## Chunk Overlap

Overlap can preserve continuity across boundaries.

But excessive overlap can create:

* duplicate retrieval results;
* wasted context;
* inflated indexing;
* repeated evidence.

Use overlap when document structure requires it.

Do not use large overlap automatically.

## Parent-Child Retrieval

When small chunks retrieve well but lack enough context, consider parent-child
strategies.

Example:

```text
small searchable chunk
        ↓
retrieve
        ↓
return larger parent section
```

This can improve precision while preserving broader context.

Do not use parent-child retrieval unless it solves an observed problem.

## Metadata

Metadata can materially improve retrieval quality.

Potential metadata includes:

* document ID;
* source;
* title;
* section;
* page;
* date;
* category;
* language;
* entity;
* product;
* access control;
* version.

Metadata should support actual retrieval, filtering, authorization, or citation
requirements.

Do not collect metadata simply because it is available.

## Metadata Filtering

Use metadata filtering when semantic similarity alone cannot reliably enforce
constraints.

Examples:

* user permissions;
* document type;
* date range;
* product;
* region;
* language;
* department.

Prefer deterministic filtering for deterministic constraints.

Do not ask an embedding model to solve access control.

## Embeddings

Choose embeddings based on actual retrieval requirements.

Consider:

* language;
* domain;
* semantic complexity;
* query/document asymmetry;
* dimensionality;
* latency;
* cost;
* provider constraints.

Do not assume that a newer or larger embedding model automatically improves the
system.

Evaluate on representative retrieval cases.

## Embedding Compatibility

Track important embedding properties:

* model identifier;
* model version;
* dimensionality;
* normalization;
* distance metric.

Documents and queries must use compatible representations.

If the embedding model changes, determine whether the index must be rebuilt.

Do not mix incompatible embedding spaces silently.

## Dense Retrieval

Dense retrieval is useful for semantic similarity.

It often performs well when queries and documents use different wording but
similar meaning.

Potential weaknesses include:

* exact identifiers;
* rare terms;
* acronyms;
* codes;
* names;
* numerical expressions.

Do not assume dense retrieval solves every search problem.

## Sparse Retrieval

Sparse retrieval such as BM25 can perform well for:

* exact terminology;
* identifiers;
* names;
* acronyms;
* uncommon terms;
* lexical matches.

Sparse retrieval should not be treated as obsolete merely because embeddings are
available.

## Hybrid Search

Hybrid retrieval combines lexical and semantic signals.

Typical pattern:

```text
dense retrieval
      +
sparse retrieval
      ↓
candidate fusion
      ↓
reranking
```

Use hybrid retrieval when representative evaluation shows complementary value.

Do not add hybrid search merely because it is considered a modern RAG pattern.

## Candidate Retrieval

Candidate retrieval should prioritize recall.

The first stage may retrieve more candidates than the final context will contain.

Separate:

```text
candidate generation
        ↓
candidate ranking
        ↓
final context
```

Do not assume `top_k` candidates must equal the number of chunks passed to the
LLM.

## Top-K

Top-K is an evaluation parameter, not a universal constant.

Increasing K may improve recall but can reduce context quality through noise.

Evaluate:

* recall;
* precision;
* answer quality;
* context size;
* latency;
* cost.

Choose K based on measured tradeoffs.

## Query Processing

The raw user query may not always be the best retrieval query.

Possible transformations include:

* normalization;
* query rewriting;
* typo correction;
* acronym expansion;
* decomposition;
* metadata extraction.

Do not transform queries without preserving user intent.

Always evaluate whether transformation improves retrieval.

## Query Rewriting

Query rewriting can help when conversational or ambiguous queries do not map well
to indexed content.

A rewritten query should preserve:

* entities;
* constraints;
* intent;
* relevant conversational references.

Do not allow query rewriting to invent requirements or remove important details.

## Query Expansion

Expansion may add:

* synonyms;
* aliases;
* related terminology;
* domain-specific equivalents.

It can improve recall but also introduce noise.

Evaluate it rather than assuming more query terms are better.

## Query Decomposition

Complex questions may contain multiple information needs.

Example:

```text
Question
  ↓
sub-question A
sub-question B
sub-question C
  ↓
retrieve separately
  ↓
combine evidence
```

Use decomposition when a single retrieval query cannot reliably cover all parts.

Do not decompose simple queries unnecessarily.

## Multi-Hop Retrieval

Some questions require retrieving evidence in multiple stages.

Example:

```text
query
 ↓
retrieve entity A
 ↓
discover entity B
 ↓
retrieve evidence about B
```

Use multi-hop retrieval when later retrieval depends on information discovered
earlier.

Do not turn ordinary retrieval into an agent loop without measurable benefit.

## Reranking

Use reranking when candidate retrieval has adequate recall but poor ordering.

Potential rerankers include:

* cross-encoders;
* dedicated reranking models;
* semantic scoring;
* LLM-based reranking.

Evaluate:

* ranking improvement;
* latency;
* cost.

Do not add expensive reranking when the base retriever already performs
adequately.

## Reranking Diagnostics

Before adding reranking, determine:

```text
Is the relevant document retrieved at all?
```

If no, the problem is recall.

If yes but ranked poorly, reranking may help.

Do not use reranking to compensate for missing candidates.

## Context Assembly

Retrieved chunks are not automatically good context.

Context assembly should consider:

* relevance;
* redundancy;
* diversity;
* ordering;
* total size;
* source boundaries;
* citations;
* user query.

Remove duplicates and near-duplicates when practical.

Avoid passing excessive retrieved content to the model.

Use `context-engineering` when context size or organization becomes a central
problem.

## Context Ordering

Order evidence intentionally.

Possible strategies include:

* relevance order;
* document structure;
* chronological order;
* source grouping;
* logical dependency.

The best strategy depends on the task.

Do not assume retrieval score order is always the best generation order.

## Contextual Retrieval

Chunks may be difficult to understand when detached from their parent document.

Adding compact contextual information can improve retrieval.

For example, a chunk may include:

```text
Document: Security Policy
Section: Authentication
Topic: Password Reset
```

Use contextualization when evaluation shows that isolated chunks lose critical
meaning.

Avoid bloating every chunk with unnecessary metadata.

## Context Compression

When retrieval returns more evidence than the model needs, consider compressing
or selecting context.

Preserve:

* relevant facts;
* citations;
* qualifiers;
* relationships;
* uncertainty.

Do not summarize away details required for factual grounding.

Use `context-engineering` for broader compression strategies.

## Generation

Generation should use retrieved evidence appropriately.

Define whether the model may:

* use only retrieved evidence;
* supplement with general knowledge;
* express uncertainty;
* refuse when evidence is insufficient.

Make these policies explicit.

Do not rely on vague instructions such as:

> Use the context.

## Grounded Answers

For knowledge-grounded tasks, instruct the system to distinguish:

* supported claims;
* uncertain claims;
* unsupported claims.

If evidence is insufficient, prefer an explicit insufficient-evidence behavior
over fabrication.

Do not force the system to answer when retrieval does not support the answer.

## Evidence Sufficiency

Retrieval relevance and evidence sufficiency are different.

A document can be relevant to the topic without containing enough information to
answer the question.

When necessary, evaluate:

```text
retrieved evidence
        ↓
sufficient?
   ┌────┴────┐
  yes       no
   ↓         ↓
answer    retrieve more
          or refuse
```

Prefer deterministic sufficiency checks when they exist.

Use semantic judgment only when necessary.

## Citations

Citations should point to evidence that actually supports the claim.

Evaluate:

* source identity;
* chunk identity;
* page or section;
* claim-evidence alignment;
* citation completeness.

Do not generate citation identifiers that are not linked to retrieved sources.

## Citation Accuracy

A response may contain citations and still be incorrectly grounded.

Check whether each important cited claim is actually supported by its cited
source.

Separate:

* citation presence;
* citation correctness;
* citation completeness.

## Refusal

Define behavior for insufficient evidence.

A useful RAG system should know when retrieval does not support an answer.

Evaluate refusal behavior using cases where:

* evidence exists;
* evidence is incomplete;
* evidence does not exist;
* retrieved documents are misleading.

Avoid a system that refuses too frequently merely to reduce hallucinations.

## Retrieval Failure Taxonomy

Classify failures before changing architecture.

Common categories include:

### Ingestion Failure

Required source missing from index.

### Parsing Failure

Information lost or corrupted before indexing.

### Chunking Failure

Relevant information split or mixed incorrectly.

### Metadata Failure

Filtering or source identification incorrect.

### Embedding Failure

Semantic representation inadequate.

### Retrieval Recall Failure

Relevant evidence not retrieved.

### Ranking Failure

Relevant evidence retrieved but ranked too low.

### Filtering Failure

Correct candidates removed.

### Context Assembly Failure

Correct evidence retrieved but poorly presented to the model.

### Generation Failure

Correct context exists but answer is incorrect.

This separation is critical for efficient debugging.

## Retrieval Evaluation

Evaluate retrieval independently from generation.

Create queries with known relevant evidence when practical.

Useful metrics include:

### Recall@K

Whether relevant evidence appears in the top K results.

### Precision@K

How much of the top K is relevant.

### MRR

How high the first relevant result appears.

### NDCG

How well relevant results are ranked.

Metrics should reflect the retrieval task.

Do not use every metric automatically.

## Answer Evaluation

Depending on the application, evaluate:

* correctness;
* groundedness;
* completeness;
* relevance;
* citation accuracy;
* refusal accuracy.

Use `llm-evaluation` for deeper semantic evaluation methodology.

## End-to-End Evaluation

End-to-end evaluation determines whether the complete system solves the task.

Measure when relevant:

* answer success;
* retrieval success;
* latency;
* token usage;
* cost;
* refusal behavior.

Do not rely only on end-to-end scores when debugging.

Component metrics explain why performance changed.

## Golden Dataset

Maintain representative RAG evaluation cases when quality matters.

A case may contain:

* query;
* relevant document IDs;
* expected evidence;
* expected answer properties;
* expected refusal;
* metadata;
* query category.

Include previously observed failures when appropriate.

Use `llm-evaluation` for broader golden-dataset methodology.

## Error Analysis

Inspect failed cases manually.

Useful categories may include:

* no relevant evidence retrieved;
* relevant evidence ranked poorly;
* wrong metadata filter;
* duplicate chunks;
* noisy context;
* missing evidence;
* unsupported generation;
* incorrect citation;
* unnecessary refusal.

Aggregate scores alone are insufficient for deciding what to change.

## Agentic RAG

Agents can dynamically decide:

* whether retrieval is needed;
* which source to query;
* whether to reformulate;
* whether additional evidence is required.

Use agentic retrieval only when dynamic reasoning materially improves the task.

Simple RAG is usually easier to:

* evaluate;
* debug;
* operate;
* optimize.

Do not add agents when a deterministic retrieval workflow is sufficient.

Use `agent-engineer` when orchestration becomes central.

## Retrieval Tools

When retrieval is exposed as an agent tool, design the interface carefully.

Specify:

* searchable corpus;
* filters;
* query input;
* result limit;
* outputs;
* errors.

Avoid returning unnecessarily large chunks or metadata.

Use `tool-design` when retrieval tool interfaces need deeper work.

## Access Control

Authorization must be enforced outside the LLM.

When users have different document permissions:

```text
authenticated user
       ↓
allowed document scope
       ↓
retrieval
```

Do not retrieve forbidden documents and rely on the model not to reveal them.

Permissions should influence retrieval before context reaches the model.

## Freshness

For changing knowledge bases, determine:

* ingestion delay;
* update behavior;
* deletion behavior;
* version handling;
* cache invalidation.

A correct retriever over stale data still produces stale answers.

Monitor document freshness when it matters.

## Observability

For production RAG systems, capture enough information to diagnose failures.

Useful signals may include:

* original query;
* transformed query;
* filters;
* retrieved document IDs;
* retrieval scores;
* reranking scores;
* context size;
* latency by stage;
* token usage;
* refusal;
* citation behavior.

Avoid logging sensitive document contents unnecessarily.

## Performance

Measure latency by stage.

Example:

```text
query processing
retrieval
reranking
context assembly
generation
```

Potential optimizations include:

* better indexing;
* metadata filtering;
* smaller candidate sets;
* efficient reranking;
* caching;
* parallel retrieval;
* reducing unnecessary context.

Optimize the actual bottleneck.

## Cost

RAG cost may come from:

* embedding generation;
* storage;
* retrieval;
* reranking;
* LLM input tokens;
* LLM output tokens;
* evaluation.

Reducing retrieval quality to save small amounts of cost may increase total cost
through failed answers and repeated requests.

Optimize cost per successful query.

## Security

Treat indexed content as potentially untrusted.

Documents may contain prompt injection or malicious instructions.

Retrieved content should be treated as evidence, not system instructions.

Do not allow documents to override:

* system policy;
* permissions;
* tool constraints;
* approval requirements.

Use `agent-engineer` or `tool-design` when RAG content can influence external
actions.

## Debugging RAG

When RAG quality drops, trace the first divergence from expected behavior.

Recommended order:

```text
source exists?
   ↓
parsed correctly?
   ↓
chunk contains evidence?
   ↓
indexed correctly?
   ↓
query constructed correctly?
   ↓
evidence retrieved?
   ↓
ranked sufficiently high?
   ↓
included in context?
   ↓
used correctly by model?
```

Do not start by rewriting the final prompt.

Use `debugging` when systematic root-cause investigation is needed.

## When Other Skills Should Be Combined

Examples:

* `rag-engineer` + `llm-evaluation` for RAG benchmarks;
* `rag-engineer` + `context-engineering` for context assembly and compression;
* `rag-engineer` + `agent-engineer` for agentic RAG;
* `rag-engineer` + `tool-design` for retrieval tools;
* `rag-engineer` + `debugging` for unexplained RAG failures;
* `rag-engineer` + `python-engineer` for implementation;
* `rag-engineer` + `experiment-designer` for controlled retrieval experiments;
* `rag-engineer` + `statistician` for deeper metric interpretation.

Do not invoke additional skills unless they materially improve the task.

## Required Output

For substantial RAG work, report:

### Architecture

Relevant ingestion, indexing, retrieval, ranking, context, and generation flow.

### Retrieval Findings

Observed retrieval strengths and failures.

### Generation Findings

Whether the model correctly uses available evidence.

### Failure Modes

The first failing layer for important cases.

### Evaluation

Relevant component and end-to-end measurements.

### Changes

Priority-ranked improvements or implemented changes.

### Validation

How improvements were or should be compared against the baseline.

### Risks

Remaining quality, freshness, security, latency, or cost concerns.

For simple tasks, keep the output proportional to the request.

## Rules

* Separate retrieval problems from generation problems.
* Verify ingestion and parsing before tuning retrieval.
* Do not change chunk size without evidence.
* Do not assume dense retrieval is always sufficient.
* Use hybrid retrieval only when evaluation supports it.
* Do not use reranking to compensate for missing candidates.
* Treat Top-K as an evaluated parameter.
* Keep context relevant and non-redundant.
* Distinguish relevance from evidence sufficiency.
* Do not force answers without sufficient evidence.
* Verify citation support, not only citation presence.
* Enforce permissions before retrieval reaches the model.
* Treat retrieved content as untrusted.
* Do not add agents when simple RAG is sufficient.
* Evaluate retrieval independently from generation.
* Preserve important failures as regression cases.
* Prefer measurable improvements over subjective impressions.
