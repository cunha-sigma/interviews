# Interview Notes — {{CANDIDATE_NAME}}

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. All bank questions are pre-listed with their expected answer, so you only need to check the box for whichever ones you actually ask and fill in the candidate's answer under them — leave the rest untouched. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

-

## Segment 2 — Role Context (interviewer-led, 5 min)

Candidate reactions / questions during this segment, if any:

-

## Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 across categories based on time and where the candidate is weak/strong. Only fill in the ones you actually ask.

### ML / Data Science & Statistics Fundamentals

- [ ] **Q:** A recognition system provides a confidence level for each of its predictions, a real value between 0 and 1. How would you decide the confidence threshold below which a record should be routed to a human instead of automated?
  > Expected: Treat it as a precision/recall trade-off at the record level: pick the threshold using a labeled holdout set by plotting error rate vs. auto-processed volume, then pick the operating point matching an acceptable error tolerance — ideally re-validated per segment (e.g. per station/layout) rather than one global cutoff.
  Candidate's answer:

- [ ] **Q:** What does it mean for a confidence score to be "calibrated," and why would that matter here specifically?
  > Expected: Calibrated = the predicted confidence matches the empirical accuracy at that confidence level. Matters because an uncalibrated score makes a single global threshold meaningless — the same "90% confidence" could mean very different real accuracy across formats/segments/model versions.
  Candidate's answer:

- [ ] **Q:** You want to know if a new prompt/model version is actually better than production, not just different. How do you test that rigorously?
  > Expected: Run both versions on the same labeled holdout/gold dataset, compare a fixed metric with a significance test (or at minimum a large enough sample and confidence interval), ideally validate with a shadow-mode run on live traffic before fully promoting.
  Candidate's answer:

- [ ] **Q:** What's the risk of evaluating a new prompt only on examples the current system already gets right?
  > Expected: Survivorship bias — you'd never see whether the new version fixes the failure cases that actually matter, or silently regresses on edge cases; the eval set needs to be weighted toward known-hard/failure cases.
  Candidate's answer:

- [ ] **Q:** Give the formulas for precision and recall, and describe a scenario in this kind of system where you'd deliberately favor one over the other.
  > Expected: Precision = TP/(TP+FP); Recall = TP/(TP+FN). Favor precision when a wrong automatic match/extraction is costly (e.g., silently misattributing a record to the wrong advertiser); favor recall when missing a case that should route to human review is the more expensive failure.
  Candidate's answer:

### LLM Engineering

- [ ] **Q:** What does "prompt versioning" actually require beyond just saving old copies of prompt text?
  > Expected: Tracking prompt version alongside the model/config it was evaluated and promoted with, plus the eval results and dataset version tied to that prompt — so an incident traces to exactly which combination was live, and rollback is unambiguous.
  Candidate's answer:

- [ ] **Q:** What is "grounding" in the context of LLM extraction, and why does it matter for something like structured data extraction from documents?
  > Expected: Constraining the model's output to the actual source — e.g., requiring it to quote/point to the exact span it took a value from — rather than inferring plausible-sounding values. Strong signal: mentions this should be enforced via a post-hook (verify the quoted span actually exists in the source), not just prompted for.
  Candidate's answer:

- [ ] **Q:** How would you reduce the cost and latency of an LLM-heavy extraction pipeline without hurting quality?
  > Expected: Caching repeated/identical requests, routing easy/common cases to a cheaper/smaller model and reserving the strongest model for hard/low-confidence cases, batching, falling back to a more expensive model only on low-confidence or failure.
  Candidate's answer:

- [ ] **Q:** A layout the system has never seen before comes in and the LLM's structure recognition is wrong. What's your process for turning that failure into an improvement, safely?
  > Expected: Capture the failure/correction as a labeled example, add to the gold/eval set, propose a candidate prompt change, evaluate against current production on the full gold set (not just the new example), only promote if it's a net improvement.
  Candidate's answer:

- [ ] **Q:** What's "few-shot exemplar" selection, and what can go wrong if you pick exemplars carelessly?
  > Expected: Choosing example input/output pairs embedded in the prompt to steer behavior. Careless selection risks exemplars that don't represent the real input distribution, or are themselves subtly wrong, propagating the error into every future output.
  Candidate's answer:

- [ ] **Q:** If different sources (e.g., different stations or agencies) have meaningfully different layouts or language, would you rather have one global extraction prompt or many source-specific ones? What's the trade-off?
  > Expected: Many specialized prompts generally handle format variation better, but each needs its own versioning/evaluation/promotion lineage — real operational overhead that scales with partition count. Strong answer names both sides.
  Candidate's answer:

- [ ] **Q:** Should prompt retraining/improvement run on a fixed schedule (e.g., weekly), or be triggered by something else? Why?
  > Expected: Event-driven is generally better — trigger retraining when a live quality signal (e.g. rising low-confidence rate) crosses a threshold, rather than a fixed calendar that could leave a real regression live too long, or retrain when nothing's actually degraded.
  Candidate's answer:

