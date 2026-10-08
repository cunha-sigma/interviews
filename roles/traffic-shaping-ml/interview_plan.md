# Interview Plan — ML Engineer to Support the Traffic Shaping System

**Interviewer:** Rodrigo Cunha, Principal ML Engineer
**Role:** Senior/Principal ML Engineer (per `role_description.md`, general framing — Scala/Spark/AWS/Kafka background). This plan generalizes the technical bar beyond that JD's specifics: the JD is a lighter, earlier description of the role; the actual day-to-day work is building and operating systems like the traffic shaping architecture in this folder. No conflict — just a finer bar than the JD states.
**Follows the timeline already committed in** `interview_structure.md` (kept intact below, translated to English for this working document).

---

## 1. What this interview is screening for

Rodrigo's stated bar for someone who can actually help him on this system:

| Area | Why it matters here |
|---|---|
| Solid software engineering (not an SE-blind data scientist) | This system is a distributed, low-latency production service, not a notebook |
| Solid ML fundamentals | Binary classification, imbalanced data, threshold selection, drift |
| MLOps/DevOps | Model registry, versioned artefact promotion, retraining pipelines |
| K8s, Docker | Two independently-scaled Deployments, KEDA, readiness probes |
| Low-latency real-time serving | gRPC, NVIDIA Triton, P99 budget under 10ms, load/stress testing (k6/ghz-style tools) |
| Self-management, independence, ownership | Principal-adjacent role — expected to own ambiguity, not wait for spec |
| Math foundations | Algebra, statistics, calculus, probability — needed to reason about thresholds, calibration, latency distributions |
| Big-O / complexity reasoning | Reading code and correctly assigning time/space complexity, not necessarily writing it live |

**Deliberate design choice:** the live system-design prompt (Section 5) gives concrete domain framing (System A / System B, a filter-or-pass decision, a strict latency budget) but deliberately withholds the ML mechanism — it does not say "classifier" or "threshold." The goal is to see what the candidate reaches for unprompted: do they land on a binary classifier with thresholds on their own? Do they consider alternatives? Do they even think to ask about thresholds, feature freshness, or fallback behavior? Handing them the full reference architecture up front would only test recall of a system they didn't design. Use the reference architecture in this repo as your internal answer key, not as the prompt.

---

## 2. Timeline (as committed in `interview_structure.md`)

| # | Duration | Segment |
|---|---|---|
| 1 | 10 min | Candidate introduces themselves — background, story |
| 2 | 10 min | Interviewer explains role context, challenges, expectations |
| 3 | 10–20 min | Technical Q&A |
| 4 | 10–20 min | Live system design problem (real-time classification service, P99 < 10ms) |
| 5 | 5 min | Candidate's questions |

Total: ~45–65 minutes. Sections 3 and 4 below are the content to slot into steps 3 and 4.

---

## 3. Segment 1–2 prep notes (15 min, low effort)

- **Segment 1:** Let them drive. Listen for: did they *own* systems end-to-end or execute tickets? Do they talk about trade-offs unprompted, or only outcomes? This is your first signal on "self-management/independence."
- **Segment 2 talking points:** the system intercepts every bid request and must return bid/skip in <10ms P99 at millions of req/sec; ML failure must never take down bidding; weekly-minimum retrain cadence. Keep it high-level — don't pre-load the architecture, since Segment 4 depends on them not having seen it.

---

## 4. Segment 3 — Technical Q&A (10–20 min)

Pick ~5-7 questions total across categories based on time and where the candidate is weak/strong. Don't run all of them — this is a bank, not a script.

### 4.1 Software Engineering & Big-O (no live coding required — reasoning only)

Show the snippet, ask for time and space complexity, and what they'd change. Full derivations below so you can defend/probe any answer the candidate gives, including ones that disagree with the "expected" answer for defensible reasons.

**Snippet A — duplicate detection**
```python
def has_duplicate_vectors(vectors: list[tuple[float, ...]]) -> bool:
    for i in range(len(vectors)):
        for j in range(i + 1, len(vectors)):
            if vectors[i] == vectors[j]:
                return True
    return False
```

