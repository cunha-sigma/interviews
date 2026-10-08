# Interview Notes — Andrii Oshlianskyi

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. All bank questions are pre-listed with their expected answer, so you only need to check the box for whichever ones you actually ask and fill in the candidate's answer under them — leave the rest untouched. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

- Education in physics
- Shifted from academic to ML engineering early in career, rifht after graduation
- Has a lot of interest on Agentic systems
- Created sinthetic data to train models and stuff
- Development of solutions on mobila and AWS
- One project with document understanding already
- Previous project was about parsing invoices
- Currently, his project is working with user environment system
  - Parse documents and structuring it as an excel, parsing information from text
  - 

## Segment 2 — Role Context (interviewer-led, 5 min)

> "Our customer runs a large-scale platform that processes advertising and media operational data — specifically files called Postlogs and Prelogs, which arrive from many different stations and sources, each with its own layout and format. Today, a meaningful amount of that recognition and data-cleanup work still needs manual intervention whenever the system can't confidently make sense of a file. You'd be helping build and improve the ML/LLM systems that read these files, extract structured data out of them, and match key fields — like the station, advertiser, or creative — against our internal master records. Critically, the goal isn't just to automate this once: we want the system to get measurably better over time from its own production mistakes, not repeat the same failure indefinitely. You'd own this end-to-end — not just the models/prompts themselves, but the evaluation and monitoring around them, so any change we ship is provably an improvement before it goes live, not just different. It's a fully remote role, working closely with a Data Engineering team and with the customer's own stakeholders."

Candidate reactions / questions during this segment, if any:

-

## Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 across categories based on time and where the candidate is weak/strong. Only fill in the ones you actually ask.

### ML / Data Science & Statistics Fundamentals

- [ ] **Q:** A recognition system provides a confidence level for each of its predictions, a real value between 0 and 1. How would you decide the confidence threshold below which a record should be routed to a human instead of automated?
  > Expected: Treat it as a precision/recall trade-off at the record level: pick the threshold using a labeled holdout set by plotting error rate vs. auto-processed volume, then pick the operating point matching an acceptable error tolerance — ideally re-validated per segment (e.g. per station/layout) rather than one global cutoff.
  Candidate's answer: Mentioned precision and reclal metrics, golden. Mentioned would begin with a global threshold, and then analyze each category

- [ ] **Q:** What does it mean for a confidence score to be "calibrated," and why would that matter here specifically?
  > Expected: Calibrated = the predicted confidence matches the empirical accuracy at that confidence level. Matters because an uncalibrated score makes a single global threshold meaningless — the same "90% confidence" could mean very different real accuracy across formats/segments/model versions.
  Candidate's answer: means that the score should correspond to actual probability, basically bound the model score to reality systematically

- [ ] **Q:** You want to know if a new prompt/model version is actually better than production, not just different. How do you test that rigorously?
  > Expected: Run both versions on the same labeled holdout/gold dataset, compare a fixed metric with a significance test (or at minimum a large enough sample and confidence interval), ideally validate with a shadow-mode run on live traffic before fully promoting.
  Candidate's answer: improvement loop, first we define the golden set, needs to be representative, edge cases and all, versioned the current prompt, get a baseline and test the new prompt against that baseline. Look at cost and latency

- [ ] **Q:** What's the risk of evaluating a new prompt only on examples the current system already gets right?
  > Expected: Survivorship bias — you'd never see whether the new version fixes the failure cases that actually matter, or silently regresses on edge cases; the eval set needs to be weighted toward known-hard/failure cases.
  Candidate's answer:

- [ ] **Q:** Give the formulas for precision and recall, and describe a scenario in this kind of system where you'd deliberately favor one over the other.
  > Expected: Precision = TP/(TP+FP); Recall = TP/(TP+FN). Favor precision when a wrong automatic match/extraction is costly (e.g., silently misattributing a record to the wrong advertiser); favor recall when missing a case that should route to human review is the more expensive failure.
  Candidate's answer: Could not identitify precision and recall difference. He is good, but is not seasoned yet (5 years only), so he is not that used with classification problems yet.

### LLM Engineering

- [ ] **Q:** What does "prompt versioning" actually require beyond just saving old copies of prompt text?
  > Expected: Tracking prompt version alongside the model/config it was evaluated and promoted with, plus the eval results and dataset version tied to that prompt — so an incident traces to exactly which combination was live, and rollback is unambiguous.
  Candidate's answer:

- [ ] **Q:** What is "grounding" in the context of LLM extraction, and why does it matter for something like structured data extraction from documents?
  > Expected: Constraining the model's output to the actual source — e.g., requiring it to quote/point to the exact span it took a value from — rather than inferring plausible-sounding values. Strong signal: mentions this should be enforced via a post-hook (verify the quoted span actually exists in the source), not just prompted for.
  Candidate's answer: Basically bound the prompt to context, to help the model to have expected behavior

- [ ] **Q:** How would you reduce the cost and latency of an LLM-heavy extraction pipeline without hurting quality?
  > Expected: Caching repeated/identical requests, routing easy/common cases to a cheaper/smaller model and reserving the strongest model for hard/low-confidence cases, batching, falling back to a more expensive model only on low-confidence or failure.
  Candidate's answer: Reduce uncessary calls, use rules based approach, like routing, use big models only to big problems and all that. Finally most important is caching the results, parallelize calls. 

- [ ] **Q:** A layout the system has never seen before comes in and the LLM's structure recognition is wrong. What's your process for turning that failure into an improvement, safely?
  > Expected: Capture the failure/correction as a labeled example, add to the gold/eval set, propose a candidate prompt change, evaluate against current production on the full gold set (not just the new example), only promote if it's a net improvement.
  Candidate's answer:

- [ ] **Q:** What's "few-shot exemplar" selection, and what can go wrong if you pick exemplars carelessly?
  > Expected: Choosing example input/output pairs embedded in the prompt to steer behavior. Careless selection risks exemplars that don't represent the real input distribution, or are themselves subtly wrong, propagating the error into every future output.
  Candidate's answer: In prompting engineering, is like giving the model some input output examples to the model in the prompt. Respect format,layout, map the extractions. Should be representative, contain egdge case without leaking information to avoid bias. Cant increase indefintely 5 to 8. When I asked him why is not good to use as many examples as you want, he said "over represent a layout, ". 
> I asked how to use different examples, the mentioned RAG

- [ ] **Q:** If different sources (e.g., different stations or agencies) have meaningfully different layouts or language, would you rather have one global extraction prompt or many source-specific ones? What's the trade-off?
  > Expected: Many specialized prompts generally handle format variation better, but each needs its own versioning/evaluation/promotion lineage — real operational overhead that scales with partition count. Strong answer names both sides.
  Candidate's answer:

- [ ] **Q:** Should prompt retraining/improvement run on a fixed schedule (e.g., weekly), or be triggered by something else? Why?
  > Expected: Event-driven is generally better — trigger retraining when a live quality signal (e.g. rising low-confidence rate) crosses a threshold, rather than a fixed calendar that could leave a real regression live too long, or retrain when nothing's actually degraded.
  Candidate's answer:

### AWS & Production Infrastructure

- [ ] **Q:** What's the difference between an IAM role and an IAM user, and how would a service like Glue or Lambda authenticate to call Bedrock?
  > Expected: A user is a persistent identity with long-lived credentials (for a person, or an external system with no way to assume a role). A role has no credentials of its own — assumed temporarily for short-lived, auto-rotated credentials. For Glue/Lambda this isn't a real "which one" choice: standard is an execution role, no IAM user involved. Static user keys embedded in a Lambda/Glue job is the anti-pattern to flag.
  Candidate's answer: Correct answered

- [ ] **Q:** What would you use CloudWatch for in an LLM pipeline like this one, beyond basic uptime?
  > Expected: Tracking latency/error rate per pipeline stage, custom metrics for confidence-score distribution and LLM cost per run, alarms on drift in those metrics — not just "is the Lambda alive."
  Candidate's answer: For logs, for metrics, for dashboards but as a developer the most interesting is monitoring error rates, and ohter relevant stuff like cost, latency, and all that.

- [ ] **Q:** Why might you choose S3 as the landing zone for incoming files rather than writing straight into a database?
  > Expected: Cheap, durable, versionable storage for raw/unstructured input regardless of format, decouples ingestion from processing (reprocess from the same raw file later), integrates natively with event-driven triggers/orchestration — a database expects structure you don't have yet at landing time.
  Candidate's answer: S3 is cheap, scalable, you can version documents, attach metadata 

### SQL

- [ ] **Q:** How would you write a query to find, for each Station, the most recent record and its value — without a full self-join?
  > Expected: Window function (`ROW_NUMBER() OVER (PARTITION BY station ORDER BY timestamp DESC)`), filter to `rn = 1` — not a correlated subquery or self-join.
  Candidate's answer:

- [ ] **Q:** You need to detect duplicate records that should reconcile to a single entity but have slightly different spellings (e.g., station names). Is this a SQL problem, and if not, what is it?
  > Expected: Exact-match dedup is a SQL problem; near-duplicate/spelling-variance detection is a fuzzy-matching problem — SQL can do exact/basic pattern matching, but real fuzzy/semantic matching needs a dedicated approach. Good answer draws this line.
  Candidate's answer: Entity matching problem, not directly SQL. We can use for candidate generation for the duplicates, but after that we need to apply fuzzy matching, or embeddings or whatever.

### Python / Software Engineering Reasoning

- [ ] **Q:** (warm-up, optional) What's the time complexity of checking membership in a Python `list` vs. a `set`/`dict`, and why does that matter for a matching pipeline processing many records?
  > Expected: `list` membership O(n) (linear scan); `set`/`dict` membership O(1) average (hash lookup). Matters directly for entity matching against master data — should use a hash-based structure, not a list scan.
  Candidate's answer: Correctly named the complexities, and also correectly described how hashable objects are useful for matching

### Ownership / Ambiguity (behavioral — no single correct answer; evaluate against criteria)

- [ ] **Q:** Tell me about a time you owned an ML system end-to-end, including a decision nobody explicitly asked you to make.
  > Criteria: A concrete unprompted decision (not just "I built X"), the reasoning behind it, ownership of the outcome/consequence.
  Candidate's answer: It is not just 1 person doing everything, but I took the responsibility in a lot of things. I stayed at this project for 6 months. A language classifier, image of text and then you need to parse the text, the pipeline was very complicated, did not use big models, but portable models. My part was to classfiy the language on the image. Some languages are very similar to each others, the goal was to improve the previous models quality for 60 languages. I had to take decisions: full reconstruction of dataset, filter it, clean it, like russian data in ukranian data, mixed, needed cleaning. A lot of data for chinese, but struggled for several new ones, needed to generate synthetic data, designing the generation and all that, I also worked testing transformers and how they could help the project, with a lot of data complexity builds up, 

- [ ] **Q:** Describe a time production feedback (errors, corrections, drift) told you something was wrong with a model/prompt in production. What did you do?
  > Criteria: A real detection mechanism (not just "someone complained"), a concrete diagnosis process, a fix actually validated before shipping — not just a quick patch.
  Candidate's answer:

## Segment 4 — Task (20–25 min)

Option used: [ ] A — Code Reading & Complexity   [ ] B — Prompt & Evaluation Design   [X] C — System Design

Notes on approach:

- Apparently has never done a system desing task before. Did not know how to use draw.io very well.
- However, he did draw the whole pipeline, with pre processing, matching, candidate selectinong steps, with score calibration, threshold definition and also the human loop to correct files and reuse that to improve prompts and reploy it to prd. 
- His solution diagram is in the file `Oshlianskyi_Sep22.drawio.png`
- I think he did very well. He actually seems to know what he is talking about. However, he is not a seasoned engineer yet, as he as 5+ years of experience only

Facilitation tracking (Option C only) — mark **U** (unprompted), **P** (prompted), or **M** (missing) per topic:

| Topic | U/P/M | Notes |
|---|-------|---|
| Recognition/extraction step & output format | U     | |
| Confidence scoring | U     | |
| Low-confidence fallback / human-in-the-loop | U     | |
| Entity matching strategy — layered, not single-shot | U     | |
| Per-source/per-partition specialization | M     | |
| Corrections feeding back into improvement | U     | |
| Evaluation before shipping a change | U     | |
| Retrain trigger — schedule vs. signal | M     | |
| Production monitoring (drift, cost, latency) | U     | |
| Safe rollout of a new version | U     | |

Overall bar level for this segment: [ ] Above bar   [X] At bar   [ ] Below bar

## Segment 5 — Candidate Questions (5–10 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

- 

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Python / SWE reasoning | High | 3           | |
| ML / Data Science & statistics fundamentals | High | 3           | |
| LLM engineering | High | 3           | |
| Entity matching / ranking problem-solving | High | 4           | |
| Evaluation & MLOps discipline | High | 4           | |
| System design synthesis | High | 3           | |
| AWS / production infra | Medium | 4           | |
| SQL | Medium | 4           | |
| Ownership / ambiguity tolerance | Medium | 3           | |
| Communication clarity | Medium | 3           | |

## Decision Lean

[X] Strong hire   [ ] Borderline   [ ] No hire

Justification:

-

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-
