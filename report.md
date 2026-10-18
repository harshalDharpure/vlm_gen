# SCF-VL / CrashInsight — Paper Results Tables

**Dataset:** CrashX-1500 (train / val / test). **Backbone:** Qwen2.5-VL-7B-Instruct (QLoRA).  
**Reported main system:** **SCF-Hybrid** (faithfulness-first decode fusion).  
**Seed:** 42. Metrics on the **test** split.

> **Venue note (honest):** These results support a strong **workshop / specialized venue** paper (video understanding, explainable multimodal, traffic safety). For a top **A\*** venue (CVPR / ICCV / NeurIPS / ACL), you still need: (i) clear external baselines (prior crash-VLM / video-LLM SOTA), (ii) multi-seed mean±std, (iii) human study or error analysis, and (iv) a sharper novelty claim than decode-time fusion alone. Tables below are publication-ready for the empirical section; A\* needs that extra package.

---

## Table 1. Main Results (Overall)

Best value per column in **bold**. SCF-Hybrid is the primary reported system.

| Method | BLEU-4 ↑ | METEOR ↑ | BERTScore ↑ | tIoU ↑ | R@0.5 ↑ | Full-NLI ↑ | Full-Ent. ↑ |
|--------|--------:|---------:|------------:|-------:|--------:|-----------:|------------:|
| Qwen2.5-VL SFT (plain) | 0.112 | 0.210 | 0.901 | 0.459 | 0.527 | 0.206 | 0.350 |
| SCF-VL (v2) | 0.104 | 0.208 | 0.901 | **0.479** | **0.580** | 0.221 | 0.361 |
| SCF-VL + FactPack (v3) | **0.129** | 0.207 | 0.901 | 0.447 | 0.373 | 0.322 | 0.429 |
| SCF-Faith | 0.123 | 0.210 | 0.900 | 0.416 | 0.467 | 0.408 | 0.518 |
| SCF-Distill (BoN) | 0.114 | 0.210 | 0.899 | 0.414 | 0.413 | 0.507 | 0.611 |
| **SCF-Hybrid (ours)** | 0.125 | **0.213** | 0.901 | **0.479** | **0.580** | **0.559** | **0.639** |

**Takeaway:** Hybrid matches best temporal localization, keeps caption metrics near the best single model, and yields the largest faithfulness gain (Full-NLI **0.56** vs plain **0.21** / v3 **0.32**).

---

## Table 2. Explanation Quality (Caption Metrics)

| Method | BLEU-1 | BLEU-2 | BLEU-3 | BLEU-4 | ROUGE-L | METEOR | BERTScore |
|--------|-------:|-------:|-------:|-------:|--------:|-------:|----------:|
| Qwen2.5-VL SFT (plain) | 0.406 | 0.260 | 0.171 | 0.112 | 0.303 | 0.210 | 0.901 |
| SCF-VL (v2) | 0.402 | 0.257 | 0.165 | 0.104 | 0.298 | 0.208 | 0.901 |
| SCF-VL + FactPack (v3) | 0.419 | **0.281** | **0.191** | **0.129** | **0.320** | 0.207 | 0.901 |
| SCF-Faith | 0.426 | 0.280 | 0.185 | 0.123 | 0.313 | 0.210 | 0.900 |
| SCF-Distill (BoN) | 0.410 | 0.263 | 0.174 | 0.114 | 0.295 | 0.210 | 0.899 |
| **SCF-Hybrid (ours)** | **0.427** | 0.281 | 0.188 | 0.125 | 0.313 | **0.213** | 0.901 |

---

## Table 3. Temporal Localization

| Method | text tIoU ↑ | text R@0.5 ↑ | head tIoU ↑ | head R@0.5 ↑ |
|--------|------------:|-------------:|------------:|-------------:|
| Qwen2.5-VL SFT (plain) | 0.459 | 0.527 | — | — |
| SCF-VL (v2) | **0.479** | **0.580** | 0.342 | 0.300 |
| SCF-VL + FactPack (v3) | 0.447 | 0.373 | **0.443** | 0.367 |
| SCF-Faith | 0.416 | 0.467 | 0.416 | **0.467** |
| SCF-Distill (BoN) | 0.414 | 0.413 | 0.414 | 0.413 |
| **SCF-Hybrid (ours)** | **0.479** | **0.580** | 0.342† | 0.300† |

† Hybrid uses v2 timestamps in the decoded text; head metrics inherit the v2 temporal head.

---

## Table 4. Faithfulness (NLI)

