# Interview Plan — Senior Python Engineer, Forward Deployed Engineering (Tabs project)

**Role:** Senior/Strong-Mid Python Engineer (per `role_description.md`). Client is Tabs, a billing platform; the team is a Forward Deployed Engineering (FDE) org building fast, customer-specific integrations.

**A note on how solid this plan is:** unlike the Traffic Shaping ML plan, this one is *not* built from an observed interview loop — as of writing, no candidate had been presented to the client yet, and the hiring brief itself says "I don't know how the 1st interview will look." This plan is a best-effort construction from the hiring brief's stated priorities plus one concrete example exercise the client shared. Treat the timeline and Q&A bank as a reasonable starting script, not settled process — update this file once real interview feedback comes back from Tabs.

---

## 1. What this interview is screening for

| Area | Why it matters here |
|---|---|
| Ambiguity tolerance & self-directed scoping | Per the Tech Lead, this is the single hardest-to-find quality and matters more than raw tech depth — tickets can be a brief description, not a spec |
| Ownership & independent decision-making | Engineers take technical decisions within their assigned work without a customer-facing lead in the room |
| Python / FastAPI / Pydantic proficiency | The must-have stack; ~90% of builds are REST APIs on this stack |
| REST API & system-integration patterns | Idempotency, safe sync/reconciliation between systems — core of what FDE builds |
| Decimal/date-precision discipline | Billing platform — financial math must never use floats; rounding and period boundaries matter |
| Testing & observability habits | pytest alongside feature work, Sentry instrumentation — expected to be routine, not bolted on |
| Agile/Scrum & PR workflow fluency | Sprints, one PR per ticket, review culture |
| Communication | Offshore engineers work through internal Product Delivery leads, not directly with customers — proactive, clear internal communication substitutes for customer-facing polish |
| Big-O / hash-map reasoning | Directly evidenced by the client's own shared coding exercise (see §4) |

**Deliberate design choice:** because the FDE mindset (ambiguity, ownership, proactive scoping) is explicitly called out as harder to find than technical skill, don't let a strong technical performance alone carry the interview — actively probe for how the candidate behaves when a requirement is underspecified, not just whether they can write correct code.

---

## 2. Timeline (best-effort construction — see caveat above)

| # | Duration | Segment |
|---|---|---|
| 1 | 5–10 min | Candidate introduces themselves — background, story |
| 2 | 5 min | Interviewer explains role context: FDE mindset, rapid turnaround, billing-domain precision requirements |
| 3 | 10–15 min | Technical & behavioral Q&A (Python/FastAPI, integrations, decimal precision, Agile/PR workflow, ambiguity/ownership) |
| 4 | ~30 min | Live coding exercise — array reconciliation (see §4) |
| 5 | 0–15 min | Optional system-design / integration discussion, if time and signal allow |
| 6 | 5 min | Candidate's questions |

Total: ~55–80 minutes depending on whether Segment 5 runs.

---

## 3. Segment 3 — Technical & Behavioral Q&A (10–15 min)

Pick a handful based on time — this is a bank, not a script.

### 3.1 Python / FastAPI / Pydantic
- **Q:** What does Pydantic give you over plain dataclasses or dicts when defining a FastAPI request/response model?
  **A:** Runtime validation and type coercion at the API boundary, automatic error responses on invalid input, and auto-generated OpenAPI schema — dataclasses/dicts give you neither validation nor the schema for free.
