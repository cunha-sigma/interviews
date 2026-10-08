# ML Platform

Transcribed as-is from `ML-platform concept, arch and WBS.docx`.

## 1 Concept

### 1.1 Problem
LLM-based recognition process must turn semi-structured Postlog / Order / Prelog files into a canonical table. When recognition or matching is wrong, an operator can correct that file. The correction is not fed back into the prompt or model, so the next file with the same layout or creative-ID variant can fail in the same way. There is also no gold-set harness to prove that a new prompt is better than the one in production.

### 1.2 North star
- **Evaluation platform** — A shared offline harness that compares raw landed files (as-is Excel) against correct canonical outputs (gold Structured Postlogs). It proves whether a candidate prompt or model is better than production before anything is promoted. The same harness can score any recognizer style — LLM, classical ML, rules, or human-in-the-loop — so improvements are measured, not assumed.
- **Recognizer + observer** — On the live path, one Recognizer LLM maps layout and columns into the canonical schema. Offline, a second Observer LLM rewrites the recognition prompt when the online quality monitor detects that live quality has dropped below a threshold. A holdout Evaluate step gates automatic deploy of the candidate only if it beats current production; otherwise, production stays on vN. Retraining is event-driven, not on a fixed calendar.
- **Optional extraction fallback** — If structure recognition fails on a live file, a separate Fallback extraction LLM (own prompt, own eval/retraining lineage) recovers the same Canonical CSV. It runs only on failure, so the happy path stays lean while hard layouts still get a recovery path without blocking the whole pipeline on a single recognizer.
- **Semantic ISCI rules** — Creative IDs are rule-generated with embedded meaning, not opaque random codes. Matching always tries exact/fuzzy ISCI / CreativeID lookup against Master Data first, which covers the common case cheaply and deterministically before any LLM is involved.
- **ISCI matching LLM** — When exact/fuzzy matching fails, a separate Semantic-match LLM (own prompt, own eval/retraining lineage) uses the semantic meaning encoded in the ISCI name to infer where the ISCI came from and which Master Data creative it belongs to, producing Matched Master Data (Creative · Station · Advertiser).
- **Automatic model / prompt promotion (Dev → Prod)** — After Evaluate passes, the winning candidate prompt or model is promoted automatically from Dev to Prod via the registry (no manual cutover). Production Glue/apps load the new Production version on the next run; failed gates leave Prod unchanged on the current version.
- **LLM families as prompt sets** — Each LLM family (XLSX reader/recognizer, fallback, semantic / entity matching, and any later family) is not a single prompt. It is a set of prompts keyed by Station, agency, or any other partition that changes layout or language. Complex, multi-use-case prompts are split so each variant is specialized, versioned, evaluated, and promoted on its own lineage rather than one overgrown prompt serving every case.

This end-state requires a fully fledged ML platform and an adopted MLOps culture: prompt/model versioning in a registry, automatic Dev → Prod promotion after eval gates, and continuous online evaluation / quality monitoring that can trigger retraining without manual one-off releases.

## 2 Architecture

Live ingestion on top → online quality monitor in the middle → offline retraining on the ML platform at the bottom.

### 2.1 Evaluation platform (offline)
Online quality monitor watches live LLM quality (recognition, extraction fallback, matching). When quality drops below a threshold, it triggers the Retraining process.

Retraining process (started only by that trigger):

| Piece | What it is |
|---|---|
| Observer LLM | Improves the prompt from failures and gold; does not serve live files |
| Candidate LLM | Proposed prompt vN+1; not in production yet |
| Evaluate | Runs Candidate (and Current prod as baseline) on Raw Postlogs (Landed Excel) vs Structured Postlogs (Canonical CSV as gold) |
| Prompt / model registry | If Candidate beats Current prod → auto-deploy as new Production LLM |

The same loop applies independently to Recognition, Extraction, and Semantic-ISCI prompts. Raw Postlogs and Structured Postlogs are separate stores: unstructured files as landed vs canonical gold outputs.

### 2.2 Live postlog data flow
- **Landing** — Postlog / Order / Prelog Excel in the S3 landing zone as-is.
- **Structure recognition** — Recognizer LLM (layout + column mapping, recognition prompt vN). Success → Canonical CSV. Failure → optional Fallback extraction LLM (own prompt) → Canonical CSV.
- **Canonical** — standardized columns and typed fields.
- **Creative matching** — Semantic ISCI rules → exact / fuzzy match first; if fail → Semantic-match LLM.
- **Result** — Matched Master Data (Creative · Station · Advertiser).

