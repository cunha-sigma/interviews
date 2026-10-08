# Interview Plan — Senior MLOps / ML Platform Engineer (REF4171X)

**Role:** Senior MLOps / ML Platform Engineer, per `job_description.md`. Client operates supply-side infrastructure in programmatic advertising — a large ad exchange handling hundreds of millions of auction requests a day — and is building an in-house predictive decisioning platform. This hire builds and owns the **platform**, not the models themselves: training orchestration, model registry/promotion, per-advertiser isolation, monitoring, reproducibility, and cost instrumentation, then hands it to the client's own engineers.

**A note on how this plan was built:** first version, constructed directly from `job_description.md`'s requirements and the client's own `Interview Criteria and Questions.pdf` (transcribed as `candidates/review_template.md`). No client-shared example exercise exists for this role — the task menu in §6 is built from scratch. Unlike some other roles in this project, there's no separate hidden reference architecture to protect here: the job description's own responsibilities list (§ below) is detailed enough to double as this plan's system-design answer key directly, and candidates have likely already read the job description before interviewing — so the system-design task in Option C doesn't need to withhold a mechanism the way other roles' plans do. Revisit once real interviews produce feedback.

---

## 1. What this interview is screening for

| Area | Why it matters here | job description signal |
|---|---|---|
| Python & platform engineering | "You write the platform, not only its YAML" — must build real logic, not just wire config | Must-have |
| Kubernetes & Docker | Resource limits, jobs/cronjobs, debugging a pod that won't start — hands-on, not conceptual | Must-have |
| CI/CD for ML & model registry/lifecycle | No manual notebook-to-production step; versioning, lineage, auditable promotion gates | Must-have |
| Orchestration, backfills & idempotency | Training orchestration across hourly/daily/weekly planes, with retries and idempotent reruns | Must-have, named directly in responsibilities |
| Monitoring & observability of ML systems | Drift, skew, calibration decay, label arrival delay — plus ordinary service metrics | Must-have |
| Cloud, IaC (infrastructure as code) & Linux | GCP preferred, infrastructure-as-code, solid Linux debugging | Must-have |
| Multi-tenant isolation & reproducibility | Per-advertiser isolation with compliance-ready evidence; rebuild any served model from recorded inputs | Named directly in responsibilities; "strongly preferred" |
| Cost instrumentation | Per-tenant training/scoring cost — client will ask before committing to phase two | Named directly in responsibilities |
| Ownership & ambiguity tolerance | End-to-end platform ownership, handoff to client engineers, working alongside (not duplicating) DevOps/SRE | Responsibilities + Personal Profile pattern seen across Sigma roles |
| Self-organization | This role means juggling orchestration, registry, monitoring, cost, and documentation simultaneously — genuinely multi-threaded work | Interview Criteria doc scores this explicitly; not naturally covered by technical Q&A alone |

**Deliberate design choice on the exercise (§6):** live coding wasn't used, for the same reason as other roles in this project — with LLM coding assistants ubiquitous, typing code live tests typing speed more than engineering judgment. The task menu instead tests reading/debugging a real config, designing a registry/promotion process on paper, or designing the platform end-to-end out loud.

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

Let them drive. Listen for: did they actually own infrastructure end-to-end and keep it running, or did they build something once and hand it off? The job description is explicit about this distinction ("production ML you kept running — not deployed once") — this segment is the first place to sense which one you're talking to, ahead of the behavioral questions in §5.9.

---

## 4. Segment 2 — Role Context (what to tell the candidate)

Read/paraphrase this — built directly from `job_description.md`'s Company/Customer/Project sections. Unlike some other roles in this project, there's no hidden mechanism to protect here, so feel free to elaborate freely if the candidate asks follow-ups:

> "Our client runs a large online advertising marketplace — a platform where ad space gets bought and sold automatically, handling hundreds of millions of these transactions every day. They're building a new system that uses machine learning to make decisions in real time: which ads are worth showing, and how to help an advertiser find new potential customers who resemble the customers they already have — all while balancing several business goals at once. You wouldn't be building those machine learning models yourself — you'd be building and owning the platform underneath them: how models get trained on a schedule and safely put into production, how many different customers' models run separately from each other while sharing the same infrastructure, how the system detects when a model's quality has quietly gotten worse, what it costs to run per customer, and eventually handing all of that over to the client's own engineers to run going forward."

