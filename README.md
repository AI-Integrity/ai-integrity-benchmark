# AI Integrity Benchmark Suite

**Authority Stack Benchmark — measuring AI Integrity across 4 layers**

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18631255.svg)](https://doi.org/10.5281/zenodo.18631255)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

---

## Overview

**AI Integrity** is the structural property in which each functional layer of an AI system operates according to its own standards, without undue distortion from other layers. This repository hosts the **Authority Stack Benchmark Suite** — a systematic measurement framework that evaluates AI systems across the full value–epistemics–source–data stack.

> This repository is not a single benchmark but a **benchmark series** measuring the full value–epistemics–source–data stack of AI systems.

The completed Layer 4 (Normative Authority) benchmark evaluated **10 commercial LLMs** across **113,400 forced-choice value judgment scenarios** based on Schwartz's Theory of Basic Human Values.

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

**Layer separation** is the key principle: each layer should operate according to its own epistemic standards. When upper layers (e.g., normative preferences) distort lower layers (e.g., factual judgments), the system exhibits **Authority Pollution** — the single most important failure mode this framework is designed to detect.

| Layer | What It Measures | Example Failure |
|-------|-----------------|-----------------|
| L4 — Normative | Which values are prioritized in ethical dilemmas | Model claims neutrality but systematically favors Universalism |
| L3 — Epistemic | Whether evidence standards vary with normative framing | Factual accuracy drops when topic conflicts with favored values |
| L2 — Source | Whether source prestige biases factual evaluation | Identical claim evaluated differently based on author affiliation |
| L1 — Data | Whether training data creates systematic blind spots | Model has rich knowledge of Western institutions but sparse coverage of Global South |

---

## Benchmark Status

| Benchmark | Layer | Status | Scenarios | Description |
|-----------|-------|--------|-----------|-------------|
| [L4-normative](./L4-normative/) | Normative | ✅ Complete | 113,400 | Schwartz 10-value forced-choice |
| [L4-normative-refined](./L4-normative-refined/) | Normative | 🔧 Dev | ~51,345 | Schwartz 19 sub-value |
| [L3-epistemic](./L3-epistemic/) | Epistemic | 📐 Design | TBD | Framing-dependent accuracy |
| [L2-source](./L2-source/) | Source | 💡 Concept | TBD | Source prestige bias |
| [L1-data](./L1-data/) | Data | 💡 Concept | TBD | Training data coverage audit |
| [applied](./applied/) | All | 💡 Concept | — | Institutional AI selection |

---

## L4 Normative: Key Findings

### 1. Universalism-first vs. Security-first Split (6:4)

Across 10 evaluated models, a clear bifurcation emerged:

| Universalism-first (6 models) | Security-first (4 models) |
|-------------------------------|--------------------------|
| Claude Haiku 4.5 (99.1%) | GPT-5 Mini (89.0%) |
| Gemini 3 Flash (95.7%) | Grok 4.1 Fast (93.9%) |
| Llama 3.3 70B (92.6%) | DeepSeek V3.2 (85.3%) |
| GPT-OSS 120B (91.7%) | Kimi K2.5 (87.7%) |
| MiniMax M2.1 (89.4%) | |
| Kimi K2 (87.8%) | |

No model ranked Power or Hedonism in the global top 3. All models exhibited strong convergence on the upper-value cluster (Universalism, Security, Benevolence) but diverged sharply on relative ordering.

### 2. Defense Domain Power Gap (27.8%–72.1%)

The defense domain produced the widest inter-model divergence on the Power value:
- **Lowest:** Claude Haiku 4.5 — 27.8% win-rate
- **Highest:** Kimi K2 — 72.1% win-rate

This gap reveals that models trained by different providers encode fundamentally different value priorities when confronted with military/security scenarios.

### 3. Intra-Provider Divergence: Kimi K2 → K2.5

| Rank | Kimi K2 | Kimi K2.5 |
|------|---------|-----------|
| #1 | Universalism (87.8%) | Security (87.7%) |
| #2 | Security (85.4%) | Universalism (85.2%) |
| #3 | Benevolence (73.1%) | Benevolence (73.8%) |

A single version update (K2 → K2.5) inverted the top two value rankings. This demonstrates that value alignment is **not stable across model versions** even within the same provider.

### 4. Universalism Paradox Hypothesis

Models with the highest Universalism scores may not be the most factually accurate. If Universalism (tolerance, fairness) systematically distorts factual judgments in socially sensitive domains, it constitutes **Authority Pollution** — normative preferences corrupting epistemic standards. This hypothesis motivates the L3 Epistemic Authority benchmark.

---

## L4 Normative: Dataset

### Scenario Construction

Each scenario is a **forced-choice value dilemma** requiring the model to prioritize one Schwartz value over another:

```
7 domains × 12 severity levels × 3 temporal conditions × 45 value pairs = 11,340 per model
11,340 × 10 models = 113,400 total scenarios
```

**Domains:** Medical Bioethics, Law, Education, Defense, Technology, Environment, Economy

**Response Format (JSON):**
```json
{
  "summary": "Situation summary (1 sentence)",
  "choice": "A or B",
  "major_value": "Selected value (Schwartz value name)",
  "sacrificed_value": "Sacrificed value (Schwartz value name)",
  "reason": "Reason for choice (2-3 sentences)"
}
```

### Data Files

| Path | Description |
|------|-------------|
| `L4-normative/data/prompts/schwartz_master_en.json` | Full 11,340 scenario prompts (system + user) |
| `L4-normative/data/responses/<model>/` | Raw model responses (JSON) |
| `L4-normative/data/processed/value_rankings.json` | Aggregated win-rate rankings (global, domain, severity, temporal) |

---

## Evaluated Models

| Provider | Model | Scenarios | Access |
|----------|-------|-----------|--------|
| Anthropic | Claude Haiku 4.5 | 11,340 | OpenRouter |
| OpenAI | GPT-5 Mini | 11,340 | OpenRouter |
| Google | Gemini 3 Flash Preview | 11,340 | OpenRouter |
| xAI | Grok 4.1 Fast | 3,193 | OpenRouter |
| Meta | Llama 3.3 70B | 11,340 | OpenRouter |
| DeepSeek | DeepSeek V3.2 | 3,192 | OpenRouter |
| Moonshot | Kimi K2 | 11,340 | OpenRouter |
| Moonshot | Kimi K2.5 | 11,340 | OpenRouter |
| MiniMax | MiniMax M2.1 | 3,187 | OpenRouter |
| Alibaba | GPT-OSS 120B | 3,194 | OpenRouter |

---

## Quick Start

```python
import json

# Load a model's raw responses
with open("L4-normative/data/responses/claude_haiku_4.5/responses.json", "r", encoding="utf-8") as f:
    data = json.load(f)

print(f"Model: {data['run']['model']}")
print(f"Total scenarios: {data['run']['total_ids']}")

# Inspect a single response
result = data["results"][0]
print(f"Scenario: {result['id']}")
print(f"Choice: {result['parsed_result']['major_value']} over {result['parsed_result']['sacrificed_value']}")
print(f"Reason: {result['parsed_result']['reason']}")
```

```python
# Load aggregated value rankings
with open("L4-normative/data/processed/value_rankings.json", "r", encoding="utf-8") as f:
    rankings = json.load(f)

# Print global top-3 for each model
for model, sections in rankings.items():
    top3 = sections["Global"][:3]
    print(f"\n{model}:")
    for i, v in enumerate(top3, 1):
        print(f"  #{i} {v['value']}: {v['win_rate']:.1%}")
```

---

## Contributing

We welcome contributions at every level:

- **L4 Normative:** Replication studies, additional language/cultural variants, alternative analysis methods
- **L3–L1 Design:** Feedback on experimental design for Layers 3, 2, and 1
- **Collaborative Research:** Joint publications, cross-institutional benchmark validation

See each layer's README for the current design status and open questions.

To contribute:
1. Fork this repository
2. Create a feature branch (`git checkout -b feature/your-contribution`)
3. Submit a Pull Request with a clear description

---

## Roadmap

| Stage | Layer | Description | Directory |
|-------|-------|-------------|-----------|
| **A** | L4 (10-value) | ✅ Complete. 113,400 scenarios across 10 models | `L4-normative/` |
| **B** | L4 (19 sub-value) | 🔧 Higher-resolution normative measurement | `L4-normative-refined/` |
| **C** | L3 Epistemic | 📐 Universalism Paradox verification + reasoning analysis | `L3-epistemic/` |
| **D** | L2 Source + L1 Data | 💡 Source bias detection + training data audit | `L2-source/`, `L1-data/` |
| **E** | Applied | 💡 Institutional AI selection support framework | `applied/` |

---

## Citation

```bibtex
@article{lee2026ai_integrity,
  title={AI Integrity: Definition, Measurement Framework, and Empirical Evidence from 113,400 Value Judgment Scenarios},
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
- Website: [https://aioq.org](https://aioq.org)
- Email: contact@aioq.org

---

*This work is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).*
