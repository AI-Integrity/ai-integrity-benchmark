# AI Integrity Benchmark — L4 Normative (Data Archive)

**Layer 4 Value Priority Measurement: 113,400 Forced-Choice Responses Across 10 AI Models**

[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.18631255-blue)](https://doi.org/10.5281/zenodo.18631255)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

> **Note:** This repository is a **data archive** containing analysis results from the L4 Normative benchmark (10 Schwartz values, 10 models). The active benchmark — expanded to 3 layers with 19 sub-values — is the **[AIO PRISM Benchmark](https://github.com/AI-Integrity/aio-prism-benchmark)**.

---

## Overview

This repository contains the complete dataset and analysis results from the first large-scale empirical measurement of AI value priorities. Using Schwartz's 10 Basic Human Values as the measurement framework, 10 commercial LLMs were evaluated across 11,340 forced-choice value judgment scenarios each, yielding 113,400 total responses.

**[→ Interactive Viewer](https://ai-integrity.github.io/ai-integrity-benchmark/)**

---

## Key Findings

### 1. Universalism–Security Split

Six models place Universalism first; four place Security first. All 10 models rank both values in the top 3, and all rank Hedonism last or near-last.

| Group | Models | Top Value |
|---|---|---|
| Universalism-first | Claude Haiku 4.5, Llama 3.3 70B, Gemini 3 Flash, Kimi K2, MiniMax M2.1, GPT-OSS 120B | Universalism (87.8%–99.1%) |
| Security-first | GPT-5 Mini, Grok 4.1 Fast, DeepSeek V3.2, Kimi K2.5 | Security (87.7%–93.6%) |

### 2. Defense Domain Power Gap

In Defense/National Security scenarios, Power win-rates range from **27.8% (Claude)** to **77.4% (Grok)** — a 49.6-point spread, the widest inter-model divergence observed. All 10 models elevate Security to #1 in defense contexts.

### 3. Universal Hedonism Suppression

All 10 models suppress Hedonism below 10% win-rate (range: 0.6%–9.8%) — the strongest cross-model consensus in the dataset.

### 4. Intra-Provider Divergence

Same-provider model pairs show notable value divergence:
- Moonshot AI: Kimi K2 (Universalism-first) vs. Kimi K2.5 (Security-first)
- OpenAI: GPT-5 Mini (Security-first) vs. GPT-OSS 120B (Universalism-first)

---

## Benchmark Design

| Dimension | Values |
|---|---|
| **Value framework** | Schwartz 10 Basic Human Values |
| **Value pairs** | C(10, 2) = 45 |
| **Domains** | MED, LAW, BIZ, DEF, EDU, CARE, TECH (7) |
| **Severity levels** | 4 impact scopes × 3 reversibility levels (12) |
| **Decision timeframes** | Immediate, Short-term, Long-term (3) |
| **Scenarios per model** | 45 × 7 × 12 × 3 = **11,340** |
| **Models evaluated** | 10 |
| **Total responses** | **113,400** |

### Evaluated Models

| Provider | Model | Collection Date |
|---|---|---|
| Anthropic | Claude Haiku 4.5 | 2026-01-15 |
| Meta | Llama 3.3 70B | 2026-01-15 |
| OpenAI | GPT-5 Mini | 2026-01-16 |
| Google | Gemini 3 Flash Preview | 2026-01-16 |
| Moonshot AI | Kimi K2 | 2026-01-17 |
| Moonshot AI | Kimi K2.5 | 2026-01-17 |
| xAI | Grok 4.1 Fast | 2026-01-18 |
| DeepSeek | DeepSeek V3.2 | 2026-01-18 |
| MiniMax | MiniMax M2.1 | 2026-01-19 |
| OpenAI | GPT-OSS 120B | 2026-01-19 |

All models accessed via OpenRouter at temperature 1.0.

---

## Data Structure

```
ai-integrity-benchmark/
├── README.md
├── LICENSE                              # CC BY 4.0
├── CITATION.cff
│
├── L4-normative/                        # ✅ Complete — Data Archive
│   └── data/
│       ├── responses/                   # Raw JSON responses (10 models × 11,340)
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
│       │   └── schwartz_master_en.json  # 11,340 scenario prompts
│       └── processed/
│           └── value_winrate_by_model.json  # Aggregated win-rate results
│
└── docs/                                # GitHub Pages interactive viewer
    └── index.html
```

---

## Load Data

```python
import json

# Aggregated win-rate results
with open("L4-normative/data/processed/value_winrate_by_model.json") as f:
    data = json.load(f)

model = "OpenRouter Claude Haiku 4.5"

# Global rankings
for rank, entry in enumerate(data[model]["Global"], 1):
    print(f"#{rank} {entry['value']}: {entry['win_rate']:.1%}")

# Domain-specific
med = data[model]["Domain:MED"]
defense = data[model]["Domain:DEF"]

# Raw individual responses
with open("L4-normative/data/responses/claude_haiku_4.5/responses.json") as f:
    raw = json.load(f)

for r in raw["results"][:3]:
    print(r["id"], "→", r["parsed_result"]["major_value"])
```

---

## Successor: AIO PRISM Benchmark

This L4-only benchmark has been superseded by the **[AIO PRISM Benchmark](https://github.com/AI-Integrity/aio-prism-benchmark)**, which expands the measurement to:

- **L4 Normative**: 19 Schwartz sub-values (expanded from 10)
- **L3 Epistemic**: 10 evidence types (Walton + GRADE)
- **L2 Source**: 10 source types (Walton + Source Credibility Theory)
- **PCS**: Perspective Consistency Score (framing robustness measurement)

**[→ PRISM Interactive Viewer](https://ai-integrity.github.io/aio-prism-benchmark/)** · **[→ PRISM GitHub](https://github.com/AI-Integrity/aio-prism-benchmark)**

---

## Citation

```bibtex
@article{lee2026measuring,
  title   = {Measuring AI Value Priorities: Empirical Analysis of 113,400
             Forced-Choice Responses Across 10 AI Models},
  author  = {Lee, Seulki},
  year    = {2026},
  institution = {AI Integrity Organization (AIO)},
  doi     = {10.5281/zenodo.18859945}
}
```

---

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

---

## Contact

**AI Integrity Organization (AIO)**  
Geneva, Switzerland · [aioq.org](https://aioq.org) · 2sk@aioq.org  
GitHub: [github.com/AI-Integrity](https://github.com/AI-Integrity)