Solid arrows = happy path; dashed = failure / fallback. Live path does not include Evaluate, Observer, Candidate, or the registry.

### 2.3 Recognizer + observer

| Agent | Role |
|---|---|
| Recognizer LLM | Online: layout + column mapping, recognition prompt vN |
| Observer LLM | Offline: from failures + gold corrections, proposes prompt vN+1 |

On quality drop → observer proposes new prompt → holdout eval → automatic deploy if gate passes; otherwise keep vN.

### 2.4 Optional: extraction fallback (Phase 2)
Runs only if Recognizer fails. Own prompt; same Canonical CSV output. Own Retraining lineage, triggered by online quality monitor.

### 2.5 Semantic ISCI (Phase 2)
Rule-generated Creative IDs → exact / fuzzy first → Semantic-match LLM on failure. Separate Retraining lineage.

## 3 Phases

| Phase | Scope | Notes |
|---|---|---|
| Phase 1 | ML platform + online ML performance monitor for postlog-recognition only | Live Glue + Bedrock recognition already works; Airflow available; MVP metrics (not research-grade eval) |
| Phase 2 | 2 more LLM models: (1) Fallback extraction (on recognition failure); (2) Semantic field-value matching (when exact/fuzzy fails) | Each with own MLflow prompt lineage + eval loop on the existing platform; host may be Glue or another application |

Phasing diagrams: `Postlog_MLPlatrofm_Phases.drawio`.

## 4 Roadmap (LLM-platform swimlane)

From `Roadmap_self_improvement.drawio` — Data Platform Transformation Program. LLM-platform work sits in its own swimlane.

Main timeline:
- **Phase 1 — this year (2026):** ML platform, online quality monitor, prompt registry, evaluation, self-improving loop, integration & release; DevOps access/infra + CI/CD with continuous retraining.
- **Phase 2 — next year (2027):** 2 more LLM models (fallback extraction + semantic field-value matching) on the existing platform.

## 5 Work breakdown (Phase 1)

### 5.1 Assumptions
- 1 ML engineer for application streams; DevOps for networking & access (1 sprint) plus CI/CD with continuous retraining (1 sprint)
- Phase 1 = postlog-recognition only
- Live Glue + Bedrock recognition already works
- Airflow available; MVP metrics (not research-grade eval)
- Calendar typically 20–40% longer than effort (reviews, access, gold-set agreement)

### 5.2 Work streams

| # | Stream | Who | Scope | Est. | When |
|---|---|---|---|---|---|
| 0 | MLflow setup, networking and access | DevOps | VPC / security groups / private connectivity for MLflow ECS, PostgreSQL, S3; Glue → MLflow / Bedrock access; IAM roles & secrets baselines; roadmap: Manual MLflow infra setup + IaC | 2–3 weeks | Phase 1 / 2026 |
| 0b | CI/CD with continuous retraining | DevOps | CI/CD for Glue/MLflow/prompt artifacts; wire promote + online quality monitor so retraining and eval-gated deploy run continuously | 2 weeks | Phase 1 / 2026 |
| 1 | Prompt registry | ML eng | Registry contract; Glue with MLflow integration | 1–2 weeks | Phase 1 / 2026 |
| 2 | Evaluation platform | ML eng | Raw / gold stores; Glue Evaluate; Glue Promote; gate Candidate vs Production | 4–6 weeks | Phase 1 / 2026 |
| 3 | Self-improving loop (orchestration) | ML eng | Glue Observer; Airflow DAG; online quality monitor → trigger | 2–3 weeks | Phase 1 / 2026 |
| 4 | Integration & handoff / release | ML eng | Ops, dashboards, dry-run promote in Prod; roadmap: Integration & release then Wrap-up | 2–3 weeks | Phase 1 / 2026 |

Stream 0 is a prerequisite for MLflow on ECS and Glue reaching the tracker; **it can start first and overlap early registry work once access paths exist.** Stream 0b (CI/CD + continuous retraining) follows once networking is in place and **should land before or alongside the self-improving loop** so promote/retrain is not manual.

