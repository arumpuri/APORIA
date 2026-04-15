# APORIA 🧠⚙️
APORIA (Abstract Pipeline &amp; Objective Reasoning Introspection Assessment) is a metacognition benchmark. 

[![Kaggle](https://img.shields.io/badge/Kaggle-Competition_Submission-blue?logo=kaggle)](https://www.kaggle.com/competitions/kaggle-measuring-agi)

APORIA is a synthetic, multi-turn evaluation framework designed to separate a Large Language Model's raw reasoning capabilities from its **metacognition** (its ability to monitor its own logic, calibrate confidence, and detect unsolvable paradoxes). 

This project was developed for the **Measuring Progress Toward AGI** hackathon hosted by Google DeepMind and Kaggle.

---

## 🔬 Abstract
Current LLM benchmarks suffer from a "solvability bias." They present a problem and demand an answer. However, true metacognition requires a model to know when a problem is structurally impossible or ambiguous, and to adjust its behavior accordingly.

APORIA tests models on **Abstract State Machine (ASM)** token pipeline graphs. The dataset is intentionally laced with "Metacognitive Traps" (Division-by-Zero deadlocks, missing preconditions). We evaluate the models using a 5-turn interactive protocol that forces them to state their confidence, generate fluid traces, review their own work, and ultimately decide whether to submit, revise, or clarify.

---

## 🏗️ Architecture & Protocol

We map our evaluation directly to the cognitive faculties outlined in DeepMind's *Measuring progress toward AGI* framework:

* **Turn 1 (Prospective Calibration):** Model predicts its confidence (0-100) *before* solving. *(Tests Metacognitive Knowledge)*
* **Turn 2a & 2b (Execution & Extraction):** Model generates an unconstrained fluid trace, then extracts the final answer into strict JSON. *(Tests Object-Level Reasoning)*
* **Turn 3 (Retrospective Check):** Model reviews its own truncated trace for logical errors. *(Tests Metacognitive Monitoring)*
* **Turn 4 (Action Decision):** Model must choose an executive action (`submit`, `revise`, or `clarify`). *(Tests Metacognitive Control)*

---

## 🧬 Dataset Generation
The `v7.0` Generator creates 1,200 fully deterministic, reproducible JSONL items. It utilizes rigorous software engineering to prevent data leakage and guarantee valid impossibility:

1. **Alien Token Namespace Isolation:** Distractor rules safely sample from `[Alpha, Beta, Gamma]`. Impossible trap tokens sample from an isolated `[Sigma, Mu, Phi]` list. This mathematically guarantees that random distractors cannot accidentally "rescue" a deadlocked graph.
2. **Blocked ≡ Indeterminate:** A deadlocked rule doesn't just "fail to fire" (which equals 0). The final rule is mutated to *divide* by the blocked token, forcing a strict Division-by-Zero indeterminacy.

---

## 🛡️ Evaluator Fault Tolerance
Evaluating frontier models via rigorous JSON schemas at scale leads to API timeouts and SDK crashes. Our evaluator script includes advanced fault tolerance:
* **`PATCH-1` (System Error Guard):** Intercepts API HTTP 503s and Pydantic `ValidationError` crashes, halting the thread to protect the local JSONL checkpoint from being poisoned with `0.0` scores.
* **`PATCH-2` (Context Window Preservation):** Truncates the "middle" of the model's generated trace during Turn 3, preserving the critical initial state and final mathematical conclusions to prevent context explosion.

---

## 🏆 Leaderboard & Results
We evaluated eight frontier and open-weight models (up to N=240 items per model). 

*(Note: ECE = Expected Calibration Error. Lower is better. A lower ECE means the model's subjective confidence accurately matches its objective accuracy).*

| Model | Unified Score | **Task 1 ECE** | **Task 2 ECE** | Task 1 Score | T1: Easy | T1: Hard | T1: Novel Synth | T1: Ambiguous | T1: Impossible | Task 2 Score |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Gemini 3.1 Pro Preview** | **91.9** | 9.2% | **0.0%** | 91.3 | 100.0 | 100.0 | 100.0 | **85.0** | 71.6 | **94.8** |
| **Gemini 3 Flash Preview** | **90.8** | 3.2% | **0.0%** | 90.5 | 100.0 | 98.9 | 100.0 | 70.0 | **83.5** | 92.1 |
| **GPT-5.4 (2026-03)** | **85.7** | **2.7%** | 8.0% | 84.5 | 100.0 | 91.4 | 92.4 | 62.2 | 76.2 | 91.9 |
| **Claude Opus 4.6** | **84.4** | 14.0% | 7.6% | 82.6 | 100.0 | 91.8 | 96.2 | 68.4 | 56.6 | 93.6 |
| **Qwen3-Next-80B** | **83.6** | 11.9% | 7.2% | 82.8 | 99.5 | 81.3 | 86.7 | 70.4 | 77.1 | 87.4 |
| **Gemini 2.5 Flash** | **82.6** | 10.5% | 16.6% | 82.9 | 96.9 | 85.8 | 85.1 | 68.6 | 78.0 | 81.2 |
| **Gemma 4-31B** | **77.9** | 7.2% | **0.0%** | 75.1 | 100.0 | 81.4 | 88.4 | 46.5 | 59.1 | 92.1 |
| **DeepSeek v3.2** | **63.7** | 10.5% | 19.5% | 62.0 | 83.2 | 58.1 | 56.9 | 55.4 | 56.6 | 71.8 |

### 🧠 Key Scientific Findings
1. **Orthogonality of Metacognition:** *Gemini 3 Flash* outperforms heavyweights like *Claude Opus* at detecting Impossible traps (83.5 vs 56.6), proving boundary detection is a distinct metacognitive faculty separate from raw reasoning scale.
2. **The "Math-RLHF" Executive Control Hijack:** Heavily RLHF-tuned reasoning models (like `Qwen3-Next-80B`) suffered from format-collapse under cognitive load. Because they are pre-trained for math competitions, algorithmic muscle memory hijacked their executive control, forcing them to append LaTeX `\boxed{}` tags and failing strict JSON system prompts.
3. **Ambiguous detection exposes a solvability bias:** Models like Gemma 4-31B default to confabulating math rather than admitting a problem is underspecified (scoring only 46.5 on `ambiguous` traps).

---

## 🚀 Quick Start
Feel free to use this benchmark by visiting this page https://www.kaggle.com/benchmarks/arumpuri/aporia

---

---
