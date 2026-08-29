---
name: nlp-engineer
description: >
  Design, implement, debug, evaluate, and improve Natural Language Processing
  systems involving text classification, information extraction, named entity
  recognition, semantic similarity, embeddings, text preprocessing, document
  processing, clustering, topic analysis, search, ranking, and language
  understanding. Use when the task primarily involves modeling, representing,
  extracting information from, or analyzing natural-language text, especially
  when dataset construction, text representation, linguistic structure,
  embeddings, model selection, or NLP-specific evaluation matters.
  For retrieval-augmented generation pipelines where retrieved knowledge is assembled as evidence for an LLM response, prefer rag-engineer as the primary skill.
---

# NLP Engineer

Use this skill when natural-language text is a primary data modality and the
problem requires NLP-specific reasoning.

Typical tasks include:

* text classification;
* multilabel classification;
* named entity recognition;
* information extraction;
* document classification;
* intent classification;
* semantic similarity;
* text embeddings;
* sentence embeddings;
* text clustering;
* topic analysis;
* duplicate detection;
* semantic matching;
* search and ranking;
* document processing;
* sequence labeling;
* text preprocessing;
* NLP dataset construction;
* NLP model evaluation.

Do not invoke this skill merely because an LLM receives text.

For generative LLM behavior use `llm-evaluation`, `agent-engineer`,
`context-engineering`, or `rag-engineer` as appropriate.

Use this skill when the problem is fundamentally about understanding,
representing, classifying, extracting, or modeling language.

## Main Goal

Build NLP systems whose representation, model, evaluation, and deployment
strategy match the linguistic problem being solved.

Prefer the simplest approach capable of meeting the requirements.

A useful default progression is:

```text
business problem
      ↓
NLP task
      ↓
dataset
      ↓
baseline
      ↓
text representation
      ↓
model
      ↓
evaluation
      ↓
error analysis
      ↓
production system
```

Do not start by choosing a transformer model.

## Define the NLP Task

Translate the practical problem into a precise NLP task.

Examples:

```text
"Identify the type of legal document"
→ document classification
```

```text
"Extract company names and contract dates"
→ named entity recognition / information extraction
```

```text
"Find documents with similar meaning"
→ semantic similarity / retrieval
```

```text
"Determine whether two descriptions refer to the same issue"
→ sentence-pair classification / semantic matching
```

Different NLP tasks require different datasets, models, and metrics.

## Unit of Analysis

Determine what constitutes one example.

Possible units include:

* token;
* sentence;
* paragraph;
* document;
* document pair;
* query-document pair;
* conversation;
* message.

Do not assume one row equals one independent document.

## Dataset Construction

Understand how text examples were collected and labeled.

Inspect:

* source;
* time period;
* language;
* document type;
* label generation;
* annotation process;
* duplicates;
* templates;
* metadata;
* class distribution.

Text datasets often contain hidden shortcuts.

Dataset construction is frequently more important than model complexity.

## Train / Validation / Test Splits

Choose splits that reflect real usage.

Potential strategies include:

* random split;
* stratified split;
* group split;
* entity split;
* temporal split;
* source-based split.

Watch for related texts across splits.

Examples:

```text
same contract template
same customer
same document version
same conversation
near-duplicate text
```

Random splitting can substantially overestimate performance when similar
documents appear across splits.

## Duplicate Detection

Check both exact and near duplicates.

Exact duplicates can be identified through normalized text or hashes.

Near duplicates may require:

* lexical similarity;
* MinHash;
* embeddings;
* document fingerprints.

Duplicate leakage can produce deceptively strong NLP metrics.

## Text Leakage

Potential leakage sources include:

* label names appearing directly in text;
* post-outcome information;
* filenames;
* headers;
* template artifacts;
* metadata;
* human annotations added after the event;
* duplicated documents.

Suspiciously high performance should trigger leakage investigation.

Use `debugging` or `eda-specialist` when deeper investigation is required.

## Text Normalization

Potential normalization includes:

* Unicode normalization;
* whitespace normalization;
* encoding cleanup;
* HTML removal;
* case normalization;
* punctuation handling.

Apply only transformations justified by the representation and model.

Modern transformer models often require much less normalization than classical
NLP pipelines.

Do not destroy meaningful linguistic information unnecessarily.

## Tokenization

Tokenization determines how text becomes model input.

Consider:

* word tokenization;
* subword tokenization;
* character tokenization;
* model-specific tokenizers.

For pretrained transformer models, use the tokenizer associated with the model
unless there is a strong reason not to.

Track tokenizer versions when reproducibility matters.

## Stopwords

Do not remove stopwords automatically.

Words traditionally considered stopwords may carry important meaning.

Examples:

```text
not
without
against
before
after
```

Their importance depends on the task.

## Stemming and Lemmatization

Use stemming or lemmatization only when the downstream representation benefits.

They may help some lexical systems but are often unnecessary for transformer
models.

Do not add linguistic preprocessing merely because it is traditional NLP
practice.

## Classical Text Representations

Simple representations remain strong baselines.

Consider:

### Bag of Words

Useful when token occurrence is informative.

### TF-IDF

Often a strong baseline for classification and retrieval.

### Character N-Grams

Useful for:

* noisy text;
* spelling variation;
* morphology;
* identifiers;
* short text.

### Word N-Grams

Useful when short lexical patterns carry meaning.

Do not skip strong lexical baselines.

## Embeddings

Embeddings represent text in continuous vector spaces.

Possible levels include:

* token embeddings;
* word embeddings;
* sentence embeddings;
* document embeddings.

Evaluate embedding models based on the downstream task.

Do not assume a newer or larger embedding model is automatically better.

## Embedding Evaluation

For embedding-based systems, evaluate the behavior embeddings are intended to
support.

Possible evaluations include:

* semantic similarity;
* retrieval;
* classification;
* clustering;
* nearest-neighbor quality.

Intrinsic geometric properties alone do not prove downstream usefulness.

## Embedding Similarity

Common similarity measures include:

```text
cosine similarity
dot product
Euclidean distance
```

Choose based on the embedding model and indexing strategy.

Understand whether embeddings are normalized.

Do not switch distance functions arbitrarily.

## Dimensionality

Embedding dimensionality affects:

* storage;
* memory;
* retrieval speed;
* downstream model complexity.

Reducing dimensionality may be useful but can lose information.

Evaluate the tradeoff empirically.

## Classical Models

Strong NLP baselines may include:

* logistic regression;
* Naive Bayes;
* linear SVM;
* tree-based models when suitable.

For sparse TF-IDF representations, linear models are often strong and efficient.

Do not assume deep learning is required.

## Neural NLP Models

Possible architectures include:

* recurrent neural networks;
* CNN-based text models;
* transformers;
* encoder models;
* sequence-to-sequence models.

Choose architecture based on task and constraints.

Avoid unnecessary architectural complexity.

## Pretrained Language Models

Pretrained encoders can be useful for:

* classification;
* NER;
* similarity;
* reranking;
* representation learning.

Consider:

* model size;
* language coverage;
* domain;
* context length;
* latency;
* hardware;
* licensing.

Use `context7` when current library or model APIs need verification.

## Fine-Tuning

Fine-tuning may be appropriate when:

* sufficient labeled data exists;
* domain adaptation matters;
* prompting or embeddings are insufficient;
* latency requires a specialized smaller model.

Before fine-tuning, establish a baseline.

Compare against simpler alternatives.

Do not fine-tune merely because it is technically possible.

## Transfer Learning

Pretrained representations often reduce data requirements.

Evaluate whether:

```text
general pretrained model
```

is sufficient before:

```text
domain adaptation
```

or:

```text
task-specific fine-tuning
```

## Text Classification

For classification tasks, define:

* label semantics;
* mutually exclusive vs multilabel;
* class imbalance;
* ambiguous examples;
* annotation quality.

Evaluate using metrics appropriate to the decision.

Possible metrics include:

* accuracy;
* precision;
* recall;
* F1;
* macro F1;
* weighted F1;
* PR-AUC;
* ROC-AUC.

Use `model-evaluator` for broader predictive evaluation.

## Multiclass Classification

Inspect the confusion matrix.

Look for:

* systematically confused classes;
* hierarchical relationships;
* ambiguous labels;
* low-support classes.

Aggregate accuracy may hide severe class-specific failures.

## Multilabel Classification

Multilabel tasks require explicit evaluation choices.

Possible metrics include:

* micro F1;
* macro F1;
* per-label precision/recall;
* subset accuracy;
* Hamming loss.

Do not evaluate multilabel systems as ordinary multiclass classification.

## Named Entity Recognition

