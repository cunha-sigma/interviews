# Interview Plan — Senior ML Engineer / Data Scientist (REF4176G)

**Role:** Senior ML Engineer / Data Scientist, per `job_description.md`. Fully remote across Europe. Client operates a large-scale media/advertising data platform; the team builds a self-learning Postlog and Prelog recognition system using LLMs — layout/structure recognition, entity matching against master data (Station, Advertiser, CreativeID), confidence-based automation, and continuous prompt/model improvement from production feedback.

**A note on how this plan was built:** there was no existing interview script or timeline for this role — this is a first version, constructed directly from `job_description.md`'s required skills and responsibilities plus the client's own `Interview Criteria and Questions.pdf` (which is transcribed as `candidates/review_template.md` and defines the final write-up's scoring bars, but has no technical content of its own). No candidate exercise from the client was shared, so the task menu in §6 is designed from scratch to match the JD. Revisit once real interviews produce feedback.

**Reference architecture:** `ml_platform_concept_arch_wbs.md` (in this role's folder) is the client's actual concept/architecture document for the system this hire will work on. It's used here as an **interviewer-only answer key** for the Q&A and system-design task below — the same principle as the traffic-shaping-ml role's Segment 4: give the candidate real business framing, but let them reach the design themselves rather than reciting an architecture they were handed. Don't read from it or summarize it to the candidate directly.

---

## 1. What this interview is screening for

| Area | Why it matters here | JD signal |
|---|---|---|
| Python & software engineering reasoning | Production system, not notebooks — must read/reason about code correctness and cost, even without live coding | Python: Expert (4/4) |
| ML / Data Science & statistics fundamentals | Classification, ranking, matching, confidence estimation, hypothesis testing, threshold optimization are named directly | Machine Learning: Expert (4/4) |
| LLM engineering | Core of the role: prompt engineering/versioning, evaluation, grounding, low-confidence handling, cost/latency optimization | LLM Engineering: Strong (3/4) |
| AWS & production infra | S3, IAM, CloudWatch, orchestration explicitly named as hands-on requirements | AWS: Strong (3/4) |
| SQL | Explicitly required | SQL: Strong (3/4) |
| Evaluation / MLOps discipline | Offline eval metrics, holdout validation, shadow-mode comparisons, production monitoring, drift — this is how "self-learning" actually gets proven safe | Responsibilities: eval pipelines, shadow-mode, drift monitoring |
| Entity matching / ranking problems | A named, concrete responsibility (Station/Advertiser/CreativeID matching), not just general ML | Requirements: "classification, ranking, matching, or recommendation-related problems" |
| Ownership & ambiguity tolerance | Personal Profile section names this directly; role expects end-to-end ML lifecycle ownership | Personal Profile section |

**Deliberate design choice on the exercise (§6):** live coding was deliberately not used — with LLM coding assistants now ubiquitous, watching someone type code live tests typing speed and assistant-prompting more than engineering judgment. Instead, the exercise menu tests the things that don't get delegated to an assistant: reading code and reasoning about its cost, designing a prompt/evaluation strategy on paper, or designing a system end-to-end out loud. None require the candidate to have live access to an LLM API during the interview.

---

## 2. Timeline (proposed — no existing script for this role, adjust as needed)

| # | Duration | Segment |
|---|---|---|
| 1 | 5–10 min | Candidate introduces themselves — background, story |
| 2 | 5 min | Interviewer explains role/project context |
| 3 | 20–25 min | Technical Q&A across areas (pick based on time and candidate strengths) |
| 4 | 20–25 min | Task — pick ONE option from §6 |
| 5 | 5–10 min | Candidate's questions |

Total: ~55–65 minutes.

---

## 3. Segment 1 — Candidate Introduction (prep notes)

Let them drive. Listen for: did they own systems end-to-end, or execute tickets handed to them? Do they talk about trade-offs and their own decisions unprompted, or only describe outcomes? First signal on ownership/ambiguity tolerance, ahead of the behavioral questions in §5.6.

---

## 4. Segment 2 — Role Context (what to tell the candidate)

Read/paraphrase this to the candidate — built directly from `job_description.md`'s Overview/Customer/Project sections:

> "Our customer runs a large-scale platform that processes advertising and media operational data — specifically files called Postlogs and Prelogs, which arrive from many different stations and sources, each with its own layout and format. Today, a meaningful amount of that recognition and data-cleanup work still needs manual intervention whenever the system can't confidently make sense of a file. You'd be helping build and improve the ML/LLM systems that read these files, extract structured data out of them, and match key fields — like the station, advertiser, or creative — against our internal master records. Critically, the goal isn't just to automate this once: we want the system to get measurably better over time from its own production mistakes, not repeat the same failure indefinitely. You'd own this end-to-end — not just the models/prompts themselves, but the evaluation and monitoring around them, so any change we ship is provably an improvement before it goes live, not just different. It's a fully remote role, working closely with a Data Engineering team and with the customer's own stakeholders."

**Keep it at this level — do not go further.** Don't mention specific mechanisms (exact/fuzzy/semantic matching layers, an Observer/Candidate/Evaluate pattern, per-partition prompt families, MLflow/Glue/Airflow specifics) — that's exactly what Segment 4 Option C is designed to see the candidate reach on their own. Giving away the shape of the answer here would invalidate that task.

---

## 5. Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 questions total across categories based on time and where the candidate is weak/strong — this is a bank, not a script.

### 5.1 ML / Data Science & Statistics Fundamentals
- **Q:** A recognition system provides a confidence level for each of its predictions, a real value between 0 and 1. How would you decide the confidence threshold below which a record should be routed to a human instead of automated?
  **A:** Treat it as a precision/recall trade-off at the record level: pick the threshold using a labeled holdout set by plotting error rate vs. auto-processed volume, then pick the operating point matching an acceptable error tolerance (not just "highest accuracy") — ideally re-validated per segment (e.g. per station/layout) rather than one global cutoff, since error rates can vary a lot by segment.
- **Q:** What does it mean for a confidence score to be "calibrated," and why would that matter here specifically?
  **A:** Calibrated = the predicted confidence matches the empirical accuracy at that confidence level (e.g., among all 90%-confidence predictions, ~90% are actually correct). Matters because an uncalibrated score makes a single global threshold meaningless — the same "90% confidence" could mean very different real accuracy across formats/segments/model versions.
- **Q:** You want to know if a new prompt/model version is actually better than production, not just different. How do you test that rigorously?
  **A:** Run both versions on the same labeled holdout/gold dataset, compare a fixed metric with a significance test (or at minimum a large enough sample and confidence interval) rather than eyeballing a small sample — and ideally validate with a shadow-mode run on live traffic before fully promoting.
- **Q:** What's the risk of evaluating a new prompt only on examples the current system already gets right?
  **A:** Survivorship bias — you'd never see whether the new version fixes the failure cases that actually matter, or silently regresses on edge cases the sample doesn't cover; the eval set needs to be weighted toward known-hard/failure cases, not just representative of easy volume.
- **Q:** Give the formulas for precision and recall, and describe a scenario in this kind of system where you'd deliberately favor one over the other.
  **A:** Precision = TP/(TP+FP); Recall = TP/(TP+FN). Favor precision when a wrong automatic match/extraction is costly and hard to catch downstream (e.g., silently misattributing a record to the wrong advertiser); favor recall when missing a case that should route to human review is the more expensive failure.

### 5.2 LLM Engineering
- **Q:** What does "prompt versioning" actually require beyond just saving old copies of prompt text?
  **A:** Tracking prompt version alongside the model/config it was evaluated and promoted with, plus the eval results and dataset version tied to that prompt — so a production incident can be traced to exactly which prompt+model+eval-set combination was live, and rollback is unambiguous.
- **Q:** What is "grounding" in the context of LLM extraction, and why does it matter for something like structured data extraction from documents?
  **A:** Constraining the model's output to the actual source — e.g., requiring it to quote/point to the exact span it took a value from — rather than letting it infer plausible-sounding values. Matters because ungrounded extraction risks hallucinated fields feeding downstream systems as if they were ground truth. **Strong signal:** candidate says this should be enforced via a post-hook — programmatically verifying after generation that the quoted span actually exists in the source — not just prompted for and trusted.
- **Q:** How would you reduce the cost and latency of an LLM-heavy extraction pipeline without hurting quality?
  **A:** Look for: caching repeated/identical requests, routing easy/common cases to a cheaper or smaller model and reserving the strongest model for genuinely hard/low-confidence cases, batching where possible, and only falling back to a second (more expensive) model on low-confidence or failure — not calling the most expensive model on every record by default.
- **Q:** A layout the system has never seen before comes in and the LLM's structure recognition is wrong. What's your process for turning that failure into an improvement, safely?
  **A:** Capture the failure (and eventually its human correction) as a labeled example, add it to the gold/eval set, propose a candidate prompt change, evaluate the candidate against current production on the full gold set (not just the new example) to check for regressions elsewhere, and only promote if it's a net improvement — not patch the prompt for the one example in isolation.
- **Q:** What's "few-shot exemplar" selection, and what can go wrong if you pick exemplars carelessly?
  **A:** Choosing example input/output pairs embedded in the prompt to steer the model's behavior. Careless selection risks exemplars that don't represent the real distribution of inputs (biasing the model toward one layout/format), or that are themselves subtly wrong, propagating the error into every future output.
- **Q:** If different sources (e.g., different stations or agencies) have meaningfully different layouts or language, would you rather have one global extraction prompt or many source-specific ones? What's the trade-off?
  **A:** Many specialized prompts (one per source/partition, or a shared prompt with source-specific few-shot exemplars) generally recognize and handle format variation better than one prompt trying to cover every case — but each variant then needs its own versioning, evaluation, and promotion lineage, which is real operational overhead that scales with the number of partitions. A strong answer names both sides of that trade explicitly, not just "specialize everything."
- **Q:** Should prompt retraining/improvement run on a fixed schedule (e.g., weekly), or be triggered by something else? Why?
  **A:** Event-driven is generally better than a fixed calendar — trigger retraining when a live quality signal (e.g., rising low-confidence rate or error rate) crosses a threshold, rather than waiting for a scheduled retrain regardless of whether anything's actually degraded, or leaving a real regression live until the next scheduled cycle.

### 5.3 AWS & Production Infrastructure
- **Q:** What's the difference between an IAM role and an IAM user, and how would a service like Glue or Lambda authenticate to call Bedrock?
  **A:** A user is a persistent identity with long-lived credentials and directly attached permissions — meant for a person, or for an external system outside AWS with no way to assume a role. A role has no credentials of its own — it's assumed, temporarily, to get short-lived, auto-rotated credentials scoped to that role. For anything running on AWS compute — including Glue/Lambda calling Bedrock — this isn't a real "which one" choice: the standard is an execution role attached directly to the service, assumed automatically, no IAM user involved. A static user's keys embedded in a Lambda/Glue job is the anti-pattern to flag, full stop — there's no legitimate case for it there.
- **Q:** What would you use CloudWatch for in an LLM pipeline like this one, beyond basic uptime?
  **A:** Look for: tracking latency/error rate per pipeline stage, custom metrics for confidence-score distribution and LLM cost per run, and alarms on drift in those metrics (e.g., a spike in low-confidence rate) — not just "is the Lambda alive."
- **Q:** Why might you choose S3 as the landing zone for incoming files rather than writing straight into a database?
  **A:** S3 is cheap, durable, versionable storage for raw/unstructured input regardless of format, decouples ingestion from processing (can reprocess from the same raw file later), and integrates natively with event-driven triggers and orchestration tools — a database expects structure you don't have yet at landing time.

### 5.4 SQL
- **Q:** How would you write a query to find, for each Station, the most recent record and its value — without a full self-join?
  **A:** Use a window function (`ROW_NUMBER() OVER (PARTITION BY station ORDER BY timestamp DESC)`) and filter to `rn = 1`, rather than a correlated subquery or self-join on max(timestamp).
- **Q:** You need to detect duplicate records that should reconcile to a single entity but have slightly different spellings (e.g., station names). Is this a SQL problem, and if not, what is it?
  **A:** Exact-match dedup is a SQL problem (GROUP BY / window functions); near-duplicate detection with spelling variance is a fuzzy-matching problem — SQL alone can do exact and maybe basic pattern matching (LIKE, soundex-style functions), but real fuzzy/semantic matching needs a dedicated approach (edit-distance libraries, or an LLM/embedding-based matcher) — a good answer draws this line rather than trying to force it into SQL.

### 5.5 Python / Software Engineering Reasoning (no live coding — see §6 Option A for the deeper version)
- **Q:** (warm-up, optional) What's the time complexity of checking membership in a Python `list` vs. a `set`/`dict`, and why does that matter for a matching pipeline processing many records?
  **A:** `list` membership is O(n) (linear scan); `set`/`dict` membership is O(1) average (hash lookup). Matters directly for entity matching against master data — looking up each extracted value against master data should use a hash-based structure, not a list scan, or performance degrades badly as master data grows.

### 5.6 Ownership / Ambiguity (behavioral — no single correct answer; evaluate against criteria)
- **Q:** Tell me about a time you owned an ML system end-to-end, including a decision nobody explicitly asked you to make.
  **A (criteria):** Look for a concrete unprompted decision (not just "I built X"), the reasoning behind it, and ownership of the outcome/consequence.
- **Q:** Describe a time production feedback (errors, corrections, drift) told you something was wrong with a model/prompt in production. What did you do?
  **A (criteria):** Look for a real detection mechanism (not just "someone complained"), a concrete diagnosis process, and a fix that was actually validated before being shipped — not just a quick patch.

---

## 6. Segment 4 — Task (20–25 min): pick ONE

Choose based on time, what Segment 3 already covered well, and where you want deeper signal. Each option below is designed to work with nothing but a shared doc/whiteboard — none require the candidate to have live LLM API access.

### Option A — Code Reading & Complexity Reasoning

Show 1–2 of the snippets below (not all three unless time allows) and ask for time/space complexity and how they'd improve it. This is reasoning, not live coding — a correct verbal/written derivation is the deliverable.

**Snippet 1 — naive entity matching against master data**
```python
def match_records(extracted: list[dict], master_data: list[dict]) -> list[dict]:
    matched = []
    for record in extracted:
        for candidate in master_data:
            if record["name"] == candidate["name"]:
                matched.append({"record": record, "match": candidate})
                break
    return matched
```
*Expected answer:* O(n·m) time (n = extracted records, m = master data size) since each record scans the full master data list; O(n) space for the output. *Fix:* build a `dict` keyed by name from `master_data` once (O(m)), then look up each extracted record in O(1) average — O(n+m) total. Good candidates should also flag the real-world gap this leaves: exact-match only, no handling for near-duplicate/fuzzy names — setting up the natural segue to "what about fuzzy matches?" (a layered exact → fuzzy → LLM approach, mirroring the JD's own described matching strategy, is the strong answer if it comes up).

**Snippet 2 — repeated LLM calls without caching**
```python
def get_station_confidence(records: list[dict], llm_client) -> list[float]:
    scores = []
    for record in records:
        prompt = build_prompt(record["station_name"])
        response = llm_client.call(prompt)  # network call to an LLM API
        scores.append(parse_confidence(response))
    return scores
```
*Expected answer:* Not a Big-O trick question — the complexity is trivially O(n) calls, but the real issue is cost/latency: if the same `station_name` appears many times (likely, since station names repeat across many records), this makes a redundant paid LLM call for every occurrence instead of once per unique value. *Fix:* cache by `station_name` (a dict/memo), only call the LLM for unseen values — turns n calls into (number of unique stations) calls, often a large real-world cost/latency reduction. This tests whether a candidate reflexively evaluates *cost*, not just algorithmic complexity, when LLM calls are involved — a distinctly LLM-engineering-flavored complexity question.

**Snippet 3 — vector similarity via loop**
```python
def cosine_similarity(a: list[float], b: list[float]) -> float:
    dot = sum(x * y for x, y in zip(a, b))
    norm_a = sum(x * x for x in a) ** 0.5
    norm_b = sum(y * y for y in b) ** 0.5
    return dot / (norm_a * norm_b)

def most_similar(query_vec: list[float], candidates: list[list[float]]) -> int:
    best_idx, best_score = -1, -1.0
    for i, vec in enumerate(candidates):
        score = cosine_similarity(query_vec, vec)
        if score > best_score:
            best_idx, best_score = score, i
    return best_idx
```
*Expected answer:* `cosine_similarity` itself is O(d) — three separate passes over the `d`-length vectors (the dot product, and a norm for each vector), each a linear scan. `most_similar` calls it once per candidate, so total time is O(n·d) (n = candidates, d = vector dimension), O(1) extra space. Since the implementation is shown here, a candidate should be able to derive the O(d) per-call cost directly from reading it — this is a fair unprompted derivation, not one that needs domain knowledge of what cosine similarity means mathematically. *Fix:* vectorize with a matrix operation (e.g., stack candidates into a matrix and compute all similarities in one batched operation via NumPy/PyTorch) — same asymptotic complexity, but a large constant-factor speedup from vectorized/batched computation vs. a Python-level loop; at larger scale, an approximate nearest-neighbor index (e.g., FAISS) trades exactness for sub-linear lookup. A candidate who names either the vectorization win or ANN indexing (ideally both, at the right scale) is showing real production ML-engineering instinct, not just textbook Big-O.

### Option B — Prompt & Evaluation Design (no LLM access required)

Two concrete deliverables, both written down on a shared doc (not just discussed verbally) — no live LLM call needed for either:

1. **A draft prompt.** Ask the candidate to actually write out, in plain text on the shared doc, the prompt they'd give an LLM for this task — real instruction text, not a description of what a prompt would contain. It doesn't need to be long or polished; it needs to be concrete enough that you can point at specific lines and ask "why did you phrase it this way?"
2. **An evaluation plan.** Once they have a draft, ask: "Now imagine you want to change this prompt later. How would you know the new version is actually better before shipping it?" — this should be a short written or verbal plan (dataset, comparison method), not code.

> Prompt to give the candidate: "You're given messy, inconsistent Excel files (different layouts per station) that need to be turned into a structured table with a fixed set of expected columns. First, write the actual prompt you'd give an LLM to do this extraction. Then, tell me how you'd know a future change to that prompt actually made things better before shipping it."

**What to look for in the written prompt itself (mirrors the JD's stated responsibilities directly):**
- Schema-constrained output (e.g., forcing a specific JSON schema / structured output mode) rather than free-form text parsing
- Some notion of grounding — instructs the model to only extract values present in the source, ideally citing where from
- Handling for a layout it's never seen before — does the prompt itself address this, or does the candidate only mention a fallback verbally when asked?
- Few-shot exemplar placeholders, and awareness that exemplars should be representative and maintained/versioned, not static forever

**What to look for in the evaluation plan:**
- A gold/labeled dataset and a concrete comparison method (candidate vs. current production on the same set), not just "test it and see"
- Bonus signal: mentions prompts should probably be split per layout/station family rather than one global prompt trying to handle everything

**Above bar vs. at bar:** above-bar candidates independently raise evaluation methodology and low-confidence fallback without being prompted; at-bar candidates design a reasonable prompt strategy but need a nudge ("how would you know if it's actually better?") to get to the evaluation piece.

*Interviewer-only reference prompt* — one reasonable example hitting everything in the "what to look for" list above, to compare the candidate's draft against. Don't expect or require a match to this exact shape — it's a calibration anchor, not the only correct answer:

```
SYSTEM:
You are a data extraction assistant. You will be given the raw contents of a
semi-structured spreadsheet ("Postlog") describing an advertising placement.
Extract the following fields into the JSON schema below. Only use information
that is literally present in the input — never infer, guess, or generate a
value that isn't explicitly stated in the source.

Output schema:
{
  "station": string | null,
  "advertiser": string | null,
  "creative_id": string | null,
  "air_date": string (YYYY-MM-DD) | null,
  "spot_length_seconds": integer | null,
  "rate": number | null,
  "confidence": { "<field>": float 0.0-1.0 },       // one per extracted field
  "source_spans": { "<field>": string },            // exact cell/text it came from
  "needs_review": boolean                            // true if layout is unfamiliar
}

Rules:
1. For every non-null field, include the exact source cell/text in "source_spans."
   If you can't point to a specific source for a value, set it to null — don't guess.
2. Assign a confidence score per field reflecting how sure you are the mapping is
   correct, not just that a value exists.
3. If you can't confidently identify more than half the expected fields, set
   "needs_review": true instead of guessing at the rest.
4. Don't normalize or infer beyond what's stated (e.g., don't expand an
   abbreviated station name — extract it as-is and mark confidence accordingly).

Examples:
[few-shot exemplar 1 — input excerpt → expected JSON output]
[few-shot exemplar 2 — a different layout/station, to show generalization]

INPUT:
<raw file content>
```

What this example demonstrates, mapped back to the checklist: schema-constrained JSON output; grounding via the mandatory `source_spans` field plus the explicit "only use what's literally present" instruction; a fallback path via `needs_review` for unfamiliar layouts (rather than the prompt silently guessing); per-field confidence (ties directly to the earlier confidence-threshold question — this is what a threshold would actually be applied to); and exemplar placeholders showing awareness that a single static example set isn't enough across layouts. A candidate's draft doesn't need to hit all of these to be considered strong — use this as a checklist to see how many they got to on their own vs. needed a nudge for.

*Interviewer-only note:* the real target design (see `ml_platform_concept_arch_wbs.md`) uses a two-LLM pattern — an Observer that proposes candidate prompt changes from recent failures/gold corrections, and a holdout Evaluate step that compares the candidate against current production on a gold dataset before auto-promoting. A candidate who independently arrives at something structurally similar (a change-proposal mechanism decoupled from a separate, gating evaluation step) is showing unusually strong alignment with production LLM-ops practice — don't require the exact shape, but recognize it as an above-bar signal if it comes up unprompted.

### Option C — System Design: Self-Learning Recognition & Entity-Matching System

Give the prompt below, then let the candidate drive; ask clarifying questions as needed rather than pre-loading the "correct" architecture.

> "We receive semi-structured files (different formats/layouts depending on source) that need to become clean structured records, with key fields (like Station, Advertiser, Creative) matched against our internal master data. We want this system to improve over time: when it gets something wrong and a human corrects it, that correction should eventually make the system better, not just fix that one record. Design this end-to-end — from file arrival to final matched output, plus how it improves itself over time."

**Facilitation guide — track U (unprompted) / P (prompted) / M (missing) per topic:**

The candidate wasnt confortable explaining the idea and drawing the diagrama the same time. He probably never did a system design task before.
He is no confident, has difficulty to explain his ideas, and he does not have a graps on how LLM pipelines should be implemented, so it is a no hire.So he prefered just to explain his ideas instead of writing the diagrama, but it was difficult to follow and to undetstand his actual idea. 

| Topic | Nudge if it doesn't come up |
|---|---|
| Recognition/extraction step and its output format | "Once the file arrives, what's the first thing that happens to it?" |
| Confidence scoring on the extraction/match | "How do you know if the system got it right or not?" |
| Low-confidence fallback / human-in-the-loop | "What happens when confidence is too low to trust?" |
| Entity matching strategy — layered, not single-shot | "How do you match an extracted name to the right master-data record?" (probe further: "would you always use the same method?") |
| Per-source/per-partition specialization vs. one global model | "Would the same recognition logic work equally well for every source/layout, or would you handle them differently?" |
| Turning human corrections into future improvement | "A human fixes a mistake — then what? Does that just fix this one record, or does it help next time?" |
| Evaluation before shipping a change (gold set, candidate vs. production) | "How would you prove a new version is actually better before turning it on for everyone?" |
| What triggers a retrain/update — schedule vs. signal | "How often would you retrain or update this, and who/what decides it's time?" |
| Monitoring in production (drift, cost, latency) | "How would you know, weeks later, that quality has quietly gotten worse?" |
| Safe rollout of a new prompt/model version | "How do you roll out a change without risking a bad surprise in production?" |

**Above bar vs. at bar vs. below bar:**
- **Above bar:** independently designs the full loop — extraction → confidence-gated fallback → layered matching (cheap/deterministic first, escalating to LLM only when needed) → human correction feeding a labeled/gold dataset → a gated evaluation step comparing candidate vs. production → safe, gated promotion — and reasons about monitoring/drift and event-driven (not fixed-schedule) retraining without being asked.
- **At bar:** gets the core pipeline right (extraction → matching → output) but needs 1–2 nudges to get to the self-improving loop, evaluation, or monitoring pieces.
- **Below bar:** treats this as a one-shot extraction problem with no feedback loop, confidence handling, or evaluation methodology, even after nudges.

*Interviewer-only note — what the real target system actually does* (see `ml_platform_concept_arch_wbs.md`; do not summarize this to the candidate — it's the answer key, not the prompt): recognition and matching are separate LLM "families," each a set of per-partition (station/agency) prompts rather than one global prompt. Matching is layered — exact match, then fuzzy match, then a semantic-match LLM only as a last resort — mirroring cost-conscious cascading rather than calling an LLM on everything. An online quality monitor triggers retraining only on a live quality drop (event-driven, not scheduled); an Observer LLM proposes the candidate prompt from recent failures plus gold-corrected examples; a holdout Evaluate step scores the candidate against current production on a gold dataset; only a winning candidate is auto-promoted via a registry, with production untouched on any failed gate. A candidate whose own design converges on this shape (without having seen it) is a strong signal; one who gets close with a nudge or two is a reasonable at-bar outcome — this is a genuinely sophisticated design, so don't expect it fully unprompted from every strong candidate.

---

## 7. Segment 5 — Candidate Questions

No prep needed. Note whether questions focus on the actual technical/system challenges vs. purely logistics/comp.

- Motivation: professional growth. I maintaing what have built in my current project, so I want new challenges to work at.
- I only worked in small teams, but only ML team. So I do not have experience with big teams, but would not be a problem either. 

## 8. Scorecard

Score 1 (below bar) – 4 (exceptional) per row. This is the internal technical scorecard used to reach a hire decision — separate from `candidates/review_template.md`, which is the client-facing write-up format.

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Python / SWE reasoning (complexity, code quality) | High | 2           | |
| ML / Data Science & statistics fundamentals | High | 1           | |
| LLM engineering (prompting, evaluation, grounding, cost) | High | 1           | |
| Entity matching / ranking problem-solving | High | 1           | |
| Evaluation & MLOps discipline (holdout, shadow-mode, monitoring) | High | 2           | |
| System design synthesis (Segment 4, whichever option used) | High | 1           | |
| AWS / production infra | Medium | 2           | |
| SQL | Medium | 2           | |
| Ownership / ambiguity tolerance | Medium | 2           | |
| Communication clarity | Medium | 3           | |

**Guidance on weighting:** LLM engineering, evaluation/MLOps discipline, and system-design synthesis are the hardest to coach and most central to this specific role — a candidate can be coached up on AWS service specifics, but not easily on the instinct to demand a rigorous evaluation before trusting a change, or on judgment about where a self-improving loop can silently go wrong. ML/stats fundamentals and Python reasoning are foundational gates: weak here is a hard stop regardless of LLM-specific strength.

---

## 9. Decision guidance

- **Strong hire:** at or above bar on all High-weight rows; demonstrates the evaluation-before-promotion instinct unprompted (in Q&A or in the Segment 4 task); no more than one Medium-weight row below bar.
- **Borderline:** strong on either the ML/stats+Python foundation or the LLM-engineering/production side, but a clear gap on the other — worth a focused follow-up round on the specific gap (e.g., a deeper evaluation-methodology discussion, or a deeper AWS/production-infra discussion) rather than an outright pass.
- **No hire:** below bar on ML/stats fundamentals or Python reasoning (the hardest to coach), or shows no instinct toward rigorous evaluation/safe promotion even after multiple nudges in Segment 4 — both are central and not easily developed on the job for a Senior-level hire.

---

## 10. Notes for future rounds

This plan has not yet been run against a real candidate — recalibrate question difficulty and the task menu after the first few interviews, and note here if the client shares their own example exercise (as happened for the `python-fde-tabs` role) so it can be incorporated as a higher-confidence reference point.
