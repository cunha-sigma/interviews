# Interview Notes — Oleksii Minenko

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. All bank questions are pre-listed with their expected answer, so you only need to check the box for whichever ones you actually ask and fill in the candidate's answer under them — leave the rest untouched. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Candidate Resume Summary

Senior ML/AI Engineer, ~8 years (Kyiv, Ukraine). Current: ML Engineer at **DataZ** (Jan 2025–present) — OSINT data pipelines, LLM-based classifiers (identity-matching, credit-scoring, content detection), PySpark on AWS, MLflow-based prompt/eval tracking. Before that: 5 years at **CaboodleAI** (2020–2024) — production ML pipelines, a deployed recommendation system, Airflow-orchestrated retraining workflows. Earlier: Data Scientist roles at **Jiji** (recs/anti-fraud/traffic-control for a marketplace, 2018–2022, overlaps with CaboodleAI — worth clarifying), **Active Wizards**, and **SynergyOne** (credit scoring). M.Sc. Actuarial & Financial Mathematics, Taras Shevchenko National University of Kyiv.

**Fit read for this role:** strong modeling/ML-pipeline background, but the CV shows no Kubernetes, no model-registry/lifecycle tooling (MLflow appears only as an eval-tracking tool, not a registry), no multi-tenant isolation experience, no Terraform, and no GCP (AWS-only). This is a genuine platform-engineering-vs-modeling profile gap worth probing directly early on, not assuming either way. Full detail in `cv.md`.

*(Pre-filled from `cv.md` before the interview — full detail there if needed.)*

---

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

-

## Segment 2 — Role Context (interviewer-led, 5 min)

Candidate reactions / questions during this segment, if any:

Bohdan is doing the RH interview part first, so I will add notes from that part here too:

- Bohdan: it is a new project, their application manages advertisement campaigns. As we have expertise on this area, they requested us to help them to build an ML Platform on top of they already have. So predictive models on top of their already existing platform.
- Candidate: what does I want to predict in that platform?
  - Mainly about predicting the audience, basically "who should I show what?". The objetive is to get more revenue from their solutions. 
  - Predictions to help with placement of ads and so on
- Candidate: sounds like a recommentation system
  - Somehow, it is a customer recommendation, like understanding how each user would respond to specific ads.
- Bohdan: team will have ML Engineers and a couple of Data Scientists to help with model development
- Bohdan: we are aiming for around 1 year of contract, and we expect a first version MVP by early 2027. The idea is to build a long term relation with the client.
- Bohdan: please tell us about your story
- Candidate: the most recent projec I worked on it was a platform, we collected data from open sources, social network places like Linkedin and so on, my part was analyizing the problems using AI, I was doing matching process using landgraph orcherstration for our AI. An agent would gather data, fullname, e-mail, surname, mobile phone, and after data we would analyze, get data, and so on, and them match the candidates with the JD description. Previously I worker on a news aggregator, my part was getting relevant news and aggregating stuff. We didn't write our own scrapers, we used ready ones. I built pipelines to gather data and train the models and predict if an article was useful for client or not. Data from google ads, store it and make it available for ML usage. before that I was working with a recommendation system, personal recommendation.
- Bohdan: we would you expect for a future project, list of green and red flags for you:
  - The company will have stable finances
  - Nice people to work with
- Bohdan: Why did you shifter from game testing to data science / ML?
  - Well, I began testing games, but once as I found a Data Science job I switched.
- Bohdan: tell me a little bit about your learning approach, how to keep knowledge uptated and so on. AI helps, but overall how you do it
  - Inteviews keep my knowledge in shape
  - Liknedin
  - Telegram publics related with ML
- Bohdan: how do you use AI in your daily-basis, or interesting cases, projects and sutff like that regarding generative AI to help your work
  - Once I tested if we could use AI or fine tunned AI to predict credit score based on person basic info. I had the training dataset, prepared data, tuned the model, recall was low, wasn't working fine. The idea was not to transform or hold all data pipeline, do not need any model maintainence. 

## Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 across categories based on time and where the candidate is weak/strong. Only fill in the ones you actually ask.

### Python & Platform Engineering