### AWS & Production Infrastructure

- [ ] **Q:** What's the difference between an IAM role and an IAM user, and how would a service like Glue or Lambda authenticate to call Bedrock?
  > Expected: A user is a persistent identity with long-lived credentials (for a person, or an external system with no way to assume a role). A role has no credentials of its own — assumed temporarily for short-lived, auto-rotated credentials. For Glue/Lambda this isn't a real "which one" choice: standard is an execution role, no IAM user involved. Static user keys embedded in a Lambda/Glue job is the anti-pattern to flag.
  Candidate's answer:

- [ ] **Q:** What would you use CloudWatch for in an LLM pipeline like this one, beyond basic uptime?
  > Expected: Tracking latency/error rate per pipeline stage, custom metrics for confidence-score distribution and LLM cost per run, alarms on drift in those metrics — not just "is the Lambda alive."
  Candidate's answer:

- [ ] **Q:** Why might you choose S3 as the landing zone for incoming files rather than writing straight into a database?
  > Expected: Cheap, durable, versionable storage for raw/unstructured input regardless of format, decouples ingestion from processing (reprocess from the same raw file later), integrates natively with event-driven triggers/orchestration — a database expects structure you don't have yet at landing time.
  Candidate's answer:

### SQL

- [ ] **Q:** How would you write a query to find, for each Station, the most recent record and its value — without a full self-join?
  > Expected: Window function (`ROW_NUMBER() OVER (PARTITION BY station ORDER BY timestamp DESC)`), filter to `rn = 1` — not a correlated subquery or self-join.
  Candidate's answer:

- [ ] **Q:** You need to detect duplicate records that should reconcile to a single entity but have slightly different spellings (e.g., station names). Is this a SQL problem, and if not, what is it?
  > Expected: Exact-match dedup is a SQL problem; near-duplicate/spelling-variance detection is a fuzzy-matching problem — SQL can do exact/basic pattern matching, but real fuzzy/semantic matching needs a dedicated approach. Good answer draws this line.
  Candidate's answer:

### Python / Software Engineering Reasoning

- [ ] **Q:** (warm-up, optional) What's the time complexity of checking membership in a Python `list` vs. a `set`/`dict`, and why does that matter for a matching pipeline processing many records?
  > Expected: `list` membership O(n) (linear scan); `set`/`dict` membership O(1) average (hash lookup). Matters directly for entity matching against master data — should use a hash-based structure, not a list scan.
  Candidate's answer:

### Ownership / Ambiguity (behavioral — no single correct answer; evaluate against criteria)

- [ ] **Q:** Tell me about a time you owned an ML system end-to-end, including a decision nobody explicitly asked you to make.
  > Criteria: A concrete unprompted decision (not just "I built X"), the reasoning behind it, ownership of the outcome/consequence.
  Candidate's answer:

- [ ] **Q:** Describe a time production feedback (errors, corrections, drift) told you something was wrong with a model/prompt in production. What did you do?
  > Criteria: A real detection mechanism (not just "someone complained"), a concrete diagnosis process, a fix actually validated before shipping — not just a quick patch.
  Candidate's answer:

## Segment 4 — Task (20–25 min)

Option used: [ ] A — Code Reading & Complexity   [ ] B — Prompt & Evaluation Design   [ ] C — System Design

Notes on approach:

-

Facilitation tracking (Option C only) — mark **U** (unprompted), **P** (prompted), or **M** (missing) per topic:

| Topic | U/P/M | Notes |
|---|---|---|
| Recognition/extraction step & output format | | |
| Confidence scoring | | |
| Low-confidence fallback / human-in-the-loop | | |
| Entity matching strategy — layered, not single-shot | | |
| Per-source/per-partition specialization | | |
| Corrections feeding back into improvement | | |
| Evaluation before shipping a change | | |
| Retrain trigger — schedule vs. signal | | |
| Production monitoring (drift, cost, latency) | | |
| Safe rollout of a new version | | |

Overall bar level for this segment: [ ] Above bar   [ ] At bar   [ ] Below bar

## Segment 5 — Candidate Questions (5–10 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

-

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|---|---|
| Python / SWE reasoning | High | | |
| ML / Data Science & statistics fundamentals | High | | |
| LLM engineering | High | | |
| Entity matching / ranking problem-solving | High | | |
| Evaluation & MLOps discipline | High | | |
| System design synthesis | High | | |
| AWS / production infra | Medium | | |
| SQL | Medium | | |
| Ownership / ambiguity tolerance | Medium | | |
| Communication clarity | Medium | | |

## Decision Lean

[ ] Strong hire   [ ] Borderline   [ ] No hire

Justification:

-

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-