For NER, define the entity schema clearly.

Examples:

```text
PERSON
ORGANIZATION
DATE
CONTRACT_ID
AMOUNT
```

Evaluate:

* entity detection;
* entity type;
* span boundaries.

Token-level accuracy can be misleading because most tokens may not belong to
entities.

Prefer entity-level evaluation when appropriate.

## Information Extraction

Information extraction may involve:

```text
document
   ↓
entities
relations
attributes
structured record
```

Evaluate each important field.

Separate:

* extraction failure;
* incorrect value;
* incorrect relation;
* missing information.

When LLMs perform extraction, combine with `llm-evaluation`.

## Semantic Similarity

Define what "similar" means for the application.

Possible interpretations include:

* same topic;
* same intent;
* paraphrase;
* same entity;
* same legal concept;
* interchangeable meaning.

Embedding similarity is only useful when the geometric notion of similarity
matches the task.

## Search and Ranking

NLP systems may rank text using:

* lexical retrieval;
* embedding similarity;
* learned rankers;
* cross-encoders.

Possible metrics include:

* Recall@K;
* Precision@K;
* MRR;
* NDCG.

Use `rag-engineer` when retrieval feeds a RAG system.

## Reranking

Cross-encoders or learned rankers can improve ordering after candidate
retrieval.

Use reranking when:

```text
candidate recall is good
but ordering is poor
```

Do not use reranking to compensate for documents that were never retrieved.

## Clustering

Text clustering may use embeddings or lexical representations.

Evaluate:

* stability;
* cluster size;
* semantic coherence;
* downstream usefulness.

Intrinsic clustering metrics do not guarantee meaningful semantic groups.

Inspect representative examples.

## Topic Analysis

Possible methods include:

* keyword analysis;
* NMF;
* LDA;
* embedding clustering;
* modern topic-modeling approaches.

Topic models are exploratory tools.

Do not interpret generated topics as objective ground truth.

## Language Detection

For multilingual datasets, identify language distribution.

Different languages may require:

* different preprocessing;
* multilingual embeddings;
* language-specific models;
* segmented evaluation.

Do not assume a model performs equally across languages.

## Multilingual NLP

Evaluate performance separately by language when possible.

Consider:

* language imbalance;
* translation artifacts;
* tokenization differences;
* culturally specific terminology.

Aggregate multilingual metrics may hide weak languages.

## Domain-Specific Language

Specialized domains may contain:

* abbreviations;
* technical vocabulary;
* identifiers;
* uncommon entities;
* domain-specific meanings.

Examples include:

* legal;
* financial;
* medical;
* scientific;
* engineering text.

Determine whether general-purpose representations handle the domain adequately
before introducing domain-specific models.

## Long Documents

Long documents create representation and context challenges.

Possible strategies include:

* truncation;
* section-level processing;
* sliding windows;
* hierarchical encoding;
* document chunking;
* retrieval.

Do not silently truncate text without understanding what information is lost.

Use `context-engineering` when context construction becomes central.

## Document Structure

Preserve structure when it carries meaning.

Potential elements include:

* title;
* headings;
* sections;
* paragraphs;
* tables;
* lists;
* footnotes.

Flattening a document into raw text may remove useful signals.

## Metadata

Metadata can improve NLP models but may also create shortcuts.

Examples:

* author;
* source;
* timestamp;
* document type;
* category.

Ask whether metadata will exist at inference time and whether reliance on it is
desirable.

## Label Quality

NLP labels can be subjective.

Inspect:

* annotation guidelines;
* disagreement;
* ambiguous examples;
* inconsistent labeling;
* label drift.

Poor label quality creates an upper bound on meaningful model performance.

## Inter-Annotator Agreement

When multiple humans label data, agreement may reveal task ambiguity.

Possible measures include:

* Cohen's kappa;
* Fleiss' kappa;
* Krippendorff's alpha.

Interpret agreement in the context of the task.

Do not use agreement metrics mechanically.

Use `statistician` when deeper inference is required.

## Error Analysis

Inspect actual NLP failures.

Possible categories include:

* negation;
* ambiguity;
* rare terminology;
* long-range dependencies;
* unseen entities;
* spelling;
* formatting;
* multilingual text;
* truncation;
* annotation ambiguity;
* domain shift.

Create a failure taxonomy when it guides improvements.

## Slice Evaluation

Evaluate meaningful text slices.

