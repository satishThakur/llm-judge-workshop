# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
uv sync                        # install dependencies
source .venv/bin/activate
jupyter notebook llm_judge_evals.ipynb
```

## Structure

- `llm_judge_evals.ipynb` — main workshop notebook (run top to bottom)
- `data/labeled_traces.jsonl` — 101 human-labeled traces, stratified train/dev/test split inside notebook
- `data/raw_traces.jsonl` — unlabeled traces for scale evaluation
- `data/trace_viewer.html` — standalone browser-based trace inspector

## Notebook flow

1. Load labeled traces, stratified split (train=15, dev=40, test=46)
2. Judge infrastructure: `run_judge(prompt, trace)`, `evaluate(split, name, prompt)`, `show_errors(results, version)`
3. Judge v1/v2 — prompts defined, results hardcoded (cached); v3 runs live on dev
4. Test set evaluation — runs live, produces `tp_test, fn_test, tn_test, fp_test, test_tpr, test_tnr`
5. Scale run — judge v3 on 200 raw traces → `judge_labels`, `p_obs`
6. judgy correction — `judgy.estimate_success_rate(test_labels, test_preds, unlabeled_preds)`
7. Bayesian grid (200³) — joint posterior over θ, TPR, TNR; marginal posterior predictive

## Model config

Set `MODEL_NAME_JUDGE` in `.env`. Default is `openai/gpt-4.1-nano` (cheap). LiteLLM is used so any provider works.

## judgy API

```python
theta_hat, lower, upper = judgy.estimate_success_rate(
    test_labels=...,      # list of 0/1 human labels
    test_preds=...,       # list of 0/1 judge predictions
    unlabeled_preds=...,  # list of 0/1 judge predictions on new data
)
```