- [ ] **Q:** Describe what do you understand by "MLOps".
  > Candidate's answer: model maintaing, basically. For some model, is trainined every day, we need to version the model, if we need to rollback to previous one, which dataset a model was used for training. Life cycle is gathering the data, the starting point, get data we need, second step is understand data, what model we need, then model training, evaluation, fine tunning HP, saving it, after that we use models to predict results, store the results. ANalyze the model, get metrics, fix bugs, analysis to make the model improvents. 

- [ ] **Q:** The job description says "you write the platform, not only its YAML." What's the practical difference between a platform engineer who writes YAML and one who writes the platform?
  > Expected: YAML wires together existing primitives; writing the platform means building the real logic underneath (retry/backfill logic, registry clients, promotion-gate code, custom operators) — actual software engineering, not just config.
  Candidate's answer: write micro services and supporting them with all that comes with it, data storages, rest aPIs in it. 

- [ ] **Q:** You're handed a data scientist's training script to put into production. What are the first things you check to figure out what will break?
  > Expected: Hardcoded paths/credentials, non-deterministic behavior, notebook-style global state, no error handling on external calls, assumptions that won't hold at scale, compute/memory mismatches, no separation between training-time and serving-time code.
  Candidate's answer: test if it works, which python version do I need, what libraries, usually I would put it into docker, run it from there, also I need check how data is gathered, connections are working, also I need to understand storage size, how powerful the infra will need. 

- [ ] **Q:** What do you understand by reproducibility, how can you enforce it, and why does it matter enough to be an explicit requirement here?
  > Expected: Being able to exactly reconstruct what was trained/served, not approximate it. Enforce via pinned dependencies/environments, immutable container images tied to a code commit, versioned/snapshotted training data, and recording which code+environment+data combination produced each served model. Matters because debugging a production incident or answering a compliance question requires reconstructing exactly what was served, not guessing.
  Candidate's answer: reproducibility I need to reconstruct every step on the process. Enforcing it by setting seeds, using the same training dataset, the same data structure, no problems with spliting data. Because we would loose money if something is wokring and is not reproducible. We could loose control and money without reproducibility. 

### Kubernetes & Docker

- [ ] **Q:** A pod won't start — CrashLoopBackOff. Walk me through how you'd debug it.
  > Expected: `kubectl describe pod` for events, `kubectl logs --previous`, check resource limits vs. actual need, verify entrypoint/command, check mounted config/secrets.
  Candidate's answer: never used k8s

- [ ] **Q:** What's the difference between a Kubernetes Job and a CronJob? Follow-up 1: Say a CronJob runs a retraining pipeline every hour, but one run takes longer than expected and is still going when the next hour's run is due to start — what could go wrong, and how would you configure the CronJob to prevent it? Follow-up 2: a Job and a Deployment are both Kubernetes objects that manage pods for you — what's the actual difference between them?
  > Expected: Job runs once; CronJob schedules Jobs recurringly. On the overlap: default `concurrencyPolicy` is `Allow`, so two copies of the pipeline run at once and can race/double-process; prevent with `Forbid` (skip) or `Replace` (kill old, start new) plus a sensible `startingDeadlineSeconds` — though this doesn't replace making the job idempotent, since a retry can still cause it to effectively run twice. Follow-up: a Deployment keeps a desired number of pods running indefinitely (replaces dead ones forever); a Job runs pods until the task completes successfully and then stops.
  Candidate's answer:

- [ ] **Q:** In Kubernetes, what are resource requests and limits — what does each one actually control? Follow-up: what specifically goes wrong for a training job, and for other things sharing its node, if requests are set too low, if the limit is set too low, or if no limit is set at all?
  > Expected: Requests = scheduler's placement reservation, not a runtime cap. Limits = enforced runtime ceiling (memory over limit → OOMKilled; CPU over limit → throttled). Misconfigurations: requests too low → node oversubscription, instability for everyone on that node; limit too low → the job itself gets OOMKilled; no limit at all → the job can consume all free node resources and starve other pods on the same node.
  Candidate's answer:

### CI/CD for ML & Model Registry

