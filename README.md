# SCODEL: Shared Canonical Ontology Definitions for Food Entity Linking and Ontology-Grounded Recipe Knowledge Graphs

**SCODEL** is a model-agnostic semantic normalisation framework for contextual food entity linking. It learns shared canonical ontology-oriented definitions for recipe food mentions and ontology concepts, improving entity linking while supporting ontology-grounded recipe knowledge graph construction.

> **Paper:** *SCODEL: Shared Canonical Ontology Definitions for Food Entity Linking and Ontology-Grounded Recipe Knowledge Graphs*  
> **Authors:** Nuhu Ibrahim, Rishi Ravikumar, Ayuba Dauda, Fuqi Xu, Robert Stevens, and Riza Batista-Navarro  
> **Affiliation:** Department of Computer Science, The University of Manchester, UK

## Resources

GitHub hosts the implementation, documentation, evaluation summaries, and examples. Larger research artefacts are hosted on Hugging Face.

| Resource | Description | Link |
|---|---|---|
| Paper | Full methodology and experimental analysis | Coming soon |
| Code | SCODEL implementation and evaluation scripts | This repository |
| ReciLinkD | Contextual recipe food entity linking benchmark | Hugging Face — coming soon |
| Trained models | Canonical definition generation models and released linking models | Hugging Face — coming soon |
| Entity linking outputs | Predictions and evaluation artefacts | Hugging Face — coming soon |
| Recipe knowledge graph | Ontology-grounded graph and associated results | Hugging Face — coming soon |

## Overview

Recipe language is often ambiguous, context-dependent, and lexically different from ontology terminology. For example, a recipe might mention **“parm”** where the corresponding ontology concept is **parmesan cheese**. SCODEL reduces this semantic gap by learning to generate a *shared canonical definition* from either:

- a food mention and its surrounding recipe context; or
- an ontology concept's label, synonyms, and taxonomic information.

These definitions augment the original inputs to existing entity linking models, rather than replacing their architectures.

### How SCODEL works

1. **Shared canonical definition learning:** Train a generator using aligned food mention and ontology concept pairs with a common manually curated ontology-oriented definition as the target.
2. **Definition generation:** Generate definitions independently for contextual mentions and ontology concepts, including concepts not encountered during training.
3. **Entity linking:** Supply the generated definitions alongside the original representations to an existing entity linking model for candidate retrieval and ranking.
4. **Knowledge graph construction:** Use linked food mentions to ground recipe information in ontology concepts.

SCODEL is evaluated with **ReFinED**, **GSM-EL**, and **BLINK**, and uses **FoodOn** as the food ontology in the recipe linking experiments.

## ReciLinkD: Contextual Food Entity Linking Benchmark

**ReciLinkD** is a benchmark of recipe-derived food mentions aligned to FoodOn concepts.

| Dataset characteristic | Value |
|---|---:|
| Manually annotated food mention and ontology concept pairs | 3,979 |
| Manually verified pairs after SCODEL-assisted expansion | 15,559 |
| Distinct FoodOn concepts represented in the initial manual annotations | 979 |
| FoodOn candidate concepts after filtering and deduplication | 1,014 |

The release will include dataset documentation, available splits, and download instructions on Hugging Face.

**Dataset:** Hugging Face link coming soon.

## Entity Linking Results

We evaluate models under two settings:

- **Seen (supervised):** Test ontology concepts are represented during training.
- **Unseen (zero-shot):** Gold test concepts are held out from training the definition generator and entity linking model, but remain available in the candidate ontology at inference.

### Main benchmark results

All values are percentages except where otherwise indicated. **Hit@1** measures whether the top-ranked ontology concept is correct.

