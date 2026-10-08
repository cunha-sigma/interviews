# Interview Notes — Serhii Liutov

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. All bank questions are pre-listed with their expected answer, so you only need to check the box for whichever ones you actually ask and fill in the candidate's answer under them — leave the rest untouched. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

- 9+ years working as data science
- Has worked with data extraction, feels conformatable and have worked before with that
- Experience with NLP
- Banking, agriculture, software companies

- Last experiences:

- Migrate pipeline to another, to github to gitlab
- Knows angular, blockchain companies, resposbile for data collection, gRPC endpoints, using GPT to transform data
- Machine Learning models to generate metrics and created power bis to visualize data

* I find very difficult to follow his english, but maybe that is not a problem

- Created a recommendation system, related with blockchain
- Developed a system to migrate projects from github to gitlab
- Analyze dependencies between files and all that,
- Improve documentation
- Executed a translator agent to write diagrams 

- Developed system to analyze engineers work, like from tasks tickets and commits and all that
- Create metrics to detect task pipelines in Jira (?) using LLMs

- Motivations
- Why do you want to work in this project?
  - Because I am smart and want to work with ML, hahaha. Joking, but I think that this project is very interesting because involves a relevant field
  in which I want to further develop myself. It will be quite challenging from one point of view, but from another is an opportunity for me to polish
  my Agentic and LLM experience. But the most challenging will be create the whole system. 

