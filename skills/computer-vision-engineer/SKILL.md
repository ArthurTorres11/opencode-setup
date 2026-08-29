---
name: computer-vision-engineer
description: >
  Design, implement, debug, evaluate, and improve computer vision systems
  involving image classification, object detection, segmentation, OCR, visual
  similarity, image embeddings, tracking, preprocessing, augmentation, transfer
  learning, and multimodal vision models. Use when images or video are a primary
  data modality and visual modeling, annotations, vision-specific evaluation, or
  learned visual representations materially affect the task. Do not invoke for
  generic image manipulation such as resize, crop, format conversion, grayscale
  conversion, or blur when no computer vision model or visual inference problem
  is involved.
---

# Computer Vision Engineer

Use this skill when images or video are a primary data modality and the problem
requires computer-vision-specific reasoning.

Typical tasks include:

* image classification;
* multilabel image classification;
* object detection;
* semantic segmentation;
* instance segmentation;
* OCR;
* image similarity;
* image embeddings;
* visual search;
* object tracking;
* image preprocessing;
* augmentation;
* transfer learning;
* image dataset construction;
* visual anomaly detection;
* multimodal vision systems.

Do not invoke this skill merely because an application contains images.

Use it when visual representation, image modeling, annotation, evaluation, or
vision-specific failure modes materially affect the task.

## Main Goal

Build computer vision systems whose data, representation, model, evaluation,
and deployment strategy match the visual problem being solved.

A useful progression is:

```text
real-world problem
       ↓
vision task
       ↓
dataset
       ↓
baseline
       ↓
preprocessing
       ↓
model
       ↓
evaluation
       ↓
error analysis
       ↓
production
```

Do not start by selecting the largest available vision model.

## Define the Vision Task

Translate the practical problem into a precise vision task.

Examples:

```text
"Does this image contain a defect?"
→ image classification
```

```text
"Where is the defect?"
→ object detection
```

```text
"Which pixels belong to the defect?"
→ segmentation
```

```text
"Extract text from this scanned page"
→ OCR
```

```text
"Find visually similar products"
→ image embeddings / visual retrieval
```

Different tasks require different labels, architectures, and metrics.

## Unit of Analysis

Determine the actual observation unit.

Possible units include:

* image;
* crop;
* frame;
* video;
* object;
* region;
* sequence.

Multiple frames from one video are not necessarily independent observations.

This matters when splitting data and estimating model quality.

## Dataset Construction

Understand how images were collected.

Inspect:

* source;
* camera or device;
* capture conditions;
* time period;
* resolution;
* labels;
* annotation process;
* image format;
* duplicates;
* class distribution.

Visual models are highly capable of learning unintended shortcuts.

Dataset construction is therefore part of model design.

## Dataset Splitting

Choose splits that reflect real deployment.

Potential strategies include:

* random;
* stratified;
* grouped;
* temporal;
* device-based;
* location-based;
* subject-based.

Avoid leakage such as:

```text
frames from same video
→ train and test
```

or:

```text
near-duplicate image
→ train and test
```

Random image-level splitting may substantially overestimate generalization.

## Exact Duplicates

Detect exact duplicates when relevant.

Possible methods include:

* file hashes;
* decoded image hashes;
* normalized representations.

Duplicate files may exist under different filenames.

## Near Duplicates

Near duplicates are especially important in vision datasets.

Examples:

* adjacent video frames;
* resized copies;
* crops of the same image;
* compressed copies;
* slightly edited images.

Potential approaches include:

* perceptual hashing;
* embedding similarity;
* image matching.

Near-duplicate leakage can create deceptively strong results.

## Image Integrity

Check for:

* corrupted files;
* unreadable images;
* incorrect channels;
* unexpected formats;
* zero-sized images;
* invalid metadata.

Do not assume every file in an image directory is valid training data.

## Dimensions

Inspect:

* width;
* height;
* aspect ratio;
* channel count.

Unexpected distributions may reveal multiple data sources or preprocessing
problems.

## Resolution

Choose resolution based on the visual signal required by the task.

Higher resolution can preserve small details but increases:

* compute;
* memory;
* latency.

Do not increase resolution automatically.

If the relevant object occupies only a few pixels, aggressive downsampling may
destroy the signal entirely.

## Color Space

Understand expected color representation.

Examples:

* RGB;
* BGR;
* grayscale;
* multispectral.

Library defaults can differ.

Silent RGB/BGR mismatches can substantially degrade behavior.

## Pixel Scaling

Ensure training and inference use consistent scaling.

Examples include:

```text
[0, 255]
```