| Method | Seen Hit@1 | Seen Hit@5 | Seen MRR | Unseen Hit@1 | Unseen Hit@5 | Unseen MRR | Avg. Hit@1 |
|---|---:|---:|---:|---:|---:|---:|---:|
| Lexical Matching | 52.66 | 73.93 | 61.62 | 56.79 | 75.27 | 65.81 | 54.73 |
| LLM4EL | 63.29 | 88.33 | 73.90 | 63.72 | 90.21 | 74.92 | 63.51 |
| LLMAEL | 85.25 | 94.51 | 89.61 | 80.71 | 94.70 | 86.47 | 82.98 |
| LLM Rerank@10 | 82.68 | 89.88 | 85.97 | 83.02 | 92.80 | 87.19 | 82.85 |
| LLM Rerank@50 | 81.13 | 88.51 | 84.52 | 80.43 | 90.35 | 84.87 | 80.78 |
| ReFinED | 81.82 | 89.71 | 85.42 | 78.40 | 92.66 | 84.38 | 80.11 |
| GSM-EL | 76.84 | 90.56 | 82.62 | 82.33 | 93.75 | 87.48 | 79.59 |
| BLINK | 87.47 | 93.65 | 90.31 | 71.19 | 87.09 | 77.90 | 79.33 |
| **SCODEL × ReFinED** | 85.08 | 93.31 | 89.11 | 82.07 | 93.75 | 87.38 | **83.58** |
| **SCODEL × GSM-EL** | 80.78 | 92.62 | 86.24 | **83.15** | **94.83** | **88.38** | 81.97 |
| **SCODEL × BLINK** | **88.33** | **94.68** | **90.95** | 75.00 | 88.72 | 81.32 | 81.67 |

**Highlights:** SCODEL improves all three evaluated modular entity linking models over their corresponding unaugmented versions. SCODEL × BLINK achieves the highest seen Hit@1 (**88.33%**), SCODEL × GSM-EL achieves the highest unseen Hit@1 (**83.15%**), and SCODEL × ReFinED has the highest average Hit@1 (**83.58%**).

The paper also reports hierarchy-aware accuracy (H-Acc), ablation studies, and evaluation on a biomedical ontology benchmark.

**Full predictions and evaluation artefacts:** Hugging Face link coming soon.

## Entity Linking Example

The following is an **illustrative example of the task**, not a claim about a specific model prediction:

| Field | Example |
|---|---|
| Recipe context | “Sprinkle freshly grated parm over the pasta before serving.” |
| Food mention | `parm` |
| Corresponding ontology concept | Parmesan cheese |
| FoodOn identifier | `FOODON_03303675` |
| SCODEL enhancement | Generate an ontology-oriented definition to augment mention and concept representations |

## Ontology-Grounded Recipe Knowledge Graph

SCODEL is applied to ground recipe food mentions in **FoodOn** and construct an ontology-grounded recipe knowledge graph. This supports ontology-aware recipe retrieval and corpus-level food analysis.

### Knowledge graph release

| Artefact | Availability |
|---|---|
| Ontology-grounded recipe knowledge graph | Hugging Face — coming soon |
| Recipe-to-ontology linking outputs | Hugging Face — coming soon |
| Graph schema and construction documentation | Coming soon |
| Knowledge graph statistics and downstream evaluation | Coming soon |

### Knowledge graph results

Detailed results will be added here, including the graph's scale, grounding coverage, and ontology-aware retrieval or corpus-analysis examples, once the final release statistics are available.

## Models

SCODEL investigates **Llama-3-8B**, **Mistral-7B**, and **Qwen2.5-7B** as backbones for canonical definition generation. The main entity linking results reported above use **Mistral-7B**, the strongest individual definition-learning backbone in the reported experiments.

Trained model artefacts and their usage instructions will be linked from Hugging Face as they become available.

## Citation

If you use SCODEL, ReciLinkD, or the released knowledge graph, please cite the associated paper. A verified BibTeX entry will be added when publication details are available.

## License

The research artefacts are intended for release under **Creative Commons Attribution–NonCommercial 4.0 International (CC BY-NC 4.0)**, subject to any third-party licensing obligations.

- [CC BY-NC 4.0 license](https://creativecommons.org/licenses/by-nc/4.0/)
- Individual Hugging Face repositories will identify the applicable license for their datasets, models, and knowledge graph artefacts.
- The source code license will be stated separately in this repository. CC BY-NC is not generally recommended as a software code license.

## Authors

Nuhu Ibrahim, Rishi Ravikumar, Ayuba Dauda, Fuqi Xu, Robert Stevens, and Riza Batista-Navarro  
Department of Computer Science, The University of Manchester, United Kingdom.
