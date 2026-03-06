# LLM License Plate Moderation System

> **Production-grade GPT pipeline for automated license plate content classification — batch optimized, multilingual, MLflow tracked.**

Built in collaboration with a Canadian Company to automate the verification of license plates which is arried out with in every two weeks, based on the set of principles,. Classifies 1,000+ license plates across 7 violation categories with systematic batch optimization, NLI-based contradiction detection, and full experiment tracking on Databricks.

---

## Key Results

| Metric | Value |
|--------|-------|
| Peak throughput | **10.2 plates/sec** (batch=100) |
| Optimal cost | **$0.027 per 1,000 plates** (batch=300–400) |
| Languages analyzed | **15** |
| Batch configurations tested | **9** (100–900 plates/batch) |
| Contradiction detection | **24 inconsistent judgments** identified in 99-record dataset |

---

## What It Does

A complete LLM-powered moderation pipeline for government vehicle registration authorities. The system classifies license plates into one of 7 violation categories or marks them as acceptable.

### Violation Categories
| Category | Examples |
|----------|---------|
| `HATE_SPEECH` | Racial slurs, extremist symbols, discrimination |
| `SEXUAL` | Sexual references, innuendo |
| `DRUG_ALCOHOL` | Drug/alcohol references |
| `POLITICAL` | Political figures, parties, slogans |
| `OBSCENE` | Abusive or offensive language |
| `RELIGION` | Religious offense or promotion |
| `UNCLEAR` | Cannot be verified by law enforcement |

---

## System Architecture

```
Input Plates (CSV/batch)
        │
        ▼
┌─────────────────────┐
│   Batch Optimizer   │  ← 9 configs tested (100–900 plates)
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│   GPT-4o-mini API   │  ← Prompt-engineered classification
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  MLflow on Databricks│  ← Latency, tokens, cost per experiment
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  NLI Contradiction   │  ← sentence-transformers cross-check
│  Detector            │
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Bias Analysis       │  ← 15 languages, Cohen's Kappa, fairness
└─────────────────────┘
```

---

## Tech Stack

- **LLM**: OpenAI GPT-4o-mini
- **Experiment Tracking**: MLflow 3.x on Databricks
- **NLI / Contradiction Detection**: `sentence-transformers`, cross-encoder models
- **Bias Analysis**: Cohen's Kappa, demographic fairness metrics
- **Visualization**: Seaborn, Plotly
- **Data**: pandas, Python

---

## Quick Start

```bash
git clone https://github.com/bushramaryam/llm-license-plate-moderation
cd llm-license-plate-moderation
pip install openai mlflow sentence-transformers pandas seaborn plotly openpyxl
```

Set your API key:
```bash
export OPENAI_API_KEY="sk-..."
```

Open `notebooks/VehicleLicensePlateOptimalCaseV2.ipynb` and run all cells.

> **Note**: MLflow experiment tracking requires a Databricks workspace or local MLflow server. Set `MLFLOW_TRACKING_URI` accordingly.

---

## Batch Optimization Findings

| Batch Size | Throughput (plates/sec) | Cost per 1K plates |
|------------|------------------------|--------------------|
| 100 | **10.2** (peak) | $0.031 |
| 200 | 7.8 | $0.029 |
| 300 | 6.1 | **$0.027** (optimal) |
| 400 | 5.4 | **$0.027** (optimal) |
| 500–900 | 3.2–4.8 | $0.028–0.031 |

**Recommendation**: Use batch=100 for real-time systems, batch=300–400 for cost-sensitive batch jobs.

---

## Multilingual Bias Analysis

Analyzed classification consistency across 15 languages including Arabic, Urdu, French, Spanish, German, Chinese, Japanese, Hindi, Portuguese, Russian, Turkish, Korean, Italian, Dutch, and Polish.

Used Cohen's Kappa to measure inter-rater agreement between the LLM's judgments on equivalent plates across languages. Results inform fairness and consistency of the moderation system.

---

## Author

**Bushra Maryam** — AI & Backend Engineer  
[LinkedIn](https://www.linkedin.com/in/bushramaryam) · [GitHub](https://github.com/bushramaryam) · [Portfolio](https://bushramaryam.github.io)