---

## 5. Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 questions total across categories based on time and where the candidate is weak/strong — this is a bank, not a script.

### 5.1 Python & Platform Engineering
- **Q:** The job description says "you write the platform, not only its YAML." What's the practical difference between a platform engineer who writes YAML and one who writes the platform?
  **A:** YAML wires together existing primitives (K8s manifests, Helm values, CI configs) — necessary, but declarative/configuration-level. Writing the platform means building the actual code that makes those primitives work together with real logic: retry/backfill logic, registry client libraries, promotion-gate evaluation code, custom operators. Someone who only writes YAML can't build custom orchestration logic or debug non-declarative failures.
- **Q:** You're handed a data scientist's training script to put into production. What are the first things you check to figure out what will break?
  **A:** Hardcoded paths/credentials, non-deterministic behavior (unpinned seeds/dependency versions), notebook-style global state or non-idempotent side effects, no error handling on external calls, assumptions about pre-cleaned data that won't hold at scale, compute/memory assumptions that don't match production hardware, and no clean separation between training-time and serving-time code (a real train/serve-skew risk).
- **Q:** What do you understand by reproducibility, how can you enforce it, and why does it matter enough to be an explicit requirement here?
  **A:** *What it means:* being able to exactly reconstruct what was actually trained and served at a given point in time — not an approximation of it. *How to enforce it:* pin dependency versions and environments (not "latest"), build immutable container images tied to a specific code commit, snapshot the training data (or version it) rather than pointing at a live, mutable table, and record which exact combination of code, environment, and data version produced each served model. *Why it matters:* when something goes wrong in production — a bad prediction, a compliance question, a regression — you need to reconstruct exactly what was served to debug it or answer "what model made this decision and why," not guess at it after the fact.

### 5.2 Kubernetes & Docker
- **Q:** A pod won't start — CrashLoopBackOff. Walk me through how you'd debug it.
  **A:** `kubectl describe pod` for events (image pull errors, resource limits exceeded, failed probes), `kubectl logs --previous` for the crashed container's last output, check resource requests/limits against actual need (OOMKilled shows in describe), verify the entrypoint/command, and check mounted config/secrets are actually present and correctly formatted.
- **Q:** What's the difference between a Kubernetes Job and a CronJob? Follow-up 1: Say a CronJob runs a retraining pipeline every hour, but one run takes longer than expected and is still going when the next hour's run is due to start — what could go wrong, and how would you configure the CronJob to prevent it? Follow-up 2: a Job and a Deployment are both Kubernetes objects that manage pods for you — what's the actual difference between them?
  **A:** A Job runs a pod to completion once; a CronJob schedules Jobs on a recurring schedule. *On the overlap scenario:* by default, Kubernetes' `concurrencyPolicy` is `Allow`, so the new hourly run starts anyway even while the previous one is still running — now two copies of the retraining pipeline are running at once, which can race on shared state or double-process the same data. Prevent it by setting `concurrencyPolicy` explicitly to `Forbid` (skip the new run entirely if the previous one hasn't finished) or `Replace` (kill the old one and start the new one) — whichever fits how the pipeline should behave in that situation — plus a sensible `startingDeadlineSeconds` so a missed run doesn't just queue up and fire late. This doesn't fully replace making the job idempotent, though: a node failure or a retry can still cause the pipeline's logic to effectively run twice even with the right `concurrencyPolicy` set. *Follow-up:* a Deployment is built to keep a desired number of pods running indefinitely — if a pod dies, it's replaced, forever, because the workload (e.g. an API server) should never just "finish." A Job is built for a workload with a defined finish line — it runs pods until the task completes successfully and then stops; it only restarts a pod on failure, not to keep something alive perpetually.
