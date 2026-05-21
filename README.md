# Critical Question Generation for Arguments

This repository contains prompts and experimental results for the paper:

> **Critical Question Generation for Arguments: A Large-Scale Analysis of Prompting and Reranking Strategies**  

## Overview

This paper presents a large-scale empirical study of Critical Question Generation (CQG), examining 42 language models across 18 prompting strategies, supervised fine-tuning, and a generate-then-select approach on the [CQs-Gen benchmark](https://hitz-zentroa.github.io/shared-task-critical-questions-generation/).

Key findings:
- Scheme-aware prompting **degrades** performance in 23 of 34 open-weight models (mean −0.0415), with a directionally consistent but weaker effect in proprietary systems (6 of 8; mean −0.0061).
- Fine-tuned Gemma 2 2B (0.59) surpasses zero-shot Gemma 2 27B (0.51) at one-thirteenth of the parameter count.
- A generate-then-select architecture with a fine-tuned ModernBERT reranker achieves **82.4% usefulness**, surpassing the ArgMining 2025 shared-task winner (67.6%) by 14.8 percentage points.

## Repository Contents

```
.
├── prompts.md                      # All 18 prompt templates with taxonomy
├── results/
│   ├── prompting_results.csv       # Top-15 model configurations (test set, averaged over 18 prompts)
│   ├── finetuning_results.csv      # Base vs. QLoRA fine-tuned results for 8 models
│   └── reranking_results.csv       # Top-10 encoder-based and LLM-judge configurations
└── README.md
```

## Results Files

### `results/prompting_results.csv`

Complete results for all 42 models on the CQs-Gen test set, ranked by average usefulness score across all 18 prompts. Open-weight models (34) appear twice — once for greedy decoding ($T=0$) and once for default sampling; proprietary models (8) appear once with provider default settings. 76 rows total.

| Column | Description |
|--------|-------------|
| `model` | Model identifier |
| `configuration` | Decoding configuration (e.g., `Greedy (T=0)`, `API Default`) |
| `param_type` | `open-weight` or `proprietary` |
| `usefulness_score_avg` | Mean usefulness score across all 18 prompts (0–1) |
| `usefulness_score_best` | Best single-prompt usefulness score (0–1) |
| `nae_pct` | Average percentage of Not-Able-to-Evaluate outputs across prompts |

### `results/finetuning_results.csv`

Comparison of best prompt-based scores versus QLoRA fine-tuned scores for 8 open-weight models on the CQs-Gen test set (102 interventions × 3 questions = 306 questions per model).

| Column | Description |
|--------|-------------|
| `model` | Model family |
| `size` | Parameter count |
| `base_score` | Best prompt-based usefulness score |
| `sft_score` | Usefulness score after QLoRA fine-tuning on Useful-labeled examples |
| `delta_score` | Change in usefulness score after fine-tuning |
| `delta_invalid` | Absolute change in Invalid question count across all 102 interventions |
| `delta_useful` | Absolute change in Useful question count across all 102 interventions |

### `results/reranking_results.csv`

Top-10 configurations for encoder-based reranking (ModernBERT) and LLM-based judge reranking (Gemini 2.5 Pro) on the CQs-Gen test set. Each row is one (generator pair, prompt, reranker) combination.

| Column | Description |
|--------|-------------|
| `reranker` | Selection mechanism: `ModernBERT` or `Gemini 2.5 Pro (LLM judge)` |
| `model_combination` | Two generator models separated by `+` |
| `prompt` | Prompt ID used for generation (P01–P18, matching `prompts.md`) |
| `usefulness_score` | Usefulness score on the test set (0–1) |
| `useful_pct` | Percentage of selected questions labeled Useful |
| `invalid_pct` | Percentage of selected questions labeled Invalid |
| `nae_pct` | Percentage of selected questions labeled Not-Able-to-Evaluate |

## Prompts

See [`prompts.md`](prompts.md) for all 18 prompt templates. The placeholder `{text}` is replaced at inference time with the argumentative intervention. Scheme-aware prompts (P10, P12, P18) additionally use `{scheme_details}`, `{argumentation_scheme}`, and `{scheme_examples}` placeholders filled from the CQs-Gen annotation.

## Benchmark

All experiments use the [CQs-Gen benchmark](https://hitz-zentroa.github.io/shared-task-critical-questions-generation/) introduced by the ArgMining 2025 shared task. The evaluation metric is the proportion of generated questions rated **Useful** by the shared-task annotation protocol.


## License

The prompt templates and result files are released for research use.
The CQs-Gen benchmark data is subject to the terms of the ArgMining 2025 shared task.