versus:

```text
[0, 1]
```

or model-specific normalization.

Train-inference preprocessing mismatch is a common source of failures.

## Preprocessing

Possible preprocessing includes:

* resize;
* crop;
* normalization;
* padding;
* denoising;
* contrast adjustment.

Apply transformations because they support the task or model.

Do not create complex preprocessing pipelines without evidence.

## Aspect Ratio

Naively resizing images to a fixed square may distort objects.

Potential alternatives include:

* padding;
* center crop;
* random crop;
* aspect-ratio-preserving resize.

Choose based on the task.

## Data Augmentation

Augmentation can improve generalization by creating realistic variation.

Possible transformations include:

* crop;
* flip;
* rotation;
* translation;
* scale;
* brightness;
* contrast;
* blur;
* noise.

Every augmentation encodes an assumption:

> The label should remain valid after this transformation.

Do not use augmentations that change the semantic label.

## Invalid Augmentation

Examples:

* horizontal flipping text when orientation matters;
* arbitrary rotation when object orientation defines the class;
* extreme color changes when color is predictive;
* cropping away the labeled object.

Visually plausible augmentation is not necessarily label-preserving.

## Augmentation Validation

Inspect augmented samples.

Do not trust an augmentation pipeline solely because the code executes.

Verify:

* labels remain valid;
* boxes remain aligned;
* masks remain aligned;
* objects remain visible.

## Class Balance

Inspect class distribution.

For detection or segmentation, also inspect:

* objects per image;
* object size;
* class frequency;
* empty images.

A balanced image count does not necessarily imply balanced object instances.

## Labels

Understand label semantics.

For classification:

```text
image → class
```

For detection:

```text
image → bounding boxes + classes
```

For segmentation:

```text
image → pixel mask
```

Annotation errors differ by task.

## Annotation Quality

Potential problems include:

* wrong classes;
* missing objects;
* inaccurate boxes;
* inconsistent masks;
* ambiguous labels;
* inconsistent annotation policy.

Model performance may be bounded by annotation quality.

Inspect labels visually when possible.

## Classification

For image classification, evaluate using metrics appropriate to class structure.

Possible metrics include:

* accuracy;
* precision;
* recall;
* F1;
* ROC-AUC;
* PR-AUC;
* top-k accuracy.

Inspect confusion matrices and representative errors.

Use `model-evaluator` for deeper predictive evaluation.

## Multilabel Classification

Images may contain multiple simultaneous labels.

Possible metrics include:

* micro F1;
* macro F1;
* per-label precision and recall;
* mAP.

Do not treat multilabel classification as ordinary multiclass classification.

## Object Detection

Detection systems predict:

```text
bounding box
+
class
+
confidence
```

Evaluation should consider both localization and classification.

Common concepts include:

* Intersection over Union;
* precision;
* recall;
* average precision;
* mean average precision.

Do not evaluate detection using classification accuracy alone.

## Intersection over Union

For bounding boxes or masks:

```text
IoU =
intersection area
/
union area
```

IoU measures spatial overlap.

Threshold choice affects what counts as a correct detection.

## Average Precision

Average Precision summarizes the precision-recall relationship for a class under
defined matching criteria.

mAP aggregates AP across classes and potentially IoU thresholds.

Always understand the exact mAP definition being reported.

Different benchmarks may use different conventions.

## Detection Error Analysis

Inspect:

* missed objects;
* false detections;
* wrong classes;
* poor localization;
* duplicate detections;
* small-object failures;
* overlapping-object failures.

A single mAP number does not explain these failure modes.

## Object Size

Segment evaluation by object size when relevant.

Models may perform differently on:

* small;
* medium;
* large objects.

Aggregate metrics can hide severe small-object failures.

## Segmentation

Segmentation tasks may use:

* pixel accuracy;
* IoU;
* Dice coefficient;
* class-level IoU.

For imbalanced segmentation, pixel accuracy can be misleading.

A model predicting mostly background may achieve high accuracy.

## Semantic Segmentation

Semantic segmentation assigns a class to each pixel.

Evaluate both:

* overall quality;
* per-class performance.

Inspect boundary and small-region failures.

## Instance Segmentation

Instance segmentation distinguishes individual object instances.

Evaluation typically combines:

* detection;
* classification;
* mask quality.

Do not evaluate only pixel overlap while ignoring instance identity.

## OCR

OCR pipelines may involve:

```text
image
  ↓
text detection
  ↓
text recognition
  ↓
post-processing
```

Evaluate components separately when debugging.

Possible metrics include:

