# Interview Notes — {{CANDIDATE_NAME}}

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. All bank questions are pre-listed with their expected answer, so you only need to check the box for whichever ones you actually ask and fill in the candidate's answer under them — leave the rest untouched. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Candidate Resume Summary

{{RESUME_SUMMARY}}

*(Pre-filled from `cv.md` before the interview — full detail there if needed.)*

---

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

-

## Segment 2 — Role Context (interviewer-led, 5 min)

Candidate reactions / questions during this segment, if any:

-

## Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 across categories based on time and where the candidate is weak/strong. Only fill in the ones you actually ask.

### Python & Platform Engineering

- [ ] **Q:** The job description says "you write the platform, not only its YAML." What's the practical difference between a platform engineer who writes YAML and one who writes the platform?
  > Expected: YAML wires together existing primitives; writing the platform means building the real logic underneath (retry/backfill logic, registry clients, promotion-gate code, custom operators) — actual software engineering, not just config.
  Candidate's answer:

- [ ] **Q:** You're handed a data scientist's training script to put into production. What are the first things you check to figure out what will break?
  > Expected: Hardcoded paths/credentials, non-deterministic behavior, notebook-style global state, no error handling on external calls, assumptions that won't hold at scale, compute/memory mismatches, no separation between training-time and serving-time code.
  Candidate's answer:

- [ ] **Q:** What do you understand by reproducibility, how can you enforce it, and why does it matter enough to be an explicit requirement here?
  > Expected: Being able to exactly reconstruct what was trained/served, not approximate it. Enforce via pinned dependencies/environments, immutable container images tied to a code commit, versioned/snapshotted training data, and recording which code+environment+data combination produced each served model. Matters because debugging a production incident or answering a compliance question requires reconstructing exactly what was served, not guessing.
  Candidate's answer:

### Kubernetes & Docker

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
  Candidate's answer:

- [ ] **Q:** What's the difference between versioning a model and having a full model lifecycle?
  > Expected: Versioning tracks artifacts; a lifecycle adds lineage, documented evaluation tied to that version, and an auditable state machine with real gates between states.
  Candidate's answer:

- [ ] **Q:** How would you design an evaluation gate that has to be auditable — someone outside the team needs to check why a model got promoted?
  > Expected: Record the metric(s) evaluated, candidate vs. production values, dataset/version used, and a timestamp — stored durably and queryably, not a Slack message.
  Candidate's answer:

### Orchestration, Backfills & Idempotency

- [ ] **Q:** What does it mean for a backfill to be idempotent, how would you actually make one idempotent, and why does that matter specifically for a system running on hourly/daily/weekly planes?
  > Expected: Running the same backfill twice (retry, re-trigger, overlap) produces the identical end state, not duplicated/corrupted data. This isn't a Kubernetes setting — the job's own logic has to guarantee it, and the mechanism depends on what the job does: upsert keyed by a stable identifier (e.g. the hour) for writes, partitioning output by the logical unit processed (not by run time) for files, an idempotency key passed to any external side-effecting call. Matters here because with three overlapping schedules, retries are routine — without idempotency, a retried hourly job can silently double-count into a daily aggregate before anyone notices.
  Candidate's answer:

- [ ] **Q:** A daily job depends on all 24 hourly jobs from the previous day succeeding. Two failed. What are your options, and what would you actually do?
  > Expected: Block until backfilled (safest, delays everything); run on partial data flagged as degraded; auto-retry with backoff and a deadline. Good answer names the trade-off explicitly.
  Candidate's answer:

### Monitoring & Observability

- [ ] **Q:** What's the difference between feature drift and prediction drift, and why do you need to watch both?
  > Expected: Feature drift = input distribution shifted; prediction drift = output distribution shifted. They can move independently — watching only one can miss the other's failure mode.
  Candidate's answer:

- [ ] **Q:** What's train/serve skew, and give a concrete way it could happen even if the model code itself hasn't changed?
  > Expected: Serving-time features don't match training-time features. E.g. a feature computed via a different code path in serving than in training (batch/offline vs. real-time).
  Candidate's answer:

- [ ] **Q:** A model's predicted probabilities are supposed to reflect real-world likelihood — something it scores 0.9 should turn out to actually be correct about 90% of the time. Over time in production, that relationship can quietly break down: the scores still look numerically similar, but they stop matching real outcomes as well as they used to, even while overall accuracy looks fine. How would you detect that happening without waiting weeks for new labeled outcomes to arrive?
  > Expected: (sometimes called "calibration decay"/"calibration drift" — candidate doesn't need the term). Without fresh labels, watch the prediction-score distribution as a proxy; reconstruct calibration once labels eventually arrive.
  Candidate's answer:

### Cloud, IaC & Linux

- [ ] **Q:** Why does the job description ask for Terraform specifically rather than just someone comfortable in the GCP console?
  > Expected: Console changes are manual/undocumented/unreviewable. IaC makes changes reviewable, reproducible, auditable — directly relevant to this job description's isolation-evidence and reproducibility asks.
  Candidate's answer:

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
|---|---|---|---|
| Kubernetes / Docker (hands-on) | High | | |
| CI/CD & model registry/lifecycle | High | | |
| Orchestration, backfills & idempotency | High | | |
| Monitoring & observability (ML-specific) | High | | |
| Multi-tenant isolation reasoning | High | | |
| System design synthesis | High | | |
| Python / platform engineering | Medium | | |
| Cloud / IaC / Linux | Medium | | |
| Cost instrumentation reasoning | Medium | | |
| Ownership / ambiguity tolerance | Medium | | |
| Self-organization (multitasking, attention to detail) | Medium | | |
| Communication clarity | Medium | | |

## Decision Lean

[ ] Strong hire   [ ] Borderline   [ ] No hire

Justification:

-

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-
