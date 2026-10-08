# Interview Notes — Victor Buava Leite

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. All bank questions are pre-listed with their expected answer, so you only need to check the box for whichever ones you actually ask and fill in the candidate's answer under them — leave the rest untouched. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Candidate Resume Summary

MLOps/ML Engineer, ~5 years, São Paulo, Brazil. Current: **MLOps Engineer at GCB Investimentos** (Oct 2023–Oct 2025, a Brazilian financial-markets holding company) — led transitioning ML engineering into a full ML platform: built a Feature Store, a Model Registry (production/shadow variants, A/B testing, auto-retraining), a custom ML Deployment Service (real-time/serverless/batch), a Ground Truth labeling service, a Model Performance Monitoring System, and a Data Drift Detection Service, all on AWS with IaC (Terraform, GitHub Actions). Concurrently: **ML Engineer at Adiante Recebíveis** (Mar 2023–Oct 2025, an SME invoice-financing fintech under the same corporate group) — built and led the ML Engineering function from prototype to production, CI/CD, end-to-end test suites, full lifecycle management. Also **ML Engineer at PeerBR** (Mar–Oct 2025, also under the same group) — first ML models for investment recommendations/lead generation. Earlier: Data Scientist roles (Adiante, Comunidade DS), and a pre-ML career as an Operational Analyst at Neoenergia (2014–2020). MBA in Machine Learning (IGTI, 2022–2023).

**Fit read for this role:** exceptionally strong on-paper match — his CV language mirrors this role's job description almost point for point (model registry with shadow variants/A-B testing, a deployment service across real-time/serverless/batch, drift detection, Terraform-based IaC, CI/CD). Worth verifying real hands-on depth rather than assuming it from the wording. Gaps to probe: no Kubernetes mentioned anywhere (deployment described via SageMaker + a custom service — confirm whether any of this involved K8s/Docker directly); AWS-only, no GCP; no explicit multi-tenant isolation or per-tenant cost instrumentation despite deep registry/governance experience. The overlapping employment dates across GCB/PeerBR/Adiante are self-explained in the CV (all under the same corporate group) rather than a red flag, but still fine to confirm directly. Full detail in `cv.md`.

*(Pre-filled from `cv.md` before the interview — full detail there if needed.)*

---

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

- Dariia introduction:
  - Position belongs to on client from Sigma, from America
  - Global advertising company, makes money from advertising
  - We need to build ML models for this existing product
  - Currently on a discovery phase with the client
  - Trying to understand what exactly we should do
  - Position involves MLops and some DAta Science as well, we don't have clear answers for that right now because we are yet in the discovery phase
  - It will be a dedicated sigma team, so you will be part of a team composed by Sigma engineers
