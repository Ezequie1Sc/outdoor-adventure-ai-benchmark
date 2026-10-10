# Outdoor Adventure AI Benchmark

![Outdoor Adventure AI Benchmark](assets/hero.png)

<p align="center">
  <strong>Evaluating free AI models for structured outdoor adventure generation.</strong>
</p>

<p align="center">
  <a href="https://www.kaggle.com/code/ezequielsalazar1/outdoor-adventure-ai-benchmark">
    Kaggle Notebook
  </a>
  ·
  <a href="https://dev.to/challenges/kaggle-2026-09-23">
    Kaggle Benchmarking Challenge
  </a>
</p>

---

## Overview

**Outdoor Adventure AI Benchmark** evaluates how reliably free API-hosted language models generate structured outdoor adventures for [Ruta Viva AI](https://github.com/Ezequie1Sc).

The benchmark focuses on a practical engineering question:

> Which models can consistently generate valid, structured adventure data that an application can consume?

Rather than measuring response speed alone, this project evaluates JSON validity, required fields, mission count, XP consistency, and response latency.

## Research Goals

- Evaluate structured-output reliability across multiple AI models.
- Measure response latency and usable response rates.
- Validate generated adventure schemas and XP calculations.
- Identify common failure modes, including truncated responses and empty outputs.
- Establish a foundation for more comprehensive model evaluations.

## Models Tested

The pilot benchmark evaluated three models through OpenRouter:

| Model | Model ID |
|---|---|
| Liquid LFM 2.5 2.6B | `liquid/lfm-2.5-2.6b:free` |
| NVIDIA Nemotron 3.5 Lightning | `nvidia/nemotron-3.5-lightning:free` |
| Apodex 1.1 Mini | `apodex/apodex-1.1-mini:free` |

Model availability and free-tier limits may change over time.

## Benchmark Methodology

The benchmark defines **30 scenarios** across 10 outdoor environments, with three prompt variations per environment.

Example environments include:

- Urban parks
- Riverside paths
- Botanical gardens
- Historic towns
- Coastal promenades
- Forest trails
- City squares
- Lake paths
- Community gardens
- Neighborhoods

Each generated adventure is expected to contain:

- `title`
- `description`
- `duration_minutes`
- `missions`
- `total_xp`

The validation logic checks whether the response contains valid JSON, includes the required fields, contains exactly three missions, and has a `total_xp` value equal to the sum of mission XP.

A basic keyword-based safety heuristic is also included. It is intended only as an initial screening mechanism, not as proof of safety.

## Pilot Results

The initial pilot ran **three scenarios per model**, for a total of nine requests.

| Model | Valid JSON | Required fields | Exactly 3 missions | XP consistent | Median latency |
|---|---:|---:|---:|---:|---:|
| Liquid LFM 2.5 2.6B | 100% | 100% | 100% | 100% | 13.57 s |
| NVIDIA Nemotron 3.5 Lightning | 33.3% | 33.3% | 33.3% | 33.3% | 41.37 s |
| Apodex 1.1 Mini | 0% | 0% | 0% | 0% | 6.36 s |

*Results reflect this specific pilot run and are not general performance guarantees.*

### Key Findings

**1. Liquid showed the strongest structured-output reliability.**

All three pilot responses passed the JSON, required-field, mission-count, and XP-consistency checks.

**2. Nemotron encountered output truncation.**

Two of its three responses reached the output limit and did not produce valid JSON.

**3. Apodex was the fastest but least reliable in this pilot.**

Its median latency was approximately 6.36 seconds, but none of its three responses produced valid JSON. One response contained no usable answer.

### Main Takeaway

A successful HTTP response does not necessarily mean a successful AI task.

For applications that depend on structured data, output validity and schema compliance are essential. A fast response that cannot be parsed may be less useful than a slower response that meets the application's requirements.

## Limitations

This is an initial pilot, not a comprehensive model evaluation.

- Only three requests were tested per model.
- Results may vary with prompts, provider availability, and model updates.
- Latency measurements reflect a particular execution environment and time.
- Keyword-based safety checks cannot establish that an adventure is safe.
- Adventure quality, accessibility, relevance, and variety require additional evaluation, including human review.

## Future Work

Potential improvements include:

- Running all 30 scenarios against each model.
- Repeating scenarios to measure consistency.
- Adding schema-constrained generation where supported.
- Evaluating relevance, practicality, accessibility, and mission diversity.
- Conducting manual safety reviews.
- Comparing retry strategies and output-token limits.
- Recording model versions, execution dates, and provider failures.

## Tech Stack

- **Python** — benchmark implementation and evaluation logic
- **Pandas** — result analysis and summary tables
- **Requests** — API communication
- **python-dotenv** — local environment configuration
- **OpenRouter** — access to hosted language models
- **Jupyter Notebook** — interactive experiment and reporting
- **Kaggle** — public benchmark notebook

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Ezequie1Sc/outdoor-adventure-ai-benchmark.git
cd outdoor-adventure-ai-benchmark
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install requests pandas python-dotenv ipykernel
```

### 4. Configure your API key

Create a `.env` file in the directory containing the notebook:

```dotenv
OPENROUTER_API_KEY=your_api_key_here
```

**Security:** Never commit `.env`, publish API keys, or include credentials in notebook outputs. Ensure `.env` is included in `.gitignore`.

### 5. Run the notebook

Open `Outdoor_Adventure_AI_Benchmark.ipynb` in VS Code or Jupyter and execute the cells in order.

The notebook starts in pilot mode. Review the results and API limits before enabling the full benchmark. API calls require network access and a valid OpenRouter API key.

The notebook can export detailed results and summary metrics to CSV files.

## Reproducibility

For meaningful comparisons, record the exact model IDs, benchmark scenarios, run date, number of requests, latency measurements, and failures. Free model availability and provider behavior can change, so rerunning the benchmark may produce different results.

## Links

- **Public Kaggle notebook:** [Outdoor Adventure AI Benchmark](https://www.kaggle.com/code/ezequielsalazar1/outdoor-adventure-ai-benchmark)
- **GitHub repository:** [Ezequie1Sc/outdoor-adventure-ai-benchmark](https://github.com/Ezequie1Sc/outdoor-adventure-ai-benchmark)
- **DEV challenge:** [Kaggle Benchmarking Challenge](https://dev.to/challenges/kaggle-2026-09-23)

## Acknowledgements

Created as part of the DEV Community × Kaggle Benchmarking Challenge.

This project explores practical AI evaluation for Ruta Viva AI, an outdoor adventure application concept.

---

<p align="center">
  <strong>Measure reliability. Validate outputs. Build better AI experiences.</strong>
</p>