### 5.3 Build order
1. Networking and access / Manual MLflow infra (DevOps, 1 sprint)
2. Prompt registry (Dev/Prod MLflow + live load)
3. Evaluation platform (Evaluate + Promote + gold)
4. CI/CD with continuous retraining (DevOps, 1 sprint)
5. Self-improving loop (Observer + Airflow + monitor)
6. Integration & handoff / release → wrap-up
7. (Phase 2) Fallback extraction LLM → Semantic field-value matching LLM (2 more models; any host app)

### 5.4 Totals

| Scope | Effort | Calendar |
|---|---|---|
| DevOps (networking & access, CI/CD + continuous retraining) | 4-5 weeks | ~1 month |
| Full Phase 1 (registry + evaluate + promote; Observer / auto-trigger later) | ~9–14 weeks | ~2.5–3.5 months |
| Phase 2 (2 more LLM models: fallback extraction + semantic field-value matching) | ~4–6 weeks total | Next year (2027) |

**Phase 1 calendar for ML eng is wall-clock after (or overlapping) the DevOps networking sprint. End-to-end from kickoff ≈ 2 DevOps sprints (access + CI/CD continuous retraining) + Phase 1 if both are fully blocking; less if overlapped.**

## 6 Work breakdown (Phase 2)

### 6.1 Assumptions
- 1 ML engineer; Phase 1 platform (registry, eval/promote, self-improving loop, CI/CD) is already live
- Phase 2 = stand up a self-improving workflow for two more LLM prompts on the existing platform (same Observer → Candidate → Evaluate → Dev→Prod promote pattern as recognition)
- The two prompts:
  - Fallback — runs when a semi-structured postlog was not recognized by the Recognizer
  - Entity matching — matches postlog field values to Master Data entities when exact/fuzzy match fails
- Host may be Glue or another application — not Glue-only
- Calendar typically 20–40% longer than effort

### 6.2 Work streams

| # | Stream | Who | Scope | Est. | When |
|---|---|---|---|---|---|
| 1 | Fallback LLM prompt + self-improving loop | ML eng | Prompt for unrecognized semi-structured postlogs; live path only on recognition failure; Canonical CSV output; registry + Evaluate + Observer + online quality monitor for this lineage | ~2–3 weeks | Phase 2 / 2027 |
| 2 | Entity-matching LLM prompt + self-improving loop | ML eng | Prompt for matching postlog values to Master Data entities; live path after exact/fuzzy fail; own registry + Evaluate + Observer + online quality monitor | ~2–3 weeks | Phase 2 / 2027 |
| 3 | Integration & handoff | ML eng | Integrate both prompts into existing pipelines and UI; dry-run promote Dev → Prod; ops/dashboards for the two new self-improving lineages | 2–3 weeks | Phase 2 / 2027 |

Streams 1 and 2 can run partly in parallel once gold sets and host integration points are agreed; 3 lands each prompt as it becomes ready.

## 6 Bottom line

*(numbered "6" in the source document; follows section 6.2 as written)*

Today, operator fixes on Postlog / Order / Prelog recognition do not feed back into the prompt, and there is no gold-set harness to prove a new prompt is better than production. The north star closes that loop: an evaluation platform (raw vs gold), a Recognizer + Observer with online quality triggers, automatic Dev → Prod promotion after eval gates, then (later) a fallback extraction LLM and a semantic ISCI-matching LLM, with exact/fuzzy rules tried first.

Architecturally, live ingestion stays separate from the offline ML platform. An online quality monitor triggers retraining (Observer → Candidate → Evaluate → registry); the live path only serves Production prompts. Recognition, extraction, and semantic matching each get their own lineage on the same platform.

Phase 1 (this year, 2026) builds the ML platform for postlog-recognition: DevOps access/infra + CI/CD with continuous retraining (~2 sprints), then one ML engineer delivers prompt registry, evaluation/promote, self-improving orchestration, and integration (~2.5–3.5 months lean MVP effort). Phase 2 (next year, 2027) adds 2 more LLM models (~6–9 weeks) on that platform, hosted in Glue or other applications.

---

**Note on a source inconsistency (not introduced by this transcription):** §5.4's totals table states Phase 2 effort as "~4–6 weeks total," while the Bottom Line section above states "~6–9 weeks" for the same Phase 2 scope. Flagging this as-is rather than picking one silently — worth clarifying which figure is authoritative if Phase 2 timing becomes relevant.