Premise = GT forensic facts (+ explanation for Full-NLI). Higher entailment / score is better; lower contradiction is better.

| Method | NLI-Ent. ↑ | NLI-Con. ↓ | NLI-Score ↑ | Full-NLI-Ent. ↑ | Full-NLI ↑ |
|--------|-----------:|-----------:|------------:|----------------:|-----------:|
| Qwen2.5-VL SFT (plain) | 0.158 | 0.165 | −0.007 | 0.350 | 0.206 |
| SCF-VL (v2) | 0.182 | 0.187 | −0.006 | 0.361 | 0.221 |
| SCF-VL + FactPack (v3) | 0.306 | 0.144 | 0.162 | 0.429 | 0.322 |
| SCF-Faith | 0.368 | 0.165 | 0.203 | 0.518 | 0.408 |
| SCF-Distill (BoN) | 0.427 | 0.184 | 0.243 | 0.611 | 0.507 |
| **SCF-Hybrid (ours)** | **0.475** | **0.141** | **0.334** | **0.639** | **0.559** |

**Relative gain (Hybrid vs plain):** Full-NLI **+171%** (0.206 → 0.559).  
**Relative gain (Hybrid vs FactPack v3):** Full-NLI **+74%** (0.322 → 0.559).

---

## Table 5. Ablation of SCF Components

All rows use the SCF-VL training setup with the named module removed (QLoRA 7B). NLI not re-scored for all ablations.

| Variant | BLEU-4 ↑ | METEOR ↑ | BERTScore ↑ | tIoU ↑ | R@0.5 ↑ | Sev. Acc. (text) |
|---------|--------:|---------:|------------:|-------:|--------:|-----------------:|
| Full SCF-VL (v2 ref.) | 0.104 | 0.208 | 0.901 | **0.479** | **0.580** | 0.453 |
| w/o phase head | 0.105 | 0.208 | 0.900 | 0.416 | 0.487 | 0.453 |
| w/o gate | 0.101 | 0.209 | 0.899 | 0.394 | 0.453 | **0.493** |
| w/o causal head | 0.113 | 0.203 | 0.900 | 0.431 | 0.500 | 0.467 |
| w/o SCF aux (LoRA-only) | 0.111 | **0.212** | 0.901 | 0.434 | 0.500 | 0.453 |

**Takeaway:** Removing the **gate** or **phase** head hurts temporal localization the most; aux-only removal still underperforms full SCF on tIoU.

---

## Optional Table 6. Design Variants (for appendix)

Two decode policies on the same checkpoints (no extra training):

| Decode policy | BLEU-4 | METEOR | tIoU | Full-NLI | Note |
|---------------|-------:|-------:|-----:|---------:|------|
| Lexical-balanced (m12f) | **0.129** | 0.211 | 0.479 | 0.436 | Prioritizes BLEU ≈ v3 |
| **Faithfulness-first (Hybrid, main)** | 0.125 | **0.213** | 0.479 | **0.559** | Prioritizes NLI (reported) |

---

## Method name mapping (for camera-ready consistency)

| Paper name | Checkpoint / run id |
|------------|---------------------|
| Qwen2.5-VL SFT (plain) | `scf_vl_plain_s42` |
| SCF-VL (v2) | `scf_vl_v2_s42` |
| SCF-VL + FactPack (v3) | `scf_vl_v3_s42` |
| SCF-Faith | `scf_faith_s42` |
| SCF-Distill (BoN) | `scf_distill_s42` |
| **SCF-Hybrid (ours)** | `scf_hybrid_s42` (= faithfulness-first / bal) |

---

## Suggested figure captions (short)

- **Fig. 1:** Pipeline — video → SCF heads (severity / impact / phase) → FactPack → explanation → NLI rerank / hybrid assemble.  
- **Fig. 2:** Bar chart of Full-NLI vs BLEU-4 (shows faithfulness–lexical tradeoff and Hybrid Pareto position).  
- **Fig. 3:** Qualitative examples (correct timing + faithful cause vs plain hallucination).

---

## What still strengthens an A\* submission

1. Compare against published CrashX / traffic-VLM baselines (not only your ablations).  
2. Report **3 seeds** (mean ± std) for Hybrid and v3.  
3. Human rating: faithfulness / usefulness (20–50 videos).  
4. Limit decode-fusion as a *system* contribution; keep SCF architecture + BoN distill as the learning contribution.  
5. Fix causal-head accuracy (currently ~0 in several runs) or drop it from claims.