- Candidate intro:
  - Always worked with data analysis
  - Started with ML 5 years ago
    - Begin as a Data Scientist, developed models for different industries, eventually began to put models to production (there was no one dedicated for that), as I progressed with that resposibility I began moving towards MLOps engineering. 
    - A lot of things were being duplicated, so we decided to unify all that and create a proper platform for that
    - We use AWS, a lot of services, and SageMaker is the basis
    - Also worked as Data Engineer for some time
  - Dariia:
    - I saw that you worked at the same time as ML Engineer and MLOps Engineer, how did you combine both positions at the same time?
      - I was the first ML Engineer in the company, so must I did was from scratch. Until a certain time I was the only one. When I began building the platform, I was the only one there. 
      - By the end of my time in the company I was totally dedicated to the platform as other engineers joined the team. My role was basically as a Tech Lead, helping them. So I would crate features, implement services, define architecture. So I had to divide my time between my main responsibilities and other "related" stuff.
      - I also mentored those other engineers
    - Your last position ended October 2025, an year ago. What have you been doing since?
      - I worked as a consultant, basically helping them to improve and implement the MLOps process. In the beggining of this year I started developing some personal products, because in my last role I had a lot of things to do, so I did not had time to learn AI and test and understand new technologies, so I took this time to learn and develop new skills. 
    - Tell me about your personal projects
      - I am developing a desktop app for Linux, Mac and Windows, it is like grammarly, correct what you are typing in line, but the focus is to learn english. The idea is to learn by doing. You try it, as you go you learn. We use the data from the customers to improve the results and the system.
    - Is this model learning from the experience?
      - Yes, it alligns the questions and learning path to the user itself.
      - I am currently learning english, so I am actively a user of my own system. I am devleoping it to my own user, but I hope to publish it eventually, but mostly is a learning project.
    - Why did you decide to look for a permanent job?
      - I decided to leave my last companies because I wanted to learn new stuff. As I really like roles related to plaform, MLOps and all that. So I prefer to work with companies but the side project is something I think it is cool to do, and in the future it could become a product. 
    - How do you plan to combine your full time job with personal projects?
      - During the week days I would work with Sigma, on my free time I will work on my own stuff.
      - I use those personal projects to learn new stuff, so that is the greates objetive of those side projects.
    - You did you decide to leave your previous company?
      - I developed stuff from scratch there, from Data ANalysis to MLOPs engineer. For the financial group decided that the project was not grown as stackholders expected, and eventually they decided to cap the improvements and ML, so my work began to be repetitive, and eventually they began to give me responsabilities that are not for an ML engineer, so I decided to leave to seek my desired path.
    - What are you looking for in your new position?
      - I am looking for roles about ML PLatforms, that is, help Data Scientist to build their models, no build the models myself. I want to work as a Platform Engineer for ML.
    - What activities you don't want to perform?
      - I just don't want to do things were I am not good enough. If there is something I need to do, I want to be were I am prepared to. 
      - I don't think I am very good delegator, so I prefer tech work. 
      - There isn't something specific I want to avoid, but I want to focus on build the ML Platform.
    - How do you feel with poor briefing in tickets? What would you do?
      - I would try to gather more information about it. Talk to who created the ticket and try to clarify the requirements as possible. Without that, I cannot propose a solution.
    - What if the person who crated the ticket is difficult to reach?
      - I would try to refine the requirements myself as far as I can. Sometimes that is possible, sometimes it is not. If the person is busy, I would need to try to make the best out it on my own until I can get proper answer. Usually, using templates to create tickets can help with that. 
    - What if you are given a task to create a feature that is very important, hwoever it brings a lot of risk. How would you handle?
      - I would point all the risks, try to understand them, see if I can minimize those risks or if there is any approach to overcome those risks. Bascillay I would need to understand the risks, present them to stackholders and discuss which direction to follow. 
    - Imagine you have 5 tasks. You need to decide which tasks to begin. How you do it?
      - I would understand which task has the highest priority and begin with that, the most important task. If that is not the case, I would begin with the most simple to solve and deliver value.
  
  - Victor questions:
    - Will you Dariia be the PM of the project?
      - No, someone else from the client will have the PM. So tech part is Sigma, Management is from the client. 

  - Rodrigo's questions:
    - Describe a cool experience you have
      - We had this model registry with different models and metadata, and to put this model in production we would just change the status of the model and that would trigger a pipeline would deploy the model to PRD. However, we weren't able to perform A/B tests or online experiemnts. It was very hard to test that in PRD. So we needed to create this A/B test environment. 
      - We decided to define a new status that would trigger the A/B testing, shadow deploument and so on.
      - We need to create reconciliation stage that would compare the proposed state (the new state) with what we have in production, see the difference, and then decide.
      - The process was automatic. 

  
## Segment 2 — Role Context (interviewer-led, 5 min)

Candidate reactions / questions during this segment, if any:

-

## Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 across categories based on time and where the candidate is weak/strong. Only fill in the ones you actually ask.

### Python & Platform Engineering

- [ ] **Q:** The job description says "you write the platform, not only its YAML." What's the practical difference between a platform engineer who writes YAML and one who writes the platform?
  > Expected: YAML wires together existing primitives; writing the platform means building the real logic underneath (retry/backfill logic, registry clients, promotion-gate code, custom operators) — actual software engineering, not just config.
  Candidate's answer: who builds the platform is the person who defines the architecture, not only the YAML files. We need to understand size of the compnay, how it wants to grow, what are the requirements, etc. So we need to understand company needs and design a solution. Understand trade-offs and so on and define tooling, define the workflow and eventually we would build the platform, that also includes the YAML files (just a detail in this case)