* character error rate;
* word error rate;
* field-level accuracy.

Do not rely only on visual inspection of a few documents.

## OCR Data Quality

OCR performance can depend strongly on:

* resolution;
* blur;
* skew;
* lighting;
* compression;
* handwriting;
* font;
* page layout.

Segment evaluation by these conditions when they matter.

## Transfer Learning

Pretrained vision models can substantially reduce training requirements.

Typical approach:

```text
pretrained backbone
       ↓
task-specific head
       ↓
fine-tuning
```

Evaluate whether freezing or fine-tuning the backbone is appropriate.

Do not train from scratch unless there is a reason.

## Pretrained Models

When selecting pretrained models, consider:

* task;
* dataset size;
* domain similarity;
* model size;
* latency;
* memory;
* hardware;
* licensing.

Use `context7` for current framework APIs when necessary.

## Vision Transformers

Vision transformers may be appropriate for many modern vision tasks.

Do not assume they dominate every problem.

CNNs and smaller architectures may provide better latency, memory, or
data-efficiency tradeoffs.

Evaluate empirically.

## Image Embeddings

Pretrained image embeddings can support:

* similarity;
* clustering;
* retrieval;
* classification;
* anomaly detection.

Evaluate embeddings on the downstream task.

Visual similarity in embedding space may not match business-defined similarity.

## Multimodal Embeddings

Models such as image-text encoders can map images and language into related
representation spaces.

Possible uses include:

* text-to-image retrieval;
* image-to-text retrieval;
* zero-shot classification;
* multimodal search.

Evaluate whether cross-modal similarity matches the intended semantics.

## Visual Search

Visual retrieval may involve:

```text
query image
   ↓
embedding
   ↓
vector search
   ↓
candidate images
```

Possible metrics include:

* Recall@K;
* Precision@K;
* MRR;
* NDCG.

Use `rag-engineer` only when retrieved information feeds a generative RAG
system.

## Visual Anomaly Detection

Anomaly detection may be useful when defects or unusual images are rare.

Evaluate:

* false alarms;
* missed anomalies;
* localization when relevant;
* performance on realistic anomaly types.

Unsupervised anomaly scores do not automatically correspond to operationally
meaningful anomalies.

## Video

Video introduces temporal dependence.

Potential tasks include:

* action recognition;
* tracking;
* event detection;
* temporal segmentation.

Do not treat adjacent frames as independent images without justification.

## Tracking

Tracking systems associate objects across frames.

Potential evaluation concerns include:

* missed tracks;
* identity switches;
* fragmentation;
* localization.

Separate detection failures from association failures.

## Temporal Sampling

For video, determine frame sampling strategy.

Excessive adjacent frames may:

* waste compute;
* increase redundancy;
* create leakage.

Too sparse sampling may miss important events.

## Shortcuts and Spurious Features

Vision models may learn unintended signals.

Examples:

* background;
* watermark;
* camera type;
* border;
* image compression;
* acquisition location;
* annotation artifacts.

A model can achieve high accuracy for the wrong reason.

Inspect saliency or explanation methods when they answer a concrete question,
but do not treat them as proof of causality.

## Domain Shift

Vision performance may degrade across:

* cameras;
* lighting;
* environments;
* locations;
* time periods;
* populations;
* image-processing pipelines.

Evaluate relevant deployment domains separately.

## Slice Evaluation

Useful slices may include:

* camera;
* resolution;
* lighting;
* object size;
* class;
* location;
* source;
* image quality;
* time period.

Aggregate metrics may hide deployment-critical failures.

## Robustness

Test realistic variations.

Examples:

* blur;
* compression;
* moderate brightness changes;
* partial occlusion;
* realistic noise.

Do not use arbitrary corruption benchmarks unless they represent a relevant
robustness objective.

## Explainability

Potential techniques include:

* saliency maps;
* Grad-CAM;
* attention visualization;
* feature attribution.

Use explanations to investigate specific hypotheses.

Do not assume attractive heatmaps prove the model is reasoning correctly.

## Error Analysis

Inspect representative failures.

Possible categories include:

* small object;
* occlusion;
* blur;
* low light;
* unusual viewpoint;
* rare class;
* annotation error;
* background shortcut;
* domain shift.

A useful failure taxonomy should guide the next experiment.

## Evaluation

Match metrics to the vision task.

Examples:

```text
classification
→ accuracy / precision / recall / F1

detection
→ AP / mAP / recall

segmentation
→ IoU / Dice

OCR
→ CER / WER / field accuracy

retrieval
→ Recall@K / MRR / NDCG
```

