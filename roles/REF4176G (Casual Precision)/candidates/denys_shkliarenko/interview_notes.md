# Interview Notes — Denys Shkliarenko

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. All bank questions are pre-listed with their expected answer, so you only need to check the box for whichever ones you actually ask and fill in the candidate's answer under them — leave the rest untouched. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

- I joined my current company as Data analyst
- After a few years the ML team has been created and I joined it
- My first project was a very small team, only 2 people
  - It was a recommendation system for an e-commerce platform
  - Recommendation system aimed to provide product recommendations for the users
  - Worked in different stages, starting from feature engineering, experimentations up to deployments
  - Over the life cycle of the project I performed improvements, techinical quality improvement, business improvements or new features and so on
- Also have experiences with differnt doemains, for example:
  - Involved in the project for training/fine tunning LLM models
  - GPT-2 at the time, before chat GPT
- Working on not quite ML models, such as SW for internal needs
  - Some REST APIs, ETL pipelines
- When more powerful LLM models have arrived, I worked on a project that involved usage of the LLMs to actually get the results that we needed 
  - Improvement of the internal data, like catalogs 
  - Validation of customer IDs 
- My focus shifted towards MLOps and operations in the past few years
  - Working on Migrations from GCP to AWS
- About the LLM initiatives:
  - Validation of customer IDs:
    - An API that took the accepted document (a link to the document), and then performs OCR based on LLMs, extracts information from the document and then performs validation logic based on that information, basically classification. 
  - Another project was about enrichment/fixing data about project, because the scale of the catalog is too big, and we need to fix information from those products. It cannot be missing data, incorrect data and so on. So the idea of the system was to fix that. The project uses LLMs to fix the catalog. 
    - One part of the application was about product specification, so there were some sets of keys and values for a set of products, the idea was to search over the internet sources, reliable sources, like manufacturer sites and so on, to gather the information we need
    - First, a tool sends a search for some information over the internet, so to do that we need to create a prompt, and the prompt is generated based on the products.
    - After the response, we gather the information from internet, validate the information, compare with what we have in our DB, normalize it and so on
    - The system actually works in two modes: batch (specific set of products) and daily (runs for the newly added products)
    - * How the promts were used (Rodrigo's questions), improved and tracked?
      - We had templates for each products, that is the prompt template, that depends on a specific product or group of products. So templates that were configurable
      - We have not used any external tool for management, like MLOps
      - Prompts are versioned controled, but not as flexibile as it would be at MLFlow or something like that
      - How do we know if the prompt is good enough? Basically, during the offline development we were experimenting with prompts and see what happens, if the results were good and so on. So we did that in the offline part. 
    - * How did the information for the prompt templates were fetched?
      - It was based on rules, so basically no automated, but based on rules for different products. 
      - For some attributes is used some sort of RAG, not exactly, but we process a bunch of products from a specific category and we run a query to fetch information about a specific category. We run not in vector store, but in memory vector. It is a RAG system, but uses numpy instead of a vector DB. 
      - Fecth stuff with cosine similarity
      - Does not require a high load vector DB, just because is does not have scale

## Segment 2 — Role Context (interviewer-led, 5 min)

Candidate reactions / questions during this segment, if any:

-

## Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 across categories based on time and where the candidate is weak/strong. Only fill in the ones you actually ask.

### ML / Data Science & Statistics Fundamentals

- [ ] **Q:** A recognition system provides a confidence level for each of its predictions, a real value between 0 and 1. How would you decide the confidence threshold below which a record should be routed to a human instead of automated?
  > Expected: Treat it as a precision/recall trade-off at the record level: pick the threshold using a labeled holdout set by plotting error rate vs. auto-processed volume, then pick the operating point matching an acceptable error tolerance — ideally re-validated per segment (e.g. per station/layout) rather than one global cutoff.
  Candidate's answer:
  > We need samples, basically the golde set, and then compare the values. (did not mention golde set on it own, I have to prompt him). But could not explain how to get the thresholds specifically. What is the average confidence on that golden dataset, lets say 90% and human is 70% it means we need to ajust the threshold. 

- [ ] **Q:** What does it mean for a confidence score to be "calibrated," and why would that matter here specifically?
  > Expected: Calibrated = the predicted confidence matches the empirical accuracy at that confidence level. Matters because an uncalibrated score makes a single global threshold meaningless — the same "90% confidence" could mean very different real accuracy across formats/segments/model versions.
  Candidate's answer:

- [ ] **Q:** You want to know if a new prompt/model version is actually better than production, not just different. How do you test that rigorously?
  > Expected: Run both versions on the same labeled holdout/gold dataset, compare a fixed metric with a significance test (or at minimum a large enough sample and confidence interval), ideally validate with a shadow-mode run on live traffic before fully promoting.
  Candidate's answer: we first measure if a result is correct or not, for that we need an additional LLM to check or just a human to check the prompt. 
> He could not answer that correctly, only after I explained how we wdould that for a simple ML model and asked him.

- [ ] **Q:** What's the risk of evaluating a new prompt only on examples the current system already gets right?
  > Expected: Survivorship bias — you'd never see whether the new version fixes the failure cases that actually matter, or silently regresses on edge cases; the eval set needs to be weighted toward known-hard/failure cases.
  Candidate's answer: Ideally, we would want a dataset that includes different cases, with documents with different labels, ideally of all classes in this set, and ideally will be balanced, make sure to include all classes. But what would be the problem? If we don't include representative classes, we can have a problem that the model won't be able to generalize because part of the population is missrepresented in the golden set. 

- [ ] **Q:** Give the formulas for precision and recall, and describe a scenario in this kind of system where you'd deliberately favor one over the other.
  > Expected: Precision = TP/(TP+FP); Recall = TP/(TP+FN). Favor precision when a wrong automatic match/extraction is costly (e.g., silently misattributing a record to the wrong advertiser); favor recall when missing a case that should route to human review is the more expensive failure.
  Candidate's answer:

### LLM Engineering

- [ ] **Q:** What does "prompt versioning" actually require beyond just saving old copies of prompt text?
  > Expected: Tracking prompt version alongside the model/config it was evaluated and promoted with, plus the eval results and dataset version tied to that prompt — so an incident traces to exactly which combination was live, and rollback is unambiguous.
  Candidate's answer:

- [ ] **Q:** What is "grounding" in the context of LLM extraction, and why does it matter for something like structured data extraction from documents?
  > Expected: Constraining the model's output to the actual source — e.g., requiring it to quote/point to the exact span it took a value from — rather than inferring plausible-sounding values. Strong signal: mentions this should be enforced via a post-hook (verify the quoted span actually exists in the source), not just prompted for.
  Candidate's answer: First, we would need to provide a set clases the model should use, thinking about a classification prompt, so what classes we want. Then we need to provide some rules, that is, how it should classify that and this, how to handle that. Also, we could give some simple examples with input output that could be difficult ot the model, that is, edge cases. Sometimes is useful to add phrases like, if you cannot classify this, return an "unknonw class". Also we could tell the model about the format of the answer, that is, how the output should be formatted. 

- [ ] **Q:** How would you reduce the cost and latency of an LLM-heavy extraction pipeline without hurting quality?
  > Expected: Caching repeated/identical requests, routing easy/common cases to a cheaper/smaller model and reserving the strongest model for hard/low-confidence cases, batching, falling back to a more expensive model only on low-confidence or failure.
  Candidate's answer:

- [ ] **Q:** A layout the system has never seen before comes in and the LLM's structure recognition is wrong. What's your process for turning that failure into an improvement, safely?
  > Expected: Capture the failure/correction as a labeled example, add to the gold/eval set, propose a candidate prompt change, evaluate against current production on the full gold set (not just the new example), only promote if it's a net improvement.
  Candidate's answer:

- [ ] **Q:** What's "few-shot exemplar" selection, and what can go wrong if you pick exemplars carelessly?
  > Expected: Choosing example input/output pairs embedded in the prompt to steer behavior. Careless selection risks exemplars that don't represent the real input distribution, or are themselves subtly wrong, propagating the error into every future output.
  Candidate's answer:
> I asked how many examples: usually would use on example for each class, basically 1 example for classes. I don't have a specific range, but I guess it depends on the problem, if the pipeline is simple, we would use 1 example per class is ok, but if we have several classes and other nuances, than we should add more (cant specify a range, he can't graps the fact that you should not use more than 5 to 8 examples.)

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
  Candidate's answer: S3 is used for row data, with a specific schema. Structured data. Dependning on what we need we would place or some other bucket. There is no need to use database, because we do not have structure when files land. when we have the schema, then we can save it to the Database.

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
  Candidate's answer: List is O(n), dicts are O(1) and sets I don't remember, but probably O(n). It matters because if n is too big, then time will be long.  