- [ ] **Q:** You're handed a data scientist's training script to put into production. What are the first things you check to figure out what will break?
  > Expected: Hardcoded paths/credentials, non-deterministic behavior, notebook-style global state, no error handling on external calls, assumptions that won't hold at scale, compute/memory mismatches, no separation between training-time and serving-time code.
  Candidate's answer: he suggested that using templates to create those scripts would help. So I prompted to answer it in the case you don't have those templates. So I would check if it runs, check if there is any type of leakage, check if features exist in PRD, check requirements and dependencies, if code is not putting a lot of stuff in memory, linters. Most of the time is about optmization of the code, like using memory and so on, avoid wasting resources. 

- [ ] **Q:** What do you understand by reproducibility, how can you enforce it, and why does it matter enough to be an explicit requirement here?
  > Expected: Being able to exactly reconstruct what was trained/served, not approximate it. Enforce via pinned dependencies/environments, immutable container images tied to a code commit, versioned/snapshotted training data, and recording which code+environment+data combination produced each served model. Matters because debugging a production incident or answering a compliance question requires reconstructing exactly what was served, not guessing.
  Candidate's answer: reproducibility is about building things that we can trust it would regenerated the same thing again and again and output the same results if same inputs were used. So for a given model, I need to be able to retraing it exactly, so using the same code, the same dataset, and so on. We need that to improve the model. If something works (or it doesn't), we need to be able to track why that happens. Whem we have reproducitbility, we can improve. To enforce it, we can use tests, end-to-end tests, verify all components. Check if the output is correct. We have docker to define the environment, build the image and use it, have metadata saved into github to track it with a commit hash, we need to check how a model was generated, that is, using a specific model, specific data, specific metadata and so on. What pipeline run generated the model and so on. We need a lineage. 

### Kubernetes & Docker

Disclaimer: Does not know a lot about k8s but is actively learning it right now. His platform is mostly built on managed services. Have used kubectl, but as a learning process. He tried to deploy a k8s cluster to learn Ray, distributed computing and all that. Tried to deploy a KubeRay. I trying to close that gap: I am a platform engineer that usually uses managed services, so I want to learn K8s and all that. 

- [ ] **Q:** A pod won't start — CrashLoopBackOff. Walk me through how you'd debug it.
  > Expected: `kubectl describe pod` for events, `kubectl logs --previous`, check resource limits vs. actual need, verify entrypoint/command, check mounted config/secrets.
  Candidate's answer: 

- [ ] **Q:** What's the difference between a Kubernetes Job and a CronJob? Follow-up 1: Say a CronJob runs a retraining pipeline every hour, but one run takes longer than expected and is still going when the next hour's run is due to start — what could go wrong, and how would you configure the CronJob to prevent it? Follow-up 2: a Job and a Deployment are both Kubernetes objects that manage pods for you — what's the actual difference between them?
  > Expected: Job runs once; CronJob schedules Jobs recurringly. On the overlap: default `concurrencyPolicy` is `Allow`, so two copies of the pipeline run at once and can race/double-process; prevent with `Forbid` (skip) or `Replace` (kill old, start new) plus a sensible `startingDeadlineSeconds` — though this doesn't replace making the job idempotent, since a retry can still cause it to effectively run twice. Follow-up: a Deployment keeps a desired number of pods running indefinitely (replaces dead ones forever); a Job runs pods until the task completes successfully and then stops.
  Candidate's answer:

- [ ] **Q:** In Kubernetes, what are resource requests and limits — what does each one actually control? Follow-up: what specifically goes wrong for a training job, and for other things sharing its node, if requests are set too low, if the limit is set too low, or if no limit is set at all?
  > Expected: Requests = scheduler's placement reservation, not a runtime cap. Limits = enforced runtime ceiling (memory over limit → OOMKilled; CPU over limit → throttled). Misconfigurations: requests too low → node oversubscription, instability for everyone on that node; limit too low → the job itself gets OOMKilled; no limit at all → the job can consume all free node resources and starve other pods on the same node.
  Candidate's answer:

### CI/CD for ML & Model Registry