Use `model-evaluator` when broader predictive evaluation is needed.

## Baselines

Establish meaningful baselines.

Possible baselines include:

* simple classifier;
* pretrained frozen backbone;
* existing production model;
* heuristic image processing;
* previous architecture.

Complex models should demonstrate measurable value over simpler alternatives.

## Experimentation

Use controlled experiments when comparing:

* architecture;
* resolution;
* augmentation;
* preprocessing;
* backbone;
* fine-tuning strategy.

Use `experiment-designer` for rigorous experiment design.

## Training

Track important training behavior:

* training loss;
* validation loss;
* task metrics;
* learning rate;
* convergence;
* overfitting.

Do not rely only on final epoch metrics.

## Overfitting

Vision models can memorize:

* backgrounds;
* devices;
* image sources;
* near duplicates.

Strong validation performance does not prove generalization if the split is
weak.

## Compute

Vision workloads can be expensive.

Consider:

* GPU memory;
* batch size;
* image resolution;
* model size;
* mixed precision;
* inference batching.

Optimize only after establishing correctness.

## Inference

Production requirements may include:

* latency;
* throughput;
* memory;
* hardware;
* batch size;
* preprocessing time.

Measure the complete pipeline, not only neural-network forward-pass time.

Use `ml-engineer` for deployment architecture.

## Edge Deployment

For edge devices, consider:

* model size;
* memory;
* power;
* hardware accelerators;
* quantization;
* pruning;
* offline operation.

A small model with slightly lower offline accuracy may be preferable under real
deployment constraints.

## Model Compression

Potential techniques include:

* quantization;
* pruning;
* distillation.

Measure both:

```text
quality
```

and:

```text
latency / memory / model size
```

Do not compress without measuring resulting behavior.

## Train-Inference Consistency

Ensure identical or equivalent handling of:

* resizing;
* color channels;
* normalization;
* cropping;
* tensor layout.

Many production vision failures originate in preprocessing differences.

## Reproducibility

Track when relevant:

* dataset version;
* split;
* annotations;
* preprocessing;
* augmentation;
* model weights;
* architecture;
* framework version;
* random seed.

## Privacy

Images may expose:

* faces;
* documents;
* screens;
* addresses;
* identifying information.

Apply appropriate access control and retention policies.

Do not store or log sensitive imagery unnecessarily.

## Multimodal Systems

When combining images with text or structured data, inspect:

* modality alignment;
* missing modalities;
* timestamp alignment;
* representation compatibility.

Use `nlp-engineer` for text-specific processing and `llm-evaluation` when a
generative multimodal model produces language outputs.

## When Other Skills Should Be Combined

Examples:

* `computer-vision-engineer` + `eda-specialist` for image dataset exploration;
* `computer-vision-engineer` + `feature-engineering` for visual
  representations;
* `computer-vision-engineer` + `model-evaluator` for predictive evaluation;
* `computer-vision-engineer` + `experiment-designer` for architecture
  comparisons;
* `computer-vision-engineer` + `ml-engineer` for production vision systems;
* `computer-vision-engineer` + `debugging` for unexpected model behavior;
* `computer-vision-engineer` + `statistician` for uncertainty analysis;
* `computer-vision-engineer` + `nlp-engineer` for multimodal systems.

Do not invoke additional skills unless they materially improve the task.

## Required Output

For substantial computer vision work, report:

### Task

The visual problem being solved.

### Data

Dataset structure, image characteristics, annotations, and important risks.

### Baseline

The simplest meaningful approach.

### Preprocessing

Required image transformations and assumptions.

### Model

Recommended modeling approach.

### Evaluation

Metrics, slices, and validation strategy.

### Failure Modes

Likely or observed vision-specific weaknesses.

### Implementation

Practical approach when requested.

### Risks

Leakage, annotation quality, domain shift, compute, privacy, or deployment
limitations.

For simple vision questions, keep output proportional to the request.

## Rules

* Define the vision task before selecting a model.
* Understand how images were collected and labeled.
* Check exact and near duplicates.
* Prevent related images or frames from leaking across splits.
* Inspect annotation quality.
* Do not use augmentations without verifying label preservation.
* Keep training and inference preprocessing consistent.
* Match metrics to the vision task.
* Do not trust aggregate metrics alone.
* Evaluate meaningful visual slices.
* Investigate shortcut learning.
* Do not assume larger models are always better.
* Consider image resolution as a quality-cost tradeoff.
* Measure the complete inference pipeline.
* Protect sensitive imagery.
* Prefer the simplest approach that satisfies the requirements.