- **Q:** In Kubernetes, what are resource requests and limits — what does each one actually control? Follow-up: what specifically goes wrong for a training job, and for other things sharing its node, if requests are set too low, if the limit is set too low, or if no limit is set at all?
  **A:** *Definitions:* Requests are what the scheduler uses to decide which node has room for the pod — a reservation used at placement time, not a runtime cap; once the pod is actually running, nothing stops it from using more than it requested (up to its limit, if one exists). Limits are the actual enforced ceiling at runtime: for memory, exceeding it gets the container killed outright (OOMKilled — memory can't be partially throttled); for CPU, exceeding it gets the container throttled (slowed down), not killed.
  *What goes wrong with each misconfiguration:*
  - Requests too low → the scheduler can pack more pods onto a node than it can actually support once everything is running at real usage — oversubscription — which can cause instability for every pod on that node, not just the one that under-declared.
  - Limit too low → the training job itself gets OOMKilled once it exceeds it, regardless of how accurate its request was.
  - No limit set at all → the container has no ceiling — it can consume memory/CPU up to whatever the node actually has free at that moment, which can starve every other pod on that same node, since nothing is stopping it from taking all of it.

### 5.3 CI/CD for ML & Model Registry
- **Q:** Assume that in your company a data scientist trains a model in a notebook, and once it looks good, someone manually exports the model file and uploads it to a production location. What would you need to build so that step is fully automated instead, and why does that matter?
  **A:** The path from a validated experiment to a production model must be fully automated and reproducible from code — training triggered from versioned code, automated evaluation against a defined gate, automated promotion into the registry/serving path, with no human touching a file in between. It matters because that manual export-and-upload step is exactly where reproducibility and audit trail break down: nobody can later reconstruct precisely which code, data, and evaluation results produced what's actually running in production.
- **Q:** What's the difference between versioning a model and having a full model lifecycle?
  **A:** Versioning tracks distinct artifacts. A lifecycle adds lineage (what data/code/config produced this version), documented evaluation results tied to that version, and a defined, auditable state machine (staging → evaluated → promoted → production → archived) with real gates between states.
- **Q:** How would you design an evaluation gate that has to be auditable — someone outside the team needs to check why a model got promoted?
  **A:** Record, alongside the promotion decision: which metric(s) were evaluated, the actual values for candidate vs. current production, the dataset/version used, and a timestamp — stored durably and queryably, not a Slack message or someone's memory.

### 5.4 Orchestration, Backfills & Idempotency
- **Q:** What does it mean for a backfill to be idempotent, how would you actually make one idempotent, and why does that matter specifically for a system running on hourly/daily/weekly planes?
  **A:** *What it means:* running the same backfill twice — because of a retry, an accidental re-trigger, or an overlapping run — leaves the system in exactly the same end state as running it once. Not "runs safely twice," but "produces the identical result both times."
  *How to actually achieve it:* this isn't a Kubernetes setting — nothing in the scheduler makes a job idempotent for you, and Kubernetes' own retry/concurrency controls only reduce the odds of a duplicate run happening, they don't prevent it. The job's own logic has to be written so a repeat run can't corrupt state, and the right mechanism depends on what the job actually does: if it writes/inserts data, use an upsert keyed by something stable (e.g. "the hour being processed") instead of a blind insert, so a repeat run overwrites the same row instead of adding a duplicate; if it writes output files, partition by the logical unit being processed (the target hour) rather than by when the job happened to run, so a retry overwrites the same partition; if it calls another system with a side effect, pass that system an idempotency key so it can recognize and drop a duplicate request.
  *Why it matters here specifically:* with three overlapping schedules (hourly/daily/weekly) all depending on each other, failures and retries are routine, not rare edge cases — without idempotency, a retried hourly job could silently double-count into that day's aggregate, and nobody would necessarily notice until the numbers looked wrong downstream.
- **Q:** A daily job depends on all 24 hourly jobs from the previous day succeeding. Two failed. What are your options, and what would you actually do?
  **A:** Block the daily job until the missing hours backfill (safest, delays everything); run on partial data flagged as degraded (keeps moving, risks a quietly-wrong result); auto-retry the failed hours with backoff and only proceed once resolved or a deadline passes. A good answer names the trade-off explicitly rather than picking one path without justifying it.

### 5.5 Monitoring & Observability
- **Q:** What's the difference between feature drift and prediction drift, and why do you need to watch both?
  **A:** Feature drift = input distribution has shifted since training. Prediction drift = the model's output distribution has shifted. They can move independently — features can drift with predictions staying stable, or predictions can drift while features look normal (a serving-path bug, not a data problem) — watching only one can miss the other's failure mode.
- **Q:** What's train/serve skew, and give a concrete way it could happen even if the model code itself hasn't changed?
  **A:** The features computed at serving time don't match what the model was trained on, even with an unchanged model. Concrete cause: a feature computed with a different code path in the serving pipeline than in training (e.g. batch/offline computation in training, a separately-maintained real-time version in serving).
- **Q:** A model's predicted probabilities are supposed to reflect real-world likelihood — something it scores 0.9 should turn out to actually be correct about 90% of the time. Over time in production, that relationship can quietly break down: the scores still look numerically similar, but they stop matching real outcomes as well as they used to, even while overall accuracy looks fine. How would you detect that happening without waiting weeks for new labeled outcomes to arrive?
  **A:** *(Interviewer note: this is sometimes called "calibration decay" or "calibration drift" — the candidate doesn't need to know or produce that term, just reason about the scenario as described.)* Without fresh labels, watch the prediction-score distribution itself for shifts as a proxy signal, and reconstruct actual calibration retrospectively once labels do eventually arrive, however delayed — which is also why label-arrival delay itself is something worth monitoring as its own thing, separate from this.

### 5.6 Cloud, IaC & Linux
- **Q:** Why does the job description ask for Terraform specifically rather than just someone comfortable in the GCP console?
  **A:** Console changes are manual, undocumented, and not reviewable — no diff, no audit trail, no reliable reproduction or rollback. IaC (infrastructure as code — defining infrastructure in version-controlled config files instead of clicking through a console) makes infrastructure changes reviewable (pull-request diffs), reproducible, and auditable (git history is the change log) — directly relevant to this job description's isolation-evidence and reproducibility requirements.
- **Q:** A training job on Linux is silently getting OOM-killed with no clear error in the application logs. How do you find that?
  **A:** Check `dmesg`/`/var/log/syslog` for OOM-killer entries (the kernel logs this; the app often never gets a chance to), check `kubectl describe pod` if containerized (shows `OOMKilled` as exit reason), and correlate the kill timestamp with actual memory usage over time rather than only looking at application-level logs.

### 5.7 Multi-Tenant Isolation & Cost
- **Q:** Say your platform trains and serves a separate model for each of your customers, so that every customer's model is only ever supposed to use that customer's own data — no customer should ever be able to see or affect another customer's model or data. A compliance auditor asks you to prove that this separation actually holds. Saying "we use separate namespaces" isn't proof on its own — what concrete evidence would you actually show them?
  **A:** Concrete artifacts, not just an architecture description: infrastructure-as-code/config showing per-tenant namespace/resource boundaries, identity and access management policies (permission rules restricting exactly who/what can touch which resources) scoped per tenant — proving tenant A's pipeline can't reach tenant B's data/model — logs/audit trails showing no cross-tenant access ever occurred, and documentation mapping which resources belong to which tenant. "We use separate namespaces" is a claim about the design; evidence is what proves that design actually held in practice.
- **Q:** Say your platform runs training and hourly scoring for many different customers, all on the same shared cluster — there's no dedicated hardware per customer. How would you figure out what it actually costs to run this for one specific customer? Follow-up: if you didn't have any cost-allocation tooling available, how would you get that same answer yourself?
  **A:** *Main approach — two things need to be in place together, not just one:*
  1. Every job/pod is labeled with a customer identifier, consistently, from the start — this has to be an ongoing operational practice, not a one-time setup, since any unlabeled or mislabeled job is cost with nowhere correct to go.
  2. A mechanism that turns those labels plus actual resource usage into a real cost number — either a cloud-native feature (GKE's Kubernetes cost allocation, or AWS's EKS split cost allocation data) or a dedicated tool (Kubecost/OpenCost). Labels alone don't compute anything — the cloud provider bills the shared node/cluster as a whole, so a label by itself doesn't split that bill. What these tools/features actually do is combine the label with real measured usage (CPU/GPU-seconds) and real node pricing to derive an apportioned cost per customer.
  *Follow-up — without any tooling:* track actual compute consumption per job yourself at the orchestration layer (CPU/GPU-seconds per run, recorded against the customer that job belongs to), then apportion the cluster's real total bill across customers in proportion to their share of total compute-time consumed. This is the same underlying logic the tools automate — a candidate who can describe this manual version is showing they understand the actual mechanism, not just the name of a tool that does it for them.

### 5.8 Ownership / Ambiguity (behavioral — no single correct answer; evaluate against criteria)
- **Q:** Tell me about a time you owned a piece of ML infrastructure end-to-end, including a decision nobody explicitly asked you to make.
  **A (criteria):** A concrete unprompted decision, the reasoning behind it, ownership of the outcome/consequence.
- **Q:** Describe a time a drift or monitoring alert fired in production. What did you actually do?
  **A (criteria):** A real detection mechanism, a concrete diagnosis process, a fix actually validated before shipping — directly maps to the job description's "drift detection in practice — including what you did when it fired."

### 5.9 Self-Organization (behavioral — added deliberately to generate signal the technical Q&A won't; no single correct answer)
- **Q:** In this role, you'd often be working on several different things at once — running training pipelines, keeping track of model versions, monitoring the system, tracking costs, and writing documentation for how everything works. Tell me about a time you had several different tasks competing for your attention at once. How did you decide what to work on first, and how did you make sure the other tasks didn't get forgotten?
  **A (criteria):** A real prioritization framework (not "I did everything"), evidence the deprioritized items weren't dropped/forgotten, some system for tracking commitments even if informal — signal specifically for multitasking and keeping commitments.
- **Q:** Tell me about a time you caught a subtle bug or configuration issue in a production ML system that wasn't obvious and nobody else had noticed.
  **A (criteria):** A specific example, a real explanation of *how* they found it (not luck), genuine attention to detail — signal specifically for the "Attention to details" sub-trait.
- **Q:** How do you keep up with MLOps tooling and practices, given how fast this space moves?
  **A (criteria):** A real, specific habit or practice (not a vague "I read blogs") — signal for self-education specifically.

---

## 6. Segment 4 — Task (20–25 min): pick ONE

Choose based on time, what Segment 3 already covered well, and where you want deeper signal.

### Option A — Config Reading & Debugging

Show the CronJob spec below and ask the candidate to find what's wrong with it and explain the production impact of each issue. Reasoning, not live editing — a correct verbal/written diagnosis is the deliverable.

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hourly-feature-backfill
spec:
  schedule: "0 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backfill
            image: registry.internal/feature-backfill:latest
            command: ["python", "run_backfill.py", "--hour=$(date +%H)"]
            resources:
              limits:
                memory: "256Mi"
                cpu: "250m"
          restartPolicy: OnFailure
```

*Planted issues (the answer key):*
1. **No `concurrencyPolicy` set** — defaults to `Allow`, so if a run takes longer than an hour, or a previous run is still retrying, overlapping runs can race and double-process the same hour.
2. **`image: ...:latest`** — not pinned, breaks reproducibility. A backfill run days later could silently pull a different image than what originally ran.
3. **`memory: 256Mi`** — unrealistically low for a backfill job processing real feature data; will get OOMKilled under real load.
4. **`$(date +%H)` evaluated inside the container at run time** rather than passed as a parameter from the scheduler — if the job retries near a time boundary or runs late, the computed hour could be inconsistent with the actual intended backfill window. A subtle correctness bug, not just a resource issue.
5. **No visible idempotency safeguard** — combined with issue #1, a strong candidate should ask: "how does `run_backfill.py` guarantee the same hour can't be double-processed if the job runs twice?"

A candidate who finds 3+ of these, and explains *why* each matters (not just "that looks wrong"), is doing well. Bonus signal: connecting issue #1 and #5 together as the same underlying risk.

### Option B — Model Registry & Promotion Design

> Prompt: "A team currently promotes new models by having someone manually copy a model file to a production bucket after eyeballing metrics in a notebook. Design what you'd build instead, and make it something that could survive an audit."

**What to look for:**
- Versioning tied to lineage (what data/code/config produced this version)
- An automated evaluation gate with a defined metric and threshold — not "eyeball it"
- A recorded, queryable decision trail (what was evaluated, what won, when, why)
- A clear promotion path — a defined state machine, not an ad hoc copy
- Bonus: separation between candidate and production states, and a rollback path

**Above bar vs. at bar:** above-bar candidates independently raise the audit trail and rollback without being prompted; at-bar candidates design a reasonable registry/versioning scheme but need a nudge ("how would someone outside the team check why this was promoted?") to get to the auditability piece.

### Option C — System Design: The Predictive Decisioning Platform

Give the prompt below, then let the candidate drive; ask clarifying questions as needed.

> "You're building the platform behind a predictive decisioning system that scores real-time traffic for many different customers, each with their own model instance. Design the platform end-to-end: how models get trained and retrained on a schedule, how a new model version gets evaluated and safely promoted into production, how you support many customers' models on shared infrastructure without their data or models leaking into each other, how you catch it when a model's quality quietly degrades in production, and how you'd prove all of this actually works to someone auditing the system afterward."
  
**Facilitation guide — track U (unprompted) / P (prompted) / M (missing) per topic:**

| Topic | Nudge if it doesn't come up |
|---|---|
| Training orchestration & scheduling (multiple cadences, retries) | "Models need retraining on some cadence — how does that actually get triggered and run?" |
| Idempotent backfills | "What happens if a scheduled run fails and needs to be re-run?" |
| Model registry & versioning | "Once a model is trained, where does it live, and how do you track which version is which?" |
| Evaluation gate before promotion | "How do you decide a new version is actually ready for production?" |
| Safe rollout (shadow mode / champion-challenger) | "Would you just flip a switch to the new version, or something safer?" |
| Per-tenant isolation | "How do you stop one customer's data or model from touching another's?" |
| Serving/publish path, freshness SLO, fallback on failure | "What happens to a request if the serving layer is behind or down?" |
| ML-specific monitoring vs. plain service metrics | "Uptime looks fine — how would you know the model itself has quietly gotten worse?" |
| Reproducibility | "Months later, someone asks exactly what model served a specific decision — can you answer that?" |
| Cost instrumentation per tenant | "How would you know what it costs to run this for one specific customer?" |
| Collaboration boundary with DevOps/SRE | "Who owns the actual deployment pipeline — you, or another team?" |

**Above bar vs. at bar vs. below bar:**
- **Above bar:** independently designs the full loop — orchestration with idempotent retries → registry with an audit-able evaluation gate → shadow/champion-challenger rollout → per-tenant isolation with a concrete notion of proof → ML-specific monitoring distinct from plain uptime → reproducibility → cost attribution — and reasons about the DevOps/SRE boundary without being asked.
- **At bar:** gets the core pipeline right (train → evaluate → promote → serve) but needs 2–3 nudges to reach isolation evidence, ML-specific monitoring, or reproducibility.
- **Below bar:** designs a single-tenant, single-model training pipeline with no promotion gate, isolation story, or ML-specific monitoring, even after nudges — this is the shape of a data scientist's personal project, not a platform.

---

## 7. Segment 5 — Candidate Questions

No prep needed. Note whether questions focus on the actual platform/systems challenges vs. purely logistics/comp.

---

## 8. Scorecard

Score 1 (below bar) – 4 (exceptional) per row. This is the internal technical scorecard used to reach a hire decision — separate from `candidates/review_template.md`, which is the client-facing write-up format.

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

**Guidance on weighting:** Kubernetes/Docker, the registry/lifecycle, orchestration/idempotency, monitoring, and multi-tenant isolation are the hardest to coach and most central to this specific role — a candidate can be coached up on which exact IaC tool or cloud console to use, but not easily on the instinct to demand idempotency, audit trails, and tenant isolation by default. Self-organization is included as its own row because the Interview Criteria doc scores it explicitly and the technical Q&A alone won't reliably surface it — see §5.9.

---

## 9. Decision guidance

- **Strong hire:** at or above bar on all High-weight rows; demonstrates the idempotency/audit-trail/isolation instinct unprompted somewhere in Q&A or the task; no more than one Medium-weight row below bar.
- **Borderline:** strong on the infrastructure/ops side (K8s, orchestration, monitoring) but thin on the ML-specific side (registry/lifecycle, drift/calibration), or vice versa — worth a focused follow-up on the specific gap rather than an outright pass.
- **No hire:** below bar on Kubernetes/Docker or orchestration/idempotency (the hardest to coach), or designs a single-tenant, ungated system even after nudges — both are central and not easily developed on the job for a Senior-level hire on this specific platform.

---

## 10. Notes for future rounds

This plan has not yet been run against a real candidate — recalibrate question difficulty and the task menu after the first few interviews, and note here if the client shares their own example exercise.
