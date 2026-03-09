# LLM License Plate Moderation Pipeline

> **Production-grade GPT pipeline for automated license plate content classification — batch optimized, multilingual, MLflow tracked.**

Built in collaboration with a Canadian Company to automate the verification of license plates which is arried out with in every two weeks, based on the set of principles.Automatically classifies license plate requests as **Approve** or **Reject** across 7 violation categories. Systematically tests 5 batch configurations to find the optimal cost-throughput tradeoff, with every experiment logged to MLflow.

---

## Key Results

| Metric | Value |
|--------|-------|
| Peak throughput | **10.2 plates / sec** (batch size = 100) |
| Optimal cost | **$0.027 per 1,000 plates** (batch size = 300–400) |
| Batch sizes tested | **5** (100, 200, 400, 600, 800) |
| Total plates generated & classified | **1,000+** |
| Errors across all runs | **0** |

---

## What It Does

1. **Generates synthetic license plates** — realistic mix of normal, offensive, religious, political, drug-related, and unclear plates
2. **Classifies each plate** via GPT using a structured prompt with 7 violation categories
3. **Tests 5 batch sizes** — measures latency, throughput, and cost per configuration
4. **Logs every experiment to MLflow** on Databricks — latency, token usage, cost, batch metadata
5. **Visualizes results** — cost curves, throughput variance, optimal operating point

---

## Violation Categories

| Code | Category | Examples |
|------|----------|---------|
| `HATE_SPEECH` | Racial slurs, extremist content | — |
| `SEXUAL` | Sexual references or innuendo | — |
| `DRUG_ALCOHOL` | Drug or alcohol references | — |
| `POLITICAL` | Political figures, parties, slogans | — |
| `OBSCENE` | Abusive or offensive language | — |
| `RELIGION` | Religious offense or promotion | — |
| `UNCLEAR` | Cannot be verified by enforcement | — |
| — | **APPROVE** | Compliant plate | 

---


## Pipeline Flow

```
generate_license_plates(n=1000)
         │
         ▼
  Split into batches (100 / 200 / 400 / 600 / 800)
         │
         ▼
  GPT-4o-mini classification per batch
  → Approve / Reject + violation category
         │
         ▼
  MLflow experiment logging
  → latency, tokens, cost, batch_size, results
         │
         ▼
  Visualization
  → cost curve, throughput variance, optimal batch recommendation
```

---

## Batch Optimization Findings

| Batch Size | Throughput (plates/sec) | Cost per 1K plates | Recommendation |
|------------|------------------------|--------------------|----------------|
| 100 | **10.2** ← peak | $0.031 | Real-time / low-latency systems |
| 200 | 7.8 | $0.029 | Balanced |
| 400 | 5.4 | **$0.027** ← optimal | Cost-sensitive batch jobs |
| 600 | 4.1 | $0.028 | — |
| 800 | 3.2 | $0.030 | — |

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| OpenAI GPT-4o-mini | License plate classification |
| MLflow on Databricks | Experiment tracking |
| Python `random` + `string` | Plate generation |
| `pandas`, `seaborn`, `plotly` | Analysis & visualization |
| `time` | Latency measurement |

---

## Setup

```bash
git clone https://github.com/bushramaryam/llm-license-plate-moderation
cd llm-license-plate-moderation
pip install openai mlflow pandas seaborn plotly
```

Set your API key:
```python
openai.api_key = "sk-..."
```

Set your MLflow tracking URI (Databricks workspace or local):
```python
mlflow.set_tracking_uri("databricks")  # or "http://localhost:5000"
```

Open `notebooks/VehicleLicensePlateOptimalCaseV2.ipynb` and run all cells.

---

## Related

**Evaluation repo**: [`llm-license-plate-evaluation`](https://github.com/BushraMaryam/LLM-License-Plate-Evaluation-Framework) — NLI contradiction detection, bias analysis, and visual evaluation of the classification results produced by this pipeline.

---

## Author

**Bushra Maryam** — AI & Backend Engineer  
[LinkedIn](https://www.linkedin.com/in/bushramaryam) · [GitHub](https://github.com/bushramaryam) · [Portfolio](https://bushramaryam.github.io)