- **Q:** How would you handle a long-running operation inside a FastAPI endpoint without blocking other requests?
  **A:** Use an `async def` endpoint and `await` on I/O-bound work (don't block the event loop with sync/CPU-bound calls); for genuinely long work, offload to a background task/worker queue and return immediately (e.g. 202 + status polling) rather than holding the request open.

### 3.2 REST / System Integration
- **Q:** What does "idempotent" mean for an API endpoint, and why does it matter when syncing data between two systems?
  **A:** Calling the same operation multiple times with the same input produces the same end state as calling it once — critical for integrations because retries after timeouts/failures are common, and a non-idempotent endpoint risks duplicate records/side effects on retry.
- **Q:** You need to reconcile records between our system and a customer's CRM. What's your approach to detecting what changed?
  **A:** Pull both sides, key records on a stable unique identifier (or a composite key if no single field is unique), diff by key to classify inserts/updates/deletes, and compare field-by-field (or a hash/checksum of relevant fields) to detect real changes vs. no-ops.

### 3.3 Decimal / Financial Precision
- **Q:** Why shouldn't you use `float` for currency amounts in Python, and what should you use instead?
  **A:** Floats are binary floating-point and can't represent most decimal fractions exactly (e.g. `0.1 + 0.2 != 0.3`), which compounds into real rounding errors over many operations — use Python's `Decimal` type instead, constructed from strings/ints (not from a float) to avoid inheriting float imprecision.
- **Q:** How would you round a monetary value for a customer-facing invoice, and what's a case where naive rounding causes a real bug?
  **A:** Use explicit rounding rules (e.g. `Decimal.quantize` with `ROUND_HALF_UP` or whatever the business specifies) rather than relying on default float rounding; a classic bug is summing already-rounded per-line amounts and having the total not match the sum of independently-rounded lines, or a billing period boundary (e.g. prorating a subscription) where naive date math double-counts or drops a day.

### 3.4 Agile / PR Workflow
- **Q:** What's your normal PR workflow, ticket to merge?
  **A:** Pick up ticket → branch → implement with tests → open PR referencing the ticket → address review feedback → merge — looking for "one PR per ticket" discipline and comfort with code review as routine, not exceptional.

### 3.5 Ambiguity / Ownership (behavioral — no single correct answer; evaluate against criteria)
- **Q:** Tell me about a time you were given a vague requirement or a task with missing information, and you had to figure out the rest yourself.
  **A (criteria):** Look for a *specific* real situation (not a hypothetical), evidence they proactively investigated/asked rather than waiting or guessing blindly, and that they made and owned a concrete decision.
- **Q:** How do you decide when to just make the call yourself versus go ask someone?
  **A (criteria):** Look for a real, applied criterion (e.g. reversibility, how much the decision blocks others, confidence level) rather than "it depends" with no substance.
- **Q:** Describe a project you delivered under a tight deadline with evolving requirements.
  **A (criteria):** Look for evidence of scoping/prioritizing under pressure and communicating trade-offs, not just "I worked extra hours."

---

## 4. Segment 4 — Live Coding Exercise (~30 min)

This is the one piece of the process directly confirmed by the client (shared from a real interview for a related role at Tabs), so treat it as the most reliable part of this plan.

### 4.1 The prompt

> "You're given two lists of records — `old` and `new` — synced from the same source at two points in time. Every record is a dict with the same set of fields. Two of those fields, taken together, **uniquely identify a record** — for example, `customer_id` and `product_id`. Those two fields never change for a given real-world record; the rest of the fields (e.g. `qty`, `price`) can.
>
> Write a function that compares `old` and `new` and classifies every record as:
> - **inserted** — its identifying fields (customer_id + product_id) appear in `new` but not in `old`
> - **deleted** — its identifying fields appear in `old` but not in `new`
> - **updated** — its identifying fields appear in *both*, but at least one of the other fields has a different value
>
> A record whose identifying fields appear in both lists with every field identical doesn't need to be reported — nothing changed.
>
> Example:
> ```
> old = [{"customer_id": 1, "product_id": 100, "qty": 5}, {"customer_id": 2, "product_id": 200, "qty": 1}]
> new = [{"customer_id": 1, "product_id": 100, "qty": 7}, {"customer_id": 3, "product_id": 300, "qty": 2}]
> ```
> → updated: the `(1, 100)` record (same identity, `qty` changed 5→7)
> → deleted: the `(2, 200)` record (its identity isn't in `new`)
> → inserted: the `(3, 300)` record (its identity isn't in `old`)
>
> Write the function."

If the candidate doesn't ask, tell them upfront which two fields are the identifying ones (don't make them guess/discover that from the data — it's given information, same as in the example). Do let them ask clarifying questions about edge cases (what if a "deleted" or "inserted" record should still be reported even if fields are missing, how to handle duplicate keys within one list, whether return format matters) before they start coding — that's itself a small ambiguity-tolerance signal.

**Common pitfall to watch for:** a candidate who builds the identity key out of *all* fields instead of just the given identifying ones. That breaks update-detection entirely — any value change then looks like a delete-plus-insert of two "different" records, since the key itself changed. Worth a direct nudge if you see it: "if `qty` changes, should that record's identity change too?"

### 4.2 Reference solution

```python
def reconcile(old: list[dict], new: list[dict], key_fields: tuple[str, ...]) -> dict:
    def make_key(record: dict) -> tuple:
        return tuple(record[f] for f in key_fields)

    old_by_key = {make_key(r): r for r in old}
    new_by_key = {make_key(r): r for r in new}

    old_keys = set(old_by_key)
    new_keys = set(new_by_key)

    inserted = [new_by_key[k] for k in new_keys - old_keys]
    deleted = [old_by_key[k] for k in old_keys - new_keys]
    updated = [
        new_by_key[k]
        for k in old_keys & new_keys
        if old_by_key[k] != new_by_key[k]
    ]

    return {"inserted": inserted, "updated": updated, "deleted": deleted}
```

- **Complexity:** building both lookup dicts is O(N) and O(M); the set operations and the membership/dict lookups are O(1) average per key, so overall **O(N + M)** time and O(N + M) space. A candidate reaching for nested loops (checking every old record against every new record) lands at O(N·M) — that's the gap this exercise is designed to surface.
- **What to probe for:**
  - Did they build a composite key deliberately (e.g. a tuple of the id fields), and did they think about collisions (e.g. don't just concatenate fields with no separator if values could contain the separator)?
  - Did they correctly distinguish "present in both but unchanged" from "updated" (a common bug is treating every key present in both sides as an update)?
  - Did they state the complexity, ideally comparing it to the naive nested-loop alternative?
  - Communication while coding — did they narrate their approach before/while writing it, or code silently and explain only after?

---

## 5. Segment 5 — Optional System Design / Integration Discussion (0–15 min, if time allows)

Lighter-weight than the ML role's live design segment — use only if time and signal from Segments 3-4 leave you wanting more. A reasonable prompt:

> "Say we need to sync customer records between our platform and a client's CRM (e.g. Salesforce) on a recurring schedule. Walk me through how you'd design that integration."

Listen for (don't require all of these — this is optional and lightly scored):
- Idempotency / safe retries on failure
- How they'd detect and handle conflicting/partial updates
- Error handling and observability (what happens on a failed sync — silent failure vs. alerting)
- Rate limits / pagination awareness for a real third-party API

---

## 6. Segment 6 — Candidate Questions (5 min)

No prep needed. Note whether questions lean toward the actual work (ambiguity, integrations, how the team operates) vs. purely logistics/comp.

---

## 7. Scorecard

Score 1 (below bar) – 4 (exceptional) per row. Weighting follows the brief's own stated priority: personal qualities (ambiguity tolerance, ownership, communication) matter as much as or more than raw stack depth, since the stack itself is considered straightforward.

| Dimension | Weight | Score (1-4) | Notes |
|---|---|---|---|
| Ambiguity tolerance / self-directed scoping | High | | |
| Ownership & independent decision-making | High | | |
| Live coding exercise (correctness + complexity) | High | | |
| Python / FastAPI / Pydantic proficiency | Medium | | |
| REST / integration patterns (idempotency, sync/reconciliation) | Medium | | |
| Decimal / financial precision discipline | Medium | | |
| Testing & observability habits (pytest, Sentry) | Medium | | |
| Agile / PR workflow fluency | Medium | | |
| Communication clarity | High | | |

---

## 8. Decision guidance

- **Strong hire:** solves the live coding exercise correctly with the efficient (hash-map) approach, or gets there quickly with light prompting; shows a real, specific example of independently handling ambiguity; communicates clearly throughout.
- **Borderline:** solves the coding exercise but needs significant prompting to reach the efficient approach, or is technically strong but gives only vague/generic answers on ambiguity and ownership (no concrete example) — worth a second conversation focused specifically on a past ambiguous/underspecified project.
- **No hire:** cannot solve the live coding exercise even with prompting (weak Big-O/hash-map reasoning), or shows a clear preference for waiting on detailed specs rather than scoping independently — both are central to how this team actually works day to day.

---

## 9. Notes for future rounds

Once real interview feedback comes back from the client (Tabs), revisit §2 (timeline), §3 (Q&A bank), and §5 (optional system design) — they're currently built from the hiring brief and a single shared exercise, not from an observed loop.
