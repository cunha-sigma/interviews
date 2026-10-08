# Interview Notes — {{CANDIDATE_NAME}}

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. All bank questions are pre-listed with their expected answer, so you only need to check the box for whichever ones you actually ask and fill in the candidate's answer under them — leave the rest untouched. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Résumé Summary (pre-filled from `cv.md` before the interview)

-

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

-

## Segment 2 — Role Context (interviewer-led, 5 min)

Candidate reactions / questions during this segment, if any:

- 

## Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 across categories based on time and where the candidate is weak/strong. Only fill in the ones you actually ask.

### Rule-Logic & Redundancy Reasoning

- [ ] **Q:** Imagine you have two separate detection rules. Rule 1 flags something when two particular conditions are both true. Rule 2 flags something when one of those same conditions is true, plus a third condition that, in your data, is almost always true whenever the second condition is true. How would you figure out — using data, not just reading the rules — whether these two rules are actually catching mostly the same cases?
  > Expected: Don't reason from the rule text alone — pull the actual flagged-event sets and measure real overlap over a representative time window, then check whether the remaining difference is operationally meaningful before merging/retiring either rule.
  Candidate's answer: 

- [ ] **Q:** Say you've confirmed two rules almost always fire together. One is much simpler to read and cheaper to run than the other. What would stop you from immediately deleting the more complex one?
  > Expected: The rules might diverge specifically on rare, high-value cases — look at where they disagree, not just the overall overlap rate, and understand why the complex rule was built before assuming it's pure redundancy.
  Candidate's answer:

- [ ] **Q:** These kinds of rules are sometimes written directly in code — for example, as logical expressions in JavaScript using `&&` and `||`. If you saw a rule written with both mixed together without parentheses, what would you want to check before assuming you understood exactly what it evaluates to?
  > Expected: `&&` binds tighter than `||` in JavaScript, so grouping may not match an intuitive reading — the right instinct is checking precedence/adding explicit parentheses/testing directly, not confidently guessing.
  Candidate's answer:

### Python Reasoning

- [ ] **Q:** You're looking at a Python function that loops through a list of detection rules, and for each rule, loops through every event in a dataset to check whether it matches. The dataset has millions of events and there are hundreds of rules. What's the performance concern, and how would you address it?
  > Expected: Nested loop (~rules × events) is slow at this scale; reasons toward vectorized/batch evaluation across the whole dataset at once, and/or pre-filtering on key signals before running full rule logic.
  Candidate's answer:

- [ ] **Q:** If you inherited a Python codebase with no tests and needed to understand what a specific function actually does before changing it, what would you do beyond reading the code top to bottom?
  > Expected: Trace actual call sites, run it against known inputs, check version history/tickets, write characterization tests before changing anything.
  Candidate's answer:

### SQL on Large, Complex Datasets

- [ ] **Q:** You need to compare the output of two detection rules against a very large table of events — tens of millions of rows per day. What would you think about to avoid the query running too slowly?
  > Expected: Filter/aggregate early, use partitioned/indexed columns, prefer set-based operations, avoid accidental cross joins, check the query plan.
  Candidate's answer:

- [ ] **Q:** How would you write a query to find, among all events flagged by either of two rules, which ones were flagged by both?
  > Expected: Join on a shared event ID between the two flagged-sets, or `INTERSECT`.
  Candidate's answer:

### Data-Flow & Root-Cause Investigation

- [ ] **Q:** You're told a specific detection rule has been producing more false positives lately, but nobody knows why, and you don't yet know how the data feeding it is produced. How would you start investigating?
  > Expected: Pin down what "false positive" means with examples, trace the rule's signals back to where they're computed, check for recent upstream changes, compare before/after, then form a specific testable hypothesis.
  Candidate's answer:

- [ ] **Q:** Say two systems both produce a count for what's supposed to be the same kind of event, but the numbers never quite match. What are some concrete reasons that could legitimately happen, even with both systems working correctly?
  > Expected: Different time windows/timezones, different event-identity/dedup definitions, ingestion-time vs. completion-time counting, filtering differences.
  Candidate's answer:

### Hypothesis-Driven Analysis & Stakeholder Communication

- [ ] **Q:** Say you've found that a specific detection rule almost never catches anything on its own — most of what it flags is already caught by other rules. How would you decide whether it's safe to remove, and how would you explain that to someone without a technical background?
  > Expected: Validate over a long enough window (rare/seasonal cases), check impact on catching genuinely new patterns, propose a safe rollout (e.g. non-blocking monitoring first). Communication: leads with outcome/risk, no jargon.
  Candidate's answer:

- [ ] **Q:** You suspect a detection rule might be too strict and is flagging legitimate activity as suspicious. How would you confirm or disprove that, using data?
  > Expected: Define concretely what a false flag would look like, pull a sample, validate against an independent signal/manual review — not circular reasoning using the same signals the rule relies on.
  Candidate's answer:

### Ambiguity Tolerance & Ownership (behavioral — evaluate against criteria)

- [ ] **Q:** Tell me about a time you had to investigate a complex system you didn't build yourself, with little or no documentation, to make a change to it. How did you approach it, and what did you do when you got stuck?
  > Criteria: concrete specific example, real investigative method beyond "read the code," evidence of resolving blockers independently.
  Candidate's answer:

- [ ] **Q:** Tell me about a time your priorities changed suddenly while you were in the middle of something. What did you do?
  > Criteria: real re-scoping example, awareness of trade-offs made, stayed effective through the change.
  Candidate's answer:

- [ ] **Q:** Describe a time you had to explain a technical finding to someone without a technical background. What did you do to make it land?
  > Criteria: genuine translation (no jargon, business/operational framing), bonus for checking understanding and adjusting.
  Candidate's answer:

## Segment 4 — Task (20–25 min)

Option used: [ ] A — Rule Redundancy & Simplification   [ ] B — Live Root-Cause Investigation

Notes on approach:

- 

**Option A facilitation (if used):**

| Finding | U/P/M | Notes |
|---|---|---|
| Rule 3 simplified to `signal_A` (Boolean algebra) | | |
| Rule 1 recognized as subset of Rule 3 | | |
| Rule 2 recognized as subset of Rule 1/Rule 3 (full chain) | | |
| Rule 4 flagged as dead/always-false | | |
| Rule 5: proposes concrete data-driven overlap check | | |

**Option B facilitation (if used):**

| Topic | U/P/M | Notes |
|---|---|---|
| Clarifies what changed (code/config/deploys) | | |
| Identifies the rule's specific signals | | |
| Checks if signal computation changed upstream | | |
| Considers population/traffic-mix shift vs. rule behavior change | | |
| Segments the spike instead of treating it as uniform | | |
| Forms a specific, testable hypothesis | | |
| Considers immediate mitigation/urgency | | |

Overall bar level for this segment: [ ] Above bar   [ ] At bar   [ ] Below bar

## Segment 5 — Candidate Questions (5–10 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

-

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|---|---|
| Rule-logic & redundancy reasoning | High | | |
| Data-flow / root-cause investigation | High | | |
| Hypothesis-driven analysis | High | | |
| Communication to non-technical stakeholders | High | | |
| Ambiguity tolerance & independent ownership | High | | |
| Python reasoning | Medium | | |
| SQL on large, complex datasets | Medium | | |
| JavaScript familiarity | Medium | | |
| Snowflake / domain exposure | Low | | |

## Decision Lean

[ ] Strong hire   [ ] Borderline   [ ] No hire

Justification:

-

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-