- [ ] **Q:** Assume that in your company a data scientist trains a model in a notebook, and once it looks good, someone manually exports the model file and uploads it to a production location. What would you need to build so that step is fully automated instead, and why does that matter?
  > Expected: Fully automated path from versioned code — training, automated evaluation against a gate, automated promotion, no human touching a file in between. Matters because that manual step is exactly where reproducibility and audit trail break down.
  Candidate's answer: it matters to avoid mistakes from people, that is, manual is error-prone. I would automate with python scripts that automatically loads models and uploads it to correct bucketm following configurations from a file. Why do that, avoid mistakes, with hundred buckets. 

- [ ] **Q:** What's the difference between versioning a model and having a full model lifecycle?
  > Expected: Versioning tracks artifacts; a lifecycle adds lineage, documented evaluation tied to that version, and an auditable state machine with real gates between states.
  Candidate's answer:

- [ ] **Q:** How would you design an evaluation gate that has to be auditable — someone outside the team needs to check why a model got promoted?
  > Expected: Record the metric(s) evaluated, candidate vs. production values, dataset/version used, and a timestamp — stored durably and queryably, not a Slack message.
  Candidate's answer: basically we have a new trained model, we need to run by a trigger when a new model is trained, then basically some docker image could run the modle to gather the metrics we need and get metrics, build the report and then automate the delivery. I would store the data the model was trained at, what data was used for evaluation, search for the report and understand what happen. The report would just be a file, something like JSON. 

### Orchestration, Backfills & Idempotency

- [ ] **Q:** What does it mean for a backfill to be idempotent, how would you actually make one idempotent, and why does that matter specifically for a system running on hourly/daily/weekly planes?
  > Expected: Running the same backfill twice (retry, re-trigger, overlap) produces the identical end state, not duplicated/corrupted data. This isn't a Kubernetes setting — the job's own logic has to guarantee it, and the mechanism depends on what the job does: upsert keyed by a stable identifier (e.g. the hour) for writes, partitioning output by the logical unit processed (not by run time) for files, an idempotency key passed to any external side-effecting call. Matters here because with three overlapping schedules, retries are routine — without idempotency, a retried hourly job can silently double-count into a daily aggregate before anyone notices.
  Candidate's answer: it is important because when we build a data pipeline, gather data, transform it for latter model usage, we need that because we may mess up the data otherwise, to enforce it we need to correctly use data parsing with airflow, we need that to guarantee that we will always get the same data and reproduce model results. 

- [ ] **Q:** A daily job depends on all 24 hourly jobs from the previous day succeeding. Two failed. What are your options, and what would you actually do?
  > Expected: Block until backfilled (safest, delays everything); run on partial data flagged as degraded; auto-retry with backoff and a deadline. Good answer names the trade-off explicitly.
  Candidate's answer:

### Monitoring & Observability

- [ ] **Q:** What's the difference between feature drift and prediction drift, and why do you need to watch both?
  > Expected: Feature drift = input distribution shifted; prediction drift = output distribution shifted. They can move independently — watching only one can miss the other's failure mode.
  Candidate's answer: I have experience with manual checks. Feature drift data is changing over time, distribution is changing, the input. For the ouutput, we are monitoring the output. Feature drift would be for instance changing its ditribution, range of values. The output drift is usually caused by an input drift. 

- [ ] **Q:** What's train/serve skew, and give a concrete way it could happen even if the model code itself hasn't changed?
  > Expected: Serving-time features don't match training-time features. E.g. a feature computed via a different code path in serving than in training (batch/offline vs. real-time).
  Candidate's answer:
  

### Cloud, IaC & Linux

- [ ] **Q:** Why does the job description ask for Terraform specifically rather than just someone comfortable in the GCP console?
  > Expected: Console changes are manual/undocumented/unreviewable. IaC makes changes reviewable, reproducible, auditable — directly relevant to this job description's isolation-evidence and reproducibility asks.
  Candidate's answer: with complicated infrastructre, you need to set up everything correclty, make devops work better, but using Terraform I can do that in a much simpler manner. 