### Ownership / Ambiguity (behavioral — no single correct answer; evaluate against criteria)

- [ ] **Q:** Tell me about a time you owned an ML system end-to-end, including a decision nobody explicitly asked you to make.
  > Criteria: A concrete unprompted decision (not just "I built X"), the reasoning behind it, ownership of the outcome/consequence.
  Candidate's answer:

- [ ] **Q:** Describe a time production feedback (errors, corrections, drift) told you something was wrong with a model/prompt in production. What did you do?
  > Criteria: A real detection mechanism (not just "someone complained"), a concrete diagnosis process, a fix actually validated before shipping — not just a quick patch.
  Candidate's answer:

## Segment 4 — Task (20–25 min)

Option used: [ ] A — Code Reading & Complexity   [ ] B — Prompt & Evaluation Design   [X] C — System Design

Notes on approach:

-

Facilitation tracking (Option C only) — mark **U** (unprompted), **P** (prompted), or **M** (missing) per topic:

| Topic | U/P/M | Notes |
|---|-------|---|
| Recognition/extraction step & output format | M     | |
| Confidence scoring | M     | |
| Low-confidence fallback / human-in-the-loop | M     | |
| Entity matching strategy — layered, not single-shot | M     | |
| Per-source/per-partition specialization | M     | |
| Corrections feeding back into improvement | M     | |
| Evaluation before shipping a change | M     | |
| Retrain trigger — schedule vs. signal | M     | |
| Production monitoring (drift, cost, latency) | M     | |
| Safe rollout of a new version | M     | |

Overall bar level for this segment: [ ] Above bar   [ ] At bar   [X] Below bar

## Segment 5 — Candidate Questions (5–10 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

-

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Python / SWE reasoning | High | 2           | |
| ML / Data Science & statistics fundamentals | High | 1           | |
| LLM engineering | High | 1           | |
| Entity matching / ranking problem-solving | High | 1           | |
| Evaluation & MLOps discipline | High | 2           | |
| System design synthesis | High | 1           | |
| AWS / production infra | Medium | 2           | |
| SQL | Medium | 1           | |
| Ownership / ambiguity tolerance | Medium | 1           | |
| Communication clarity | Medium | 3           | |

## Decision Lean

[ ] Strong hire   [ ] Borderline   [X] No hire

Justification:

- his experience in his CV is alligned with what we need, however in the interview his performance did not correspond. So I think he is a no hire.

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-