*Time:* the outer loop runs `n` times. For each `i`, the inner loop runs `n - 1 - i` times. Summing across all `i`: `(n-1) + (n-2) + ... + 1 + 0 = n(n-1)/2`. Drop constants and lower-order terms → **O(n²)**. This is the classic arithmetic-series sum, the same pattern as "how many handshakes in a room of n people."

*A pedantic-but-correct add-on:* each `vectors[i] == vectors[j]` comparison isn't O(1) — comparing two tuples of dimension `d` costs O(d) in the worst case. So the fully precise bound is O(n²·d). In practice `d` (the feature-vector dimension) is fixed/constant relative to `n` (the number of vectors), so it's dropped and people just say O(n²) — but a candidate who *notices* this and explicitly says "I'm treating d as constant" is showing more rigor than one who doesn't think about it at all. Don't penalize either the "just O(n²)" answer or the more pedantic one — both are fine; what you're checking is whether they can walk you through *why*, not which phrasing they use.

*Space:* no extra data structure is allocated that grows with input — just loop counters. **O(1)** extra space (the input list itself isn't counted).

*Follow-up — how do you get to O(n)?* Put each vector into a `set` as you go; check membership before inserting. Hashing a tuple of dimension `d` costs O(d), so each of the `n` insertions/lookups is O(d) (constant relative to n) → **O(n)** time overall, but now **O(n)** space too, since the set holds up to n vectors. This is a genuine time/space trade — a good candidate names it explicitly ("I'm paying O(n) memory to buy back the O(n²) time").

**Snippet B — rolling feature aggregation**
```python
def rolling_avg(values: list[float], window: int) -> list[float]:
    result = []
    for i in range(len(values)):
        start = max(0, i - window + 1)
        result.append(sum(values[start:i+1]) / (i - start + 1))
    return result
```

*Time:* the outer loop runs `n` times (once per element). Inside, `values[start:i+1]` builds a slice of up to `window` (call it `k`) elements, and `sum(...)` walks all of them — so each iteration costs up to O(k). Total: **O(n·k)**. Note this is *not* O(n²) unless `k` scales with `n` — if `window` is a fixed small constant (e.g. 5), this is technically O(n), but the useful answer names `k` as its own variable rather than silently assuming it's constant, since in the real system `window` could be large (e.g. a 24-hour rolling window over per-minute data).

*Space:* `result` grows to size `n` → O(n) for output. The temporary slice inside the loop is O(k) but is discarded each iteration (not accumulated), so it doesn't add to the running total, just to peak memory at any one instant.

*Follow-up — how do you get to O(n) regardless of window size?* Maintain a running sum instead of re-summing the window every time: keep a `current_sum`; when the window slides forward by one, `current_sum += values[i]` (new element entering) and `current_sum -= values[i - window]` (old element leaving, once you're past the initial fill). That's O(1) work per element instead of O(k) → **O(n)** total time, same O(n) output space, but now only O(1) *additional* space beyond the output. This is exactly the shape of the online-features rolling-aggregation job — connecting the abstract complexity fix to "this job runs every 5 minutes over a lot of rows, so O(n·k) vs O(n) is a real cost difference" is the signal you want.

**Snippet C — recursive scoring without memoization**
```python
def combined_risk(n: int) -> int:
    if n <= 1:
        return n
    return combined_risk(n - 1) + combined_risk(n - 2)
```

This is structurally identical to naive recursive Fibonacci — same recurrence, same failure mode. Worth understanding thoroughly since it's the one most candidates get wrong or hand-wave.

*Why it's exponential — build the call tree.* Every call to `combined_risk(n)` (for `n > 1`) makes exactly two recursive calls: `combined_risk(n-1)` and `combined_risk(n-2)`. Draw it for `n = 4`:

```
                    combined_risk(4)
                   /                \
          combined_risk(3)         combined_risk(2)
          /            \            /            \
  combined_risk(2)  combined_risk(1) combined_risk(1) combined_risk(0)
   /        \           |                 |               |
 cr(1)     cr(0)      returns 1        returns 1        returns 0
  |          |
returns 1  returns 0
```

Notice `combined_risk(2)` is computed twice, and `combined_risk(1)` is computed three times, from scratch, independently, with zero sharing between the branches. That redundant re-computation is *the whole story* — nothing is cached, so the same subproblem gets solved over and over, and the number of redundant solves compounds as `n` grows.

*Formalizing it:* let `T(n)` = number of calls made to compute `combined_risk(n)`. From the code: `T(n) = T(n-1) + T(n-2) + 1` (one call each for the two recursive branches, plus the current call itself), with `T(0) = T(1) = 1`. This recurrence has *the same shape as the Fibonacci recurrence itself* — and the solution to that recurrence is known to grow as `φⁿ` where `φ ≈ 1.618` (the golden ratio). So the tight bound is **Θ(φⁿ)** — exponential. Since `φ < 2`, `O(2ⁿ)` is also a valid (looser, easier-to-state) upper bound, which is why "O(2ⁿ)" is the answer you'll hear most often and why it's an acceptable answer — but a candidate who says "closer to 1.6ⁿ, but exponential either way" is demonstrating deeper recurrence-relation fluency. Don't require the tight bound; do give extra credit for it.

*Intuitive shortcut if you don't want to derive the recurrence:* a call tree where (almost) every node spawns 2 children and the tree has depth `n` looks like a binary tree of depth `n`, which has up to `2ⁿ` nodes. That's a valid, if slightly loose, way to arrive at "exponential, O(2ⁿ)" without solving the recurrence formally — this is the level most candidates should be able to reach, and is fine.

*Space — the part people get wrong in the other direction.* Despite branching into 2 calls at every level, the space complexity is **O(n), not O(2ⁿ)**. Why: Python (like most languages) evaluates function calls sequentially, depth-first — `combined_risk(n-1)` is called, and *fully returns*, before `combined_risk(n-2)` is even started. So at any given instant, only *one path* from root to a leaf is sitting on the call stack, not the whole tree. The maximum depth of that path is `n` (each level decrements `n` by 1 until it hits the base case), so the call stack holds at most O(n) frames at once. **This distinction — "number of calls made over the whole execution" (exponential) vs. "maximum call-stack depth at any one moment" (linear) — is exactly what separates a candidate who's memorized "recursion = exponential" from one who actually understands how a call stack executes.** If a candidate says O(2ⁿ) space, that's the specific gap to probe on.

*Follow-up — how do you fix the time complexity?*
1. **Memoization (top-down):** cache the result of `combined_risk(k)` the first time it's computed; on any later call for the same `k`, return the cached value instead of recomputing. There are only `n` distinct subproblems (`combined_risk(0)` through `combined_risk(n)`), each now does O(1) work beyond its (cached) recursive calls → **O(n) time**, O(n) space for the cache plus O(n) call stack.
2. **Iterative / bottom-up DP:** just keep the last two values in a loop from `2` to `n`, no recursion at all → **O(n) time, O(1) additional space** (no cache, no call stack growth). This is strictly better than the memoized version on space, and is the answer that shows the candidate can eliminate recursion entirely when it isn't needed, not just patch it.

### 4.2 ML Fundamentals
- **Q:** Bias/variance trade-off — explain in your own words, and give an example of a real fix for each.
  **A:** Bias = systematic error from an overly simple model (underfitting) → fix by adding features/capacity or reducing regularization. Variance = error from over-sensitivity to the training set (overfitting) → fix by regularization, more training data, simpler model, or cross-validation.
- **Q:** The traffic shaping decision is a binary classifier on a very imbalanced dataset (most requests are skipped or most are bid on, depending on publisher). How does that change your choice of metric?
  **A:** Prefer AUC-PR (and precision/recall at the operating threshold) over accuracy or plain ROC-AUC, since both are misleadingly optimistic under imbalance; also check calibration, since imbalance skews raw predicted probabilities.
- **Q:** Why would you use a time-based train/test split instead of a random split for a system that scores live traffic?
  **A:** Random splits leak information through correlated rows (same session/user/publisher in both train and test), inflating offline metrics; a time-based split mirrors production, where the model only ever predicts on the future given the past.
- **Q:** What's the difference between a recall-based threshold and an ROI-based threshold, and when do you have enough data to justify one over the other?
  **A:** Recall-based = pick the cutoff hitting a target recall/coverage regardless of cost. ROI-based = pick the cutoff maximizing expected business value by weighing the cost of false positives vs. false negatives. ROI-based requires enough volume and reliable cost/outcome data to estimate that trade-off — otherwise default to recall-based.
- **Q:** What is model drift, and how would you detect it without ground-truth labels arriving in real time?
  **A:** Drift = the input distribution or the input→output relationship changes over time. Detect without live labels via input/feature distribution monitoring (e.g. PSI, KL divergence), prediction-score distribution shifts, and delayed/proxy labels once they eventually arrive.

### 4.3 MLOps / DevOps
- **Q:** You retrain a model weekly. What has to be versioned together, and why?
  **A:** Feature encoder + classifier weights + threshold/config must be versioned and promoted as a single atomic bundle — deploying any one independently risks feature/encoding mismatch or a threshold tuned for the wrong score distribution.
- **Q:** Walk me through a deployment strategy for pushing a new model version into production with the lowest possible risk.
  **A:** Shadow (score live traffic, take no action) → canary/small-% A/B (act on a limited slice, bound the blast radius) → ramp up → full promote — each stage validates a different risk (correctness, then real-world impact, then scale).
- **Q:** If you had to split A/B traffic for a system where the same customer must always see the same model version, how would you do the split?
  **A:** Deterministic hash of a stable customer/user ID into buckets (e.g. hash(id) % 100), not random per-request assignment.
- **Q:** What goes wrong if the training pipeline reads directly from the production transactional database?
  **A:** Load/performance impact on the prod DB, tight schema coupling/brittleness, risk of reading uncommitted/in-flight state (leakage), and no stable/reproducible snapshot for training — should read from a replica, warehouse, or feature store instead.

### 4.4 Kubernetes / Docker / Low-Latency Serving
- **Q:** What's the difference between a liveness probe and a readiness probe, and why does a readiness-probe failure matter more on the request-serving path?
  **A:** Liveness = "is the process alive; restart if not." Readiness = "is it ready to serve traffic; pull from load-balancer rotation if not." A pod can be alive but not warmed up/overloaded — readiness is what actually protects request traffic.
- **Q:** Why would you choose KEDA over the standard Horizontal Pod Autoscaler for a service whose load is inference-request volume, not CPU?
  **A:** KEDA scales on custom/external metrics (queue depth, request rate), while stock HPA natively scales mainly on CPU/memory, which doesn't track inference load well.
- **Q:** Why gRPC instead of REST/JSON for a service-to-service call inside a 10ms latency budget?
  **A:** gRPC uses HTTP/2 with binary protobuf serialization (fast to encode/decode, multiplexed/streaming connections) vs. REST/JSON's text parsing and HTTP/1.1 overhead — meaningfully cheaper per call.
- **Q:** What is NVIDIA Triton, and what problem does it solve that a plain Flask/FastAPI model server doesn't?
  **A:** An inference-serving system providing dynamic batching, multi-model/multi-framework serving, GPU/CPU-optimized concurrent execution, and model ensembles/DAGs out of the box — a plain Flask/FastAPI server just wraps a Python function and does none of this. (Partial credit for naming any one of these; full credit for batching or ensembles.)
- **Q:** If you had to prove a service meets a P99 < 10ms SLA before shipping it, how would you test that?
  **A:** Load-test with k6/ghz/locust, ramping concurrent request rate, measuring latency percentiles (not averages) at each load level, until finding the saturation/breaking point — not a single one-off run.
- **Q:** A single pod in your serving deployment dies. What should happen, and what should *not* happen?
  **A:** Should: readiness probe fails fast, pod pulled from the load balancer, K8s reschedules a replacement, other pods keep serving unaffected. Should not: the single pod's death blocking/delaying auction traffic or cascading into retry storms/broader outage.

### 4.5 Math Foundations
- **Q:** Give the formulas for precision and recall, and explain a scenario where you'd deliberately optimize for one over the other.
  **A:** Precision = TP/(TP+FP); Recall = TP/(TP+FN). Optimize precision when false positives are expensive (e.g. wrongly filtering good traffic); optimize recall when missing a true positive is expensive (e.g. letting fraud/abuse through).
- **Q:** What does it mean for a classifier's output to be "calibrated," and why would that matter if you're picking a threshold?
  **A:** Calibrated = predicted probability matches empirical frequency (e.g. among all 0.7-scored cases, ~70% are truly positive). Matters because an uncalibrated score makes a chosen threshold meaningless or unstable across segments/time.
- **Q:** Latency numbers are usually not normally distributed. Why do we report P99 instead of average latency, and what does that imply about how you'd size infrastructure?
  **A:** Latency is right-skewed/long-tailed, so the average hides tail behavior that still affects a meaningful share of requests; P99 reflects near-worst-case experience, so capacity must be sized for tail load, not typical load.
- **Q:** (Optional if time) Walk through, at a high level, how gradient-boosted trees differ mathematically from a single decision tree — what is each new tree fitting to?
  **A:** A single tree fits the data directly in one shot. Gradient boosting builds trees sequentially, where each new tree fits the residual error (negative gradient of the loss) left by the ensemble so far, and final prediction is the (weighted) sum across all trees.

### 4.6 Ownership / Self-Management (keep brief — 1-2 questions)
Behavioral questions — there's no single "correct answer" to match against; instead evaluate whether the candidate names a *specific* unprompted decision (not just an outcome) and gives a clear, reasonable criterion for when to decide vs. escalate.
- **Q:** Tell me about a time you owned a system end-to-end, including a decision nobody explicitly asked you to make.
  **A:** Look for: a concrete unprompted decision (not just "I built X"), the reasoning behind it, and ownership of the consequence.
- **Q:** How do you decide when to make an architecture call yourself versus escalate it?
  **A:** Look for: a real criterion (e.g. reversibility, blast radius, cost of being wrong) rather than "it depends" with no substance.

---

## 5. Segment 4 — Live System Design (10–20 min)

### 5.1 The prompt (read to candidate, verbatim-ish)

Give the business context concretely — this is real domain framing they'd get on the job, and removing it would just cost time re-deriving it. What stays open is *how a "filter or not" decision actually gets made* — that's the part you're evaluating.

> "Some quick AdTech background in case it's new to you: when someone loads a page or app with an ad slot, that impression opportunity gets offered up for sale in real time. A 'bid request' is the message describing that opportunity — things like which publisher/app, what ad slot, user/device/geo context — sent out so that potential buyers can respond with a bid in a live auction, and whoever wins gets to show their ad. This all happens in the tens-of-milliseconds range, for a huge volume of impressions per second.
>
> Here's the setup: System A sends bid requests to System B, which runs that real-time auction. We want to insert an ML system in between: it intercepts every bid request from A before it reaches B, and returns a decision — filter it out, or let it through. If it's let through, it goes to B and the auction happens as normal. If it's filtered, it's discarded and never reaches B.
>
> Constraints: the decision has to come back in under 10ms at P99, at millions of requests per second, and if this ML system fails or degrades, it must never be allowed to take down or block the auction traffic.
>
> Design this system end-to-end — from data to the actual decision to serving it in production. Walk me through it, I'll ask questions as you go."

Deliberately do **not** say the words "classifier," "score," or "threshold" — that mechanism is exactly what you want the candidate to arrive at themselves. If they ask "how should the filter/no-filter decision be made?", turn it back: "that's part of what I want you to design." Watch for whether they even recognize this as a classification problem versus reaching for pure rules/heuristics — and if they do land on a model, whether they think past "it outputs a decision" to *how* a continuous score becomes a binary action, and what alternatives exist (fixed rules, a model with a single global cutoff, per-segment cutoffs, cost-sensitive/ROI-based decisioning, a bandit-style approach, etc.).

### 5.2 Facilitation guide — what to probe if it doesn't come up unprompted

Track whether each item is: **(U)** raised unprompted, **(P)** only surfaces after a prompt/nudge, or **(M)** missing even after a nudge. Unprompted is the strongest signal.

| Topic | Nudge if it doesn't come up |
|---|---|
| Feature/data pipeline & training-serving skew | "Where does the model get its input at inference time, and how do you know it matches what it was trained on?" |
| Decision mechanism: score + threshold vs. hard-coded rule vs. something else | "The model presumably outputs a score. How do you turn that into a yes/no decision?" |
| Per-segment/per-dimension thresholds vs. one global threshold | "Would the same threshold be right for every customer/segment/geo?" |
| How the threshold value itself gets chosen | "How would you decide *what* the threshold should be — and how would you know if it's wrong?" |
| Low-latency serving stack (compiled runtime, optimized inference server, in-memory feature lookup) | "Your 10ms budget — where does that time actually go? What would you profile first?" |
| Graceful degradation / fallback if the model service is down or slow | "What happens to the caller if your ML component is completely down?" |
| Deployment safety for new model versions | "How do you roll out a new model version without risking a bad surprise in production?" |
| Monitoring / drift | "How do you know, weeks after launch, that the model is still doing a good job?" |
| Scaling strategy | "Millions of req/sec — what scales, and on what signal?" |

A strong (Principal-caliber) candidate should hit at least the threshold mechanism, degradation/fallback, and the low-latency serving stack largely **unprompted**, and should ask clarifying questions before diving in rather than assuming a single "correct" stack.

### 5.3 What "above bar" looks like vs. "at bar"

- **Above bar:** independently proposes a score + configurable threshold (and questions whether one global threshold is right), reasons about where the 10ms actually goes (network hops, encoding, inference, decision logic) instead of treating "the model" as a black box, and raises graceful degradation without being asked, since "ML failure ≠ system failure" is a first-order constraint they were explicitly given.
- **At bar:** gets to a workable pipeline (ingest → features → model → decision) with correct high-level shape, but needs 1-2 nudges to reach threshold/degradation/latency-budget reasoning.
- **Below bar:** treats this as an offline ML modeling problem (accuracy, cross-validation) without engaging the systems/latency/production-safety dimensions at all, even after nudges.

---

## 6. Segment 5 — Candidate questions (5 min)

No prep needed. Note whether their questions are about the *system/technical challenges* or purely about logistics/comp — another light signal on genuine engineering interest vs. just job-seeking.

---

## 7. Scorecard

Fill during or immediately after the interview. Score 1 (below bar) – 4 (exceptional) per row; weight reflects how hard each gap is to coach on the job.

| Dimension | Weight | Score (1-4) | Notes |
|---|---|---|---|
| SWE fundamentals / Big-O reasoning | High | | |
| ML fundamentals (imbalance, metrics, drift, leakage) | High | | |
| MLOps/DevOps (versioning, deployment safety) | High | | |
| K8s/Docker/serving infra (Triton, gRPC, autoscaling) | High | | |
| Low-latency/perf engineering instincts | High | | |
| Math foundations | Medium | | |
| System design synthesis (Segment 4, overall) | High | | |
| Ownership / self-management | Medium | | |
| Communication clarity | Medium | | |

**Guidance on weighting:** SWE fundamentals, low-latency serving instincts, and system-design synthesis are the hardest to coach and should gate the decision — a candidate can be coached up on Triton specifics or K8s API details, but not on the instinct to reason about latency budgets, failure isolation, or complexity trade-offs. ML fundamentals gaps are more coachable given Rodrigo is the ML lead on this system already.

---

## 8. Decision guidance

- **Strong hire:** above-bar or at-bar on all High-weight rows, no missing (M) items in the Section 5.2 facilitation table on threshold mechanism, degradation, and latency budget.
- **Borderline:** at-bar on most High rows but 1-2 nudges needed in Segment 4, or a clear single-category gap (e.g., strong SWE/systems but weak ML fundamentals, or vice versa) — consider a second-round deep dive targeted at the specific gap (e.g., a live k6/ghz load-test exercise, or a focused ML metrics discussion) rather than rejecting outright.
- **No hire:** below bar on SWE fundamentals/Big-O *or* treats Segment 4 as a pure offline-ML problem even after multiple nudges — both are hard to coach and central to this role.

---

## 9. Notes for future rounds (optional, not part of this single-session plan)

If signal on hands-on low-latency serving is thin from conversation alone, a natural follow-up round is a short take-home or paired exercise: containerize a toy model behind gRPC, and load-test it with k6/ghz to find the P99 breaking point. Not required for this interview, but worth keeping in mind if Segment 3/4 leave that specific skill ambiguous.