- [ ] **Q:** A training job on Linux is silently getting OOM-killed with no clear error in the application logs. How do you find that?
  > Expected: Check `dmesg`/`/var/log/syslog` for OOM-killer entries, `kubectl describe pod` if containerized (shows OOMKilled), correlate kill timestamp with memory usage over time.
  Candidate's answer:

### Multi-Tenant Isolation & Cost

- [ ] **Q:** Say your platform trains and serves a separate model for each of your customers, so that every customer's model is only ever supposed to use that customer's own data — no customer should ever be able to see or affect another customer's model or data. A compliance auditor asks you to prove that this separation actually holds. Saying "we use separate namespaces" isn't proof on its own — what concrete evidence would you actually show them?
  > Expected: Infrastructure-as-code/config showing per-tenant boundaries, identity/access-management policies scoped per tenant, logs/audit trails showing no cross-tenant access ever occurred, documentation mapping resources to tenants — concrete artifacts, not just a claim about the design.
  Candidate's answer:

- [ ] **Q:** Say your platform runs training and hourly scoring for many different customers, all on the same shared cluster — there's no dedicated hardware per customer. How would you figure out what it actually costs to run this for one specific customer? Follow-up: if you didn't have any cost-allocation tooling available, how would you get that same answer yourself?
  > Expected: Two things needed together: (1) every job labeled with a customer identifier, consistently, as an ongoing practice; (2) a mechanism combining those labels with actual usage (CPU/GPU-seconds) and real pricing to compute a cost — a cloud-native feature (GKE cost allocation, EKS split cost allocation) or a tool (Kubecost/OpenCost), since a label alone doesn't split a shared node's bill on its own. Follow-up (no tooling): track compute-time per job yourself at the orchestration layer, apportion the cluster's real bill proportionally to each customer's share of total compute-time — the same logic the tools automate.
  Candidate's answer:

### Ownership / Ambiguity (behavioral — no single correct answer; evaluate against criteria)

- [ ] **Q:** Tell me about a time you owned a piece of ML infrastructure end-to-end, including a decision nobody explicitly asked you to make.
  > Criteria: A concrete unprompted decision, the reasoning behind it, ownership of the outcome/consequence.
  Candidate's answer: on a previous project I build an ML pipeline to train if a model was relevant to a client or not. Was a crontab task on EC2, runs once a week. So I trained models for every customers, the models was stored on a bucket. Basically, I gathered the data, did some preprocessing, working with text, so was my decision to use a sentence encoder, after that we went from text to embedding, based on data I tried to train some models, but I understaood that they did not need complex models and simpler models would work. Binary classifcation. So I used simpler models, it worked faster, cheaper, keeping the quality. Model metrics were store into MongoDB.

- [ ] **Q:** Describe a time a drift or monitoring alert fired in production. What did you actually do?
  > Criteria: A real detection mechanism, a concrete diagnosis process, a fix actually validated before shipping.
  Candidate's answer: from time to time our customers would tell us that we did not have relevant articles, after that the first thing was look into the mongoDB, check the metrics and lineage, usually it was good for one class and bad for another. Usually data was imballanced, our client in order to work our system correclty, needed to label manually documents, but mostly they did that not not relevant articles. So I had to understand if it was a training problem or data problem to eventually undersatnd it wwould need to use synthetic data to account for those more relevant data, so we used synthetic data to improve the mmodel. 

### Self-Organization (behavioral — no single correct answer; evaluate against criteria)

- [ ] **Q:** In this role, you'd often be working on several different things at once — running training pipelines, keeping track of model versions, monitoring the system, tracking costs, and writing documentation for how everything works. Tell me about a time you had several different tasks competing for your attention at once. How did you decide what to work on first, and how did you make sure the other tasks didn't get forgotten?
  > Criteria: A real prioritization framework, evidence deprioritized items weren't dropped, some system for tracking commitments.
  Candidate's answer: usually when we have critical client request, this would be priority number one. Fix that stuff. Monitoring is separate tab on google chrome, so always looking at that, also slack notifications, always paing attention to that, lets say if we have critical parts of system I would ne to prioritize that over everything else. Regular task. 

