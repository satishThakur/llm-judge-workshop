# LLM-as-Judge Workshop

A hands-on workshop on building and correcting LLM judges for automated evaluation.

## What you'll build

1. **An LLM judge** — prompt an LLM to evaluate bot responses against a dietary restriction
2. **Iterative prompt refinement** — diagnose false positives/negatives and improve the judge
3. **Bias correction** — use [`judgy`](https://github.com/mozilla-ai/judgy) to correct for judge TPR/TNR errors
4. **Bayesian model** — full posterior over the true success rate, propagating uncertainty in judge accuracy

## Setup

```bash
uv sync
cp .env.example .env   # add your API key
source .venv/bin/activate
jupyter notebook llm_judge_evals.ipynb
```

## Data

`data/labeled_traces.jsonl` — 101 human-labeled conversation traces (recipe bot responses with PASS/FAIL labels)

`data/raw_traces.jsonl` — unlabeled traces for running the judge at scale

`data/trace_viewer.html` — open in browser to inspect individual traces

Each trace has:
```json
{
  "dietary_restriction": "vegan",
  "query": "user message...",
  "response": "bot response...",
  "label": "PASS",
  "reasoning": "human reasoning..."
}
```

## Key concepts

- **TPR (True Positive Rate)**: how often the judge correctly identifies a passing response
- **TNR (True Negative Rate)**: how often the judge correctly identifies a failing response
- **Judge bias**: if TPR/TNR < 1, the raw observed pass rate is misleading — correction is needed
- **Bayesian correction**: models uncertainty in both the true pass rate *and* the judge accuracy