Examples:

* short vs long documents;
* language;
* source;
* document type;
* rare classes;
* entity density;
* noisy vs clean text.

Aggregate performance may hide important weaknesses.

## Robustness

Test realistic variations when relevant.

Examples:

* whitespace changes;
* capitalization;
* minor spelling errors;
* formatting changes;
* OCR noise;
* reordered irrelevant sections.

Do not create adversarial perturbations disconnected from real usage unless
robustness itself is the objective.

## Distribution Shift

Text changes over time.

Potential shifts include:

* vocabulary;
* topics;
* document templates;
* sources;
* label frequencies;
* writing style.

Evaluate whether models remain effective on newer data.

Use `ml-engineer` for production monitoring.

## Evaluation

Match metrics to the NLP task.

Do not use one universal NLP metric.

Examples:

```text
classification
→ precision / recall / F1

NER
→ entity-level precision / recall / F1

ranking
→ Recall@K / MRR / NDCG

similarity
→ correlation / retrieval evaluation
```

Use `model-evaluator` for traditional predictive evaluation and
`llm-evaluation` when generative outputs are involved.

## Baselines

Always establish a meaningful baseline when evaluating a new NLP approach.

Potential baselines include:

* majority class;
* keyword rules;
* TF-IDF + linear classifier;
* BM25;
* existing model;
* current business rules.

Modern NLP models should earn their complexity.

## Experimentation

Use controlled experiments when comparing:

* representations;
* preprocessing;
* embedding models;
* architectures;
* fine-tuning strategies.

Use `experiment-designer` for rigorous experimental design.

## Production Inference

Consider:

* latency;
* throughput;
* batching;
* model size;
* tokenizer cost;
* hardware;
* memory;
* concurrency.

A slightly better model may not justify dramatically higher inference cost.

Use `ml-engineer` for production architecture.

## Privacy and Sensitive Text

Text may contain:

* personal information;
* confidential information;
* credentials;
* legal information;
* proprietary documents.

Minimize unnecessary exposure.

Respect access controls and retention requirements.

Do not include sensitive text in logs without justification.

## Reproducibility

Track when relevant:

* dataset version;
* preprocessing;
* tokenizer;
* vocabulary;
* embedding model;
* model version;
* random seed;
* evaluation split.

Text representations can change substantially across model or tokenizer
versions.

## When Other Skills Should Be Combined

Examples:

* `nlp-engineer` + `eda-specialist` for text dataset exploration;
* `nlp-engineer` + `feature-engineering` for text representations;
* `nlp-engineer` + `model-evaluator` for predictive NLP models;
* `nlp-engineer` + `experiment-designer` for model comparison;
* `nlp-engineer` + `ml-engineer` for production NLP systems;
* `nlp-engineer` + `rag-engineer` for retrieval systems;
* `nlp-engineer` + `llm-evaluation` when LLM outputs are involved;
* `nlp-engineer` + `context-engineering` for long-document processing;
* `nlp-engineer` + `debugging` for unexpected NLP behavior;
* `nlp-engineer` + `statistician` for statistical analysis.

Do not invoke additional skills unless they materially improve the task.

## Required Output

For substantial NLP work, report:

### Task

The NLP problem being solved.

### Data

Dataset structure, labels, text characteristics, and important risks.

### Baseline

The simplest meaningful approach.

### Representation

How text should be represented and why.

### Model

Recommended modeling approach.

### Evaluation

Metrics, slices, and validation strategy.

### Failure Modes

Likely or observed NLP-specific weaknesses.

### Implementation

Practical approach when requested.

### Risks

Leakage, duplicates, label quality, domain shift, privacy, or operational
limitations.

For simple NLP questions, keep output proportional to the request.

## Rules

* Define the NLP task before selecting a model.
* Understand how the text dataset was created.
* Check exact and near duplicates.
* Investigate text-specific leakage.
* Do not remove stopwords automatically.
* Do not apply stemming or lemmatization automatically.
* Preserve useful document structure.
* Do not silently truncate important text.
* Use simple lexical baselines when appropriate.
* Do not assume embeddings are always superior.
* Do not assume larger language models are always better.
* Evaluate meaningful slices.
* Match metrics to the NLP task.
* Separate predictive NLP evaluation from generative LLM evaluation.
* Consider latency and model size.
* Protect sensitive text.
* Prefer the simplest approach that meets the requirements.