- Would it be ok for you to work at 9 and 10pm? Out of your working-hours as the project is in east europe and you are localted in Ukraine
  - (Apparently, yes - It is difficult to follow what he says. His english is not great, he has heavy accent, dicction is not great, and the flow of his narrative is confused.

- The size of the teams you ahve been working at?
  - Depends on the company, the blockchain company was a startup, up to 30 people in the company. Today I work at a smaller company, it is ok for me because I can work with ok. But I worked with big companies with couple of 1000 people as well as in KPMG.
  * He got the question wrong, Zoriana asked about the team he works, not the company
  - In general I feel more confortable in big companies.
- 


## Segment 2 — Role Context (interviewer-led, 5 min)

> "Our customer runs a large-scale platform that processes advertising and media operational data — specifically files called Postlogs and Prelogs, which arrive from many different stations and sources, each with its own layout and format. Today, a meaningful amount of that recognition and data-cleanup work still needs manual intervention whenever the system can't confidently make sense of a file. You'd be helping build and improve the ML/LLM systems that read these files, extract structured data out of them, and match key fields — like the station, advertiser, or creative — against our internal master records. Critically, the goal isn't just to automate this once: we want the system to get measurably better over time from its own production mistakes, not repeat the same failure indefinitely. You'd own this end-to-end — not just the models/prompts themselves, but the evaluation and monitoring around them, so any change we ship is provably an improvement before it goes live, not just different. It's a fully remote role, working closely with a Data Engineering team and with the customer's own stakeholders."

Candidate reactions / questions during this segment, if any:

- He seems interested in working with Agents and LLMs, so he was intertested.

## Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 across categories based on time and where the candidate is weak/strong. Only fill in the ones you actually ask.

### ML / Data Science & Statistics Fundamentals

- [ ] **Q:** A recognition system provides a confidence level for each of its predictions, a real value between 0 and 1. How would you decide the confidence threshold below which a record should be routed to a human instead of automated?
  > Expected: Treat it as a precision/recall trade-off at the record level: pick the threshold using a labeled holdout set by plotting error rate vs. auto-processed volume, then pick the operating point matching an acceptable error tolerance — ideally re-validated per segment (e.g. per station/layout) rather than one global cutoff.
  Candidate's answer: Mentioned calibration, mentioned analzing it from different perspectives

- [ ] **Q:** What does it mean for a confidence score to be "calibrated," and why would that matter here specifically?
  > Expected: Calibrated = the predicted confidence matches the empirical accuracy at that confidence level. Matters because an uncalibrated score makes a single global threshold meaningless — the same "90% confidence" could mean very different real accuracy across formats/segments/model versions.
  Candidate's answer: IMpacts the outputs of the models, erros can propagate. Bascically we need to do it to be sure the model works.

- [ ] **Q:** You want to know if a new prompt/model version is actually better than production, not just different. How do you test that rigorously?
  > Expected: Run both versions on the same labeled holdout/gold dataset, compare a fixed metric with a significance test (or at minimum a large enough sample and confidence interval), ideally validate with a shadow-mode run on live traffic before fully promoting.
  Candidate's answer: mentioned golde set and metrics to asure

- [ ] **Q:** What's the risk of evaluating a new prompt only on examples the current system already gets right?
  > Expected: Survivorship bias — you'd never see whether the new version fixes the failure cases that actually matter, or silently regresses on edge cases; the eval set needs to be weighted toward known-hard/failure cases.
  Candidate's answer:

- [ ] **Q:** Give the formulas for precision and recall, and describe a scenario in this kind of system where you'd deliberately favor one over the other.
  > Expected: Precision = TP/(TP+FP); Recall = TP/(TP+FN). Favor precision when a wrong automatic match/extraction is costly (e.g., silently misattributing a record to the wrong advertiser); favor recall when missing a case that should route to human review is the more expensive failure.
  Candidate's answer: he answered correclty, knows what precision and recall is

### LLM Engineering

- [ ] **Q:** What does "prompt versioning" actually require beyond just saving old copies of prompt text?
  > Expected: Tracking prompt version alongside the model/config it was evaluated and promoted with, plus the eval results and dataset version tied to that prompt — so an incident traces to exactly which combination was live, and rollback is unambiguous.
  Candidate's answer:

- [ ] **Q:** What is "grounding" in the context of LLM extraction, and why does it matter for something like structured data extraction from documents?
  > Expected: Constraining the model's output to the actual source — e.g., requiring it to quote/point to the exact span it took a value from — rather than inferring plausible-sounding values. Strong signal: mentions this should be enforced via a post-hook (verify the quoted span actually exists in the source), not just prompted for.
  Candidate's answer: He did not know the conpcet, but knows that you need to give the model concept and constraints into the prompt so the model can behave correctly. 

- [ ] **Q:** How would you reduce the cost and latency of an LLM-heavy extraction pipeline without hurting quality?
  > Expected: Caching repeated/identical requests, routing easy/common cases to a cheaper/smaller model and reserving the strongest model for hard/low-confidence cases, batching, falling back to a more expensive model only on low-confidence or failure.
  Candidate's answer: understand complexity of the project and the problem, in general we need to improve the process, understand what layouts we have, which rules to apply and so on. Come up with rules and templates bla bla bla. At the end, he meant routing complex cases to expensive models and simple ones to cheapes models. Very difficult to follow and udnerstand.

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
  Candidate's answer: Would use S3 because data is structured but you don't have it when you get data. 

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

Option used: [X] A — Code Reading & Complexity   [ ] B — Prompt & Evaluation Design   [ ] C — System Design

Notes on approach:

- He correctly identified O notation, but he is not that good doing that. It was a O(n) time and O(n) space function, the one with the LLM call. He also mentioned caching and parallelization. Did a good job.

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

Overall bar level for this segment: [ ] Above bar   [X] At bar   [ ] Below bar

## Segment 5 — Candidate Questions (5–10 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

- What is the composition of the project? How many engineers:
  - 2 data scientists, half an arquitect and pottentially you, but in the future more people might join the team

- How long have you been working on this project?
  - 6 months with different ideas so far, the project is about to take form now, so you are joining in the beggining

- What is the frequency of the data?
  - I think is not that frequent, but I cannot give a precise answer

- What is the perfect candidate for this project?
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

[ ] Strong hire   [X] Borderline   [ ] No hire

Justification:

- He has 9+ years of experience, has broad knolwdege on models, data, AWS stuff, LLMs, agents, quite complete. However, communication was awful.
Very difficult to follow his english. His diction was not good, his english neither. So I could not make notes on a lot he spoke because I could not understand him very well. Hi even repeated correct answers he had already told me (according to him) but I could not get it right. His narrative is also very confuse. Cannot focus on answering a direct question, that requires a direct answer. Goes very far on the answer, diverging a lot. Difficult to follow him, I have to interrupt him few times prompting for him to be more concise in his answers, but did not improve a lot. He repeats himself a lot without going anywhere, just add bla bla bla. Not easy.

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

- He seems like a competent engineer. However, communication is a problem. His attitude is a bit arrogant as well, but that is a soft impression on my own.
- I think he should move to the next phases because he technically fits, so if everyelse thinks his communication is enough, I can tell the tech part it is.