- [ ] **Q:** Assume that in your company a data scientist trains a model in a notebook, and once it looks good, someone manually exports the model file and uploads it to a production location. What would you need to build so that step is fully automated instead, and why does that matter?
  > Expected: Fully automated path from versioned code — training, automated evaluation against a gate, automated promotion, no human touching a file in between. Matters because that manual step is exactly where reproducibility and audit trail break down.
  Candidate's answer: fist I would get the correct version of the code, structure the code, put in the CI/CD to generate a docker file, which would have the training script, evaluation and all that, put those docker files into a repository, after that we need to define the pipeline in the CI/CD process, we could choose if we will run this in GitHub actions or maybe a pipeline tool like Airflow. So the pipeline should registry the model into a model registry, so we have metadata, understand what happent, track it. Then we can deploy it to sage maker. We could also run tests, end-to-end tests and all that to also guarantee guardrails. 

- [ ] **Q:** What's the difference between versioning a model and having a full model lifecycle?
  > Expected: Versioning tracks artifacts; a lifecycle adds lineage, documented evaluation tied to that version, and an auditable state machine with real gates between states.
  Candidate's answer: 

- [ ] **Q:** How would you design an evaluation gate that has to be auditable — someone outside the team needs to check why a model got promoted?
  > Expected: Record the metric(s) evaluated, candidate vs. production values, dataset/version used, and a timestamp — stored durably and queryably, not a Slack message.
  Candidate's answer:

### Orchestration, Backfills & Idempotency

It used a little bit of airflow, but in my previous work we decided to migrate to SageMaker pipelines. 

- [ ] **Q:** What does it mean for a backfill to be idempotent, how would you actually make one idempotent, and why does that matter specifically for a system running on hourly/daily/weekly planes?
  > Expected: Running the same backfill twice (retry, re-trigger, overlap) produces the identical end state, not duplicated/corrupted data. This isn't a Kubernetes setting — the job's own logic has to guarantee it, and the mechanism depends on what the job does: upsert keyed by a stable identifier (e.g. the hour) for writes, partitioning output by the logical unit processed (not by run time) for files, an idempotency key passed to any external side-effecting call. Matters here because with three overlapping schedules, retries are routine — without idempotency, a retried hourly job can silently double-count into a daily aggregate before anyone notices.
  Candidate's answer: if I need to run something again with the same inputs, the new result will be the same result as before and it will not generate one more result. Problems that no indempotency coudl cause: that could mess your models, because it could corrupt your data, that could hurt reproducibility. 

- [ ] **Q:** A daily job depends on all 24 hourly jobs from the previous day succeeding. Two failed. What are your options, and what would you actually do?
  > Expected: Block until backfilled (safest, delays everything); run on partial data flagged as degraded; auto-retry with backoff and a deadline. Good answer names the trade-off explicitly.
  Candidate's answer:

### Monitoring & Observability

What he knows about it: Infra is monitored, if machine is running, model monitoring, data monitoring, keep metrics that could explain the behavior of the model in the future.
  - He knows somethign about monitoring, not new for him. Know shap valeus and all that.

- [ ] **Q:** What's the difference between feature drift and prediction drift, and why do you need to watch both?
  > Expected: Feature drift = input distribution shifted; prediction drift = output distribution shifted. They can move independently — watching only one can miss the other's failure mode.
  Candidate's answer: feature drift is when we expect a specific dsitrbution for a given feature and in prd we have a different one. The shape of the input is changing a bit. It can impact the model. prediction drift is about wehn you receive the same input but the output changes. In our view there is no reason for that to happen, but something outside of the model could change.

- [ ] **Q:** What's train/serve skew, and give a concrete way it could happen even if the model code itself hasn't changed?
  > Expected: Serving-time features don't match training-time features. E.g. a feature computed via a different code path in serving than in training (batch/offline vs. real-time).
  Candidate's answer:

- [ ] **Q:** A model's predicted probabilities are supposed to reflect real-world likelihood — something it scores 0.9 should turn out to actually be correct about 90% of the time. Over time in production, that relationship can quietly break down: the scores still look numerically similar, but they stop matching real outcomes as well as they used to, even while overall accuracy looks fine. How would you detect that happening without waiting weeks for new labeled outcomes to arrive?
  > Expected: (sometimes called "calibration decay"/"calibration drift" — candidate doesn't need the term). Without fresh labels, watch the prediction-score distribution as a proxy; reconstruct calibration once labels eventually arrive.
  Candidate's answer:

### Cloud, IaC & Linux

- [ ] **Q:** Why does the job description ask for Terraform specifically rather than just someone comfortable in the GCP console?
  > Expected: Console changes are manual/undocumented/unreviewable. IaC makes changes reviewable, reproducible, auditable — directly relevant to this job description's isolation-evidence and reproducibility asks.
  Candidate's answer: we terraform or similar tool for IaC because we need to know what exactly is in production. If we need to audit, to guarantee stuff, security to avoid unnexpected changes and all that. 

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
  Candidate's answer:

- [ ] **Q:** Describe a time a drift or monitoring alert fired in production. What did you actually do?
  > Criteria: A real detection mechanism, a concrete diagnosis process, a fix actually validated before shipping.
  Candidate's answer:

### Self-Organization (behavioral — no single correct answer; evaluate against criteria)

- [ ] **Q:** In this role, you'd often be working on several different things at once — running training pipelines, keeping track of model versions, monitoring the system, tracking costs, and writing documentation for how everything works. Tell me about a time you had several different tasks competing for your attention at once. How did you decide what to work on first, and how did you make sure the other tasks didn't get forgotten?
  > Criteria: A real prioritization framework, evidence deprioritized items weren't dropped, some system for tracking commitments.
  Candidate's answer:

- [ ] **Q:** Tell me about a time you caught a subtle bug or configuration issue in a production ML system that wasn't obvious and nobody else had noticed.
  > Criteria: A specific example, a real explanation of how they found it (not luck), genuine attention to detail.
  Candidate's answer:

- [ ] **Q:** How do you keep up with MLOps tooling and practices, given how fast this space moves?
  > Criteria: A real, specific habit or practice, not a vague "I read blogs."
  Candidate's answer:

## Segment 4 — Task (20–25 min)

Option used: [ ] A — Config Reading & Debugging   [ ] B — Model Registry & Promotion Design   [ ] C — System Design

Notes on approach:

-

Facilitation tracking (Option C only) — mark **U** (unprompted), **P** (prompted), or **M** (missing) per topic:

| Topic | U/P/M | Notes |
|---|---|---|
| Training orchestration & scheduling | | |
| Idempotent backfills | | |
| Model registry & versioning | | |
| Evaluation gate before promotion | | |
| Safe rollout (shadow / champion-challenger) | | |
| Per-tenant isolation | | |
| Serving/publish path, freshness SLO, fallback | | |
| ML-specific monitoring vs. plain service metrics | | |
| Reproducibility | | |
| Cost instrumentation per tenant | | |
| DevOps/SRE collaboration boundary | | |

Overall bar level for this segment: [ ] Above bar   [ ] At bar   [ ] Below bar

## Segment 5 — Candidate Questions (5–10 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

-

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Kubernetes / Docker (hands-on) | High | 2           | |
| CI/CD & model registry/lifecycle | High | 3           | |
| Orchestration, backfills & idempotency | High | 3           | |
| Monitoring & observability (ML-specific) | High | 3           | |
| Multi-tenant isolation reasoning | High | 2           | |
| System design synthesis | High |             | |
| Python / platform engineering | Medium | 3           | |
| Cloud / IaC / Linux | Medium | 3           | |
| Cost instrumentation reasoning | Medium |             | |
| Ownership / ambiguity tolerance | Medium | 3           | |
| Self-organization (multitasking, attention to detail) | Medium | 3           | |
| Communication clarity | Medium | 3           | |

## Decision Lean

[ ] Strong hire   [X] Borderline   [] No hire

Justification:

-

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

- There was not enough time to give him the system design task. However, I had time to ask him several questions that pointed to the direction that he is indeed fit to the role.
- I did not had time to ask him behavorial qustions, but in the beggining he told about hsi experiences and there is information there about it, so analyze the text we have about him and draw the conclusions.
- He seems to be almost senior, but not senior yet. However he is a good candidate, I think he is a Hire.
