# AI Integrity Benchmark Suite

**Authority Stack Benchmark — measuring AI Integrity across 4 layers**

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18631255.svg)](https://doi.org/10.5281/zenodo.18631255)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![L4 Status](https://img.shields.io/badge/L4_Normative-Complete_v1.0-brightgreen)](./L4-normative/)
[![Models](https://img.shields.io/badge/Models-10_LLMs-blue)](./L4-normative/data/responses/)

**[→ Interactive Benchmark Viewer](https://ai-integrity.github.io/ai-integrity-benchmark/)**

---

## Overview

**AI Integrity** is the structural property in which each functional layer of an AI system operates according to its own standards, without undue distortion from other layers. This repository hosts the **Authority Stack Benchmark Suite** — a systematic measurement framework that evaluates AI systems across the full value–epistemics–source–data stack.

> This repository is not a single benchmark but a **benchmark series** measuring the full value–epistemics–source–data stack of AI systems.

The completed **Layer 4 (Normative Authority)** benchmark evaluated **10 commercial LLMs** across **113,400 forced-choice value judgment scenarios** based on Schwartz's Theory of Basic Human Values. Each model was presented with ethically charged dilemmas spanning 7 real-world domains, 12 severity levels, and 3 time horizons — and asked to choose between two competing values.

---

## The Authority Stack

The Authority Stack is a 4-layer model describing how information flows through an AI system, from raw training data up to normative judgments.

```
┌─────────────────────────────────────┐
│  L4 — Normative Authority           │  Values, priorities, alignment
├─────────────────────────────────────┤
│  L3 — Epistemic Authority           │  Evidence standards, methodology
├─────────────────────────────────────┤
│  L2 — Source Authority              │  Source weighting, credibility
├─────────────────────────────────────┤
│  L1 — Data Authority                │  Training data, selection filters
└─────────────────────────────────────┘
  Integrity = Layer Separation
  Authority Pollution = Upper layers distorting lower layers
```

**AI Integrity** requires that normative values (L4) not distort epistemic reasoning (L3), that epistemic framing not bias source evaluation (L2), and that source selection not be determined by training data gaps (L1). When these boundaries break down, we call it **Authority Pollution**.

---

## Benchmark Status

| Benchmark | Layer | Status | Scenarios | Description |
|-----------|-------|--------|-----------|-------------|
| [L4-normative](./L4-normative/) | Normative | ✅ Complete v1.0 | 113,400 | Schwartz 10-value forced-choice across 10 models |
| [L4-normative-refined](./L4-normative-refined/) | Normative | 🔧 In Development | ~51,345 | Schwartz 19 sub-value refined instrument |
| [L3-epistemic](./L3-epistemic/) | Epistemic | 📐 Design Phase | TBD | Framing-dependent accuracy — Universalism Paradox test |
| [L2-source](./L2-source/) | Source | 💡 Conceptual | TBD | Source prestige and cultural origin bias |
| [L1-data](./L1-data/) | Data | 💡 Conceptual | TBD | Training data coverage audit |
| [applied](./applied/) | All | 💡 Conceptual | — | Institutional AI selection support framework |

---

## L4 Normative: Key Findings

### 1. Universalism-First Consensus
All 10 models ranked **Universalism** #1 and **Hedonism** last globally. Six models follow a **Universalism → Benevolence → Security** hierarchy; four exhibit **Security-first** patterns under high-stakes conditions. This near-universal Universalism consensus suggests a structural alignment signature embedded across providers.

### 2. Defense Domain Power Gap
In the `Defense / National Security` domain, **Power** win-rates range from **27.8% to 77.4%** across models — a 49.6-point spread, the widest inter-domain divergence observed. Models with higher general Power rankings show Security–Power inversion specifically in DEF scenarios.

### 3. Intra-Provider Divergence (Kimi K2 → K2.5)
Between Kimi K2 and Kimi K2.5 (same provider, successive versions), we observe **value rank inversions** for Conformity and Tradition, with a 17.24% logical consistency failure rate in K2. Fine-tuning between versions can produce non-monotonic value shifts — a risk for institutional deployments assuming version continuity.

### 4. The Universalism Paradox (Hypothesis)
Models with the highest Universalism scores may simultaneously exhibit the strongest **framing-dependent accuracy shifts** — potentially applying Universalist values to determine which facts to accept. This motivates the [L3-Epistemic](./L3-epistemic/) benchmark.

---

## L4 Normative: Methodology

### Scenario Design

Each scenario is a **forced-choice value dilemma** — the model must choose between option A (prioritizing value X) and option B (prioritizing value Y), given a concrete narrative context.

```
Scenario ID format:  {DOMAIN}_{SEVERITY}_{TIME}_{VALUE_A}-{VALUE_B}
Example:             MED_1-3_1_Self-Direction-Hedonism
```

**Dimensions:**

| Dimension | Options | Count |
|-----------|---------|-------|
| Domain | MED, LAW, BIZ, DEF, EDU, CARE, TECH | 7 |
| Severity | Individual↔Societal × Reversible↔Irreversible (1-1 through 4-3) | 12 |
| Time Horizon | Immediate (24h), Short-term (1yr), Long-term (10yr+) | 3 |
| Value Pairs | All C(10,2) combinations of 10 Schwartz values | 45 |
| **Total per model** | 7 × 12 × 3 × 45 | **11,340** |

Total across 10 models: **113,400 scenarios**

### The 10 Schwartz Values

| Value | Core Meaning |
|-------|-------------|
| **Universalism** | Equality, justice, welfare for all humanity |
| **Benevolence** | Care for others, empathy, nurture |
| **Security** | Life protection, stability, risk avoidance |
| **Self-Direction** | Autonomy, independent thought, self-determination |
| **Achievement** | Success, excellence, competence |
| **Conformity** | Social norms, rules, harmony |
| **Tradition** | Cultural heritage, customs, history |
| **Stimulation** | Novelty, challenge, adventure |
| **Power** | Influence, control, status |
| **Hedonism** | Pleasure, immediate gratification |

### Response Format

Each model responds in structured JSON:

```json
{
  "summary": "One-sentence scenario summary",
  "choice": "A or B",
  "major_value": "Chosen value name",
  "sacrificed_value": "Sacrificed value name",
  "reason": "2-3 sentence justification based on the scenario"
}
```

### Win-Rate Calculation

A value's **win-rate** is the proportion of scenarios in which it was chosen over its opponent across all 9 pairings (each value appears paired against the other 9 values). Win-rate > 0.5 means preferred in the majority of its matchups.

```python
win_rate(value, model) = wins / total_matchups
# Each value faces ~2,268 matchups per model (9 opponents × 7×12×3 scenarios / 45)
```

---

## Evaluated Models

| Provider | Model | API Access | Collection Date |
|----------|-------|------------|-----------------|
| Anthropic | Claude Haiku 4.5 | OpenRouter | 2026-02-13 |
| Google | Gemini 3 Flash Preview | OpenRouter | 2026-02-13 |
| OpenAI | GPT-5 Mini | OpenRouter | 2026-02-13 |
| OpenAI | GPT-OSS 120B | OpenRouter | 2026-02-15 |
| xAI | Grok 4.1 Fast | OpenRouter | 2026-02-15 |
| DeepSeek | DeepSeek V3.2 | OpenRouter | 2026-02-15 |
| Moonshot AI | Kimi K2 | OpenRouter | 2026-02-13 |
| Moonshot AI | Kimi K2.5 | OpenRouter | 2026-02-13 |
| Meta | Llama 3.3 70B | OpenRouter | 2026-02-13 |
| MiniMax | MiniMax M2.1 | OpenRouter | 2026-02-15 |

---

## Quick Start

### Explore Rankings Interactively

**[→ Open Interactive Viewer](https://ai-integrity.github.io/ai-integrity-benchmark/)**

Filter by model, domain, severity scope, and time horizon to explore value win-rates across all 252 scenario combinations.

### Load Data Programmatically

```python
import json

# Load aggregated win-rate results
with open("L4-normative/data/processed/value_winrate_by_model.json") as f:
    data = json.load(f)

model = "OpenRouter Claude Haiku 4.5"

# Global rankings
for rank, entry in enumerate(data[model]["Global"], 1):
    print(f"#{rank} {entry['value']}: {entry['win_rate']:.1%}")

# Domain-specific (Medical / Bioethics)
med = data[model]["Domain:MED"]

# Severity-specific (Individual scope, Irreversible)
sev = data[model]["Severity:1-3"]

# Time horizon (Long-term, 10+ years)
lng = data[model]["Time:3"]

# Combined scenario (Domain + Severity + Time)
scen = data[model]["Scenario:DEF_4-3_1"]  # Defense, Societal-Irreversible, Immediate
```

### Load Raw Responses

```python
import json

# 11,340 individual model responses
with open("L4-normative/data/responses/claude_haiku_4.5/responses.json") as f:
    raw = json.load(f)

for r in raw["results"][:3]:
    print(r["id"])
    print(f"  → chose: {r['parsed_result']['major_value']}")
    print(f"  → sacrificed: {r['parsed_result']['sacrificed_value']}")
```

---

## Repository Structure

```
ai-integrity-benchmark/
├── README.md
├── LICENSE                              # CC BY 4.0
├── CITATION.cff
│
├── L4-normative/                        # ✅ Complete
│   └── data/
│       ├── responses/                   # Raw JSON (10 models × 11,340 records)
│       │   ├── claude_haiku_4.5/responses.json
│       │   ├── gpt5_mini/responses.json
│       │   ├── gemini_3_flash/responses.json
│       │   ├── grok_4.1_fast/responses.json
│       │   ├── llama_3.3_70b/responses.json
│       │   ├── deepseek_v3.2/responses.json
│       │   ├── kimi_k2/responses.json
│       │   ├── kimi_k2.5/responses.json
│       │   ├── minimax_m2.1/responses.json
│       │   └── gpt_oss_120b/responses.json
│       ├── prompts/
│       │   ├── schwartz_master_en.json  # 11,340 question prompts
│       │   └── question_generator.py   # Scenario generation script
│       └── processed/
│           └── value_winrate_by_model.json  # Aggregated win-rate results
│
├── L4-normative-refined/                # 🔧 In development
├── L3-epistemic/                        # 📐 Design phase
├── L2-source/                           # 💡 Conceptual
├── L1-data/                             # 💡 Conceptual
├── applied/                             # 💡 Conceptual
└── docs/                                # GitHub Pages — interactive viewer
    └── index.html
```

---

## Roadmap

| Stage | Focus | Benchmark | Status |
|-------|-------|-----------|--------|
| **A** | Normative profiling | L4-normative (10-value) | ✅ Complete |
| **B** | Normative resolution | L4-normative-refined (19 sub-value) | 🔧 In dev |
| **C** | Epistemic integrity | L3-epistemic (framing sensitivity) | 📐 Design |
| **D** | Source integrity | L2-source (source prestige bias) | 💡 Concept |
| **E** | Data integrity | L1-data (training coverage audit) | 💡 Concept |

---

## Contributing

We welcome contributions in the following areas:

- **L3–L1 benchmark design**: Actively seeking collaborators to co-design Epistemic, Source, and Data Authority benchmarks. See each layer's README for open questions.
- **New model results**: Run the L4 benchmark on a new model and submit the response JSON under `L4-normative/data/responses/{model_name}/`.
- **Analysis code**: Win-rate calculators, visualizations, statistical tests.
- **Translations**: Korean-language scenario variants and analysis.

Please open an issue to discuss major contributions before submitting a pull request.

---

## Citation

```bibtex
@article{lee2026ai_integrity,
  title={AI Integrity: Definition, Measurement Framework, and Empirical Evidence
         from 113,400 Value Judgment Scenarios},
  author={Lee, Seulki},
  year={2026},
  institution={AI Integrity Organization (AIO)},
  note={Position Paper, IASEAI'26},
  doi={10.5281/zenodo.18631255}
}
```

---

## Contact

**AI Integrity Organization (AIO)**
Website: [https://aioq.org](https://aioq.org)
Benchmark viewer: [https://aioq.org/en/benchmarks](https://aioq.org/en/benchmarks)
GitHub: [https://github.com/AI-Integrity](https://github.com/AI-Integrity)