- [ ] **Q:** Tell me about a time you caught a subtle bug or configuration issue in a production ML system that wasn't obvious and nobody else had noticed.
  > Criteria: A specific example, a real explanation of how they found it (not luck), genuine attention to detail.
  Candidate's answer: A service for recommendation stoped working, basically I started diving on it to understand what happent. It worked on my machine, but no tin the ssystem. The problem was dependencies. I would use poetry to avoid that problem. 

- [ ] **Q:** How do you keep up with MLOps tooling and practices, given how fast this space moves?
  > Criteria: A real, specific habit or practice, not a vague "I read blogs."
  Candidate's answer: Subscriptions on my e-mail, linkedin, also some chats in telegram. Also I have friends that works with that asl well, so I share knowledge with them. On demand, I do not have a day in the week in which I study those things. 

## Segment 4 — Task (20–25 min)

Option used: [ ] A — Config Reading & Debugging   [ ] B — Model Registry & Promotion Design   [X] C — System Design

Notes on approach:

- Proposed using airflow to automate pipelines, preproc and stuff and save data back to s3.
- Then another airflow pipeline reads data from S3 and saves the model back to s3, one folder per customer, inside of them one folder for each version. No model restry at all
- Deployment very weak, could not correctly propose an API solutin for the platform or an organized way to server the batch serving option.
- As far as I understand, he struggles to desing the offline and online pipelines for a system that would use a single model for a single purpose, not to mention a platform with multiple users, models, business goasl and so on. I feel like he never actually productionized an ML model, at least not under real contrains. 

Facilitation tracking (Option C only) — mark **U** (unprompted), **P** (prompted), or **M** (missing) per topic:

| Topic | U/P/M | Notes                                                                                                  |
|---|-------|--------------------------------------------------------------------------------------------------------|
| Training orchestration & scheduling | P     |                                                                                                        |
| Idempotent backfills | M     |                                                                                                        |
| Model registry & versioning | P     | only saves models to s3, no model registry or more organized approach                                  |
| Evaluation gate before promotion | M     |                                                                                                        |
| Safe rollout (shadow / champion-challenger) | M     |                                                                                                        |
| Per-tenant isolation | M     |                                                                                                        |
| Serving/publish path, freshness SLO, fallback | M     |                                                                                                        |
| ML-specific monitoring vs. plain service metrics | M     |                                                                                                        |
| Reproducibility | M     | All he has is S3 with JSON files with input/output information and the trained model file, no tracking |
| Cost instrumentation per tenant | M     |                                                                                                        |
| DevOps/SRE collaboration boundary | M     |                                                                                                        |

Overall bar level for this segment: [ ] Above bar   [ ] At bar   [X] Below bar

## Segment 5 — Candidate Questions (5–10 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

- How is to work at sigma

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Kubernetes / Docker (hands-on) | High | 1           | |
| CI/CD & model registry/lifecycle | High | 1           | |
| Orchestration, backfills & idempotency | High | 2           | |
| Monitoring & observability (ML-specific) | High | 1           | |
| Multi-tenant isolation reasoning | High | 1           | |
| System design synthesis | High | 1           | |
| Python / platform engineering | Medium | 2           | |
| Cloud / IaC / Linux | Medium | 1           | |
| Cost instrumentation reasoning | Medium | 1           | |
| Ownership / ambiguity tolerance | Medium | 2           | |
| Self-organization (multitasking, attention to detail) | Medium | 1           | |
| Communication clarity | Medium | 3           | |

## Decision Lean

[ ] Strong hire   [ ] Borderline   [X] No hire

Justification:

- The candidate works as ML engineer, but it lacks understanding of basic MLOps tools. He could not define MLOps, never used MLFlow and never mentioned a Model REgistry, just saves stuff to s3 and that is it. He apparently never actually worked with APIs. If he did, was a very simple API with no latency or throughput requirements. I think mostly he did batch processing, small experience with APIs. He never worked with Kubernetes, knows little about Docker, knows nothing about CI/CD, never mentioned unit testing, linting or anything similar.
- He knows ML, but lacks understanding of MLOps stuff that is critical for that role. 

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

- His english level was ok, he was calm and would speak very slowly. He could communicate fine anyways, I think the problem whas content, not form. 
