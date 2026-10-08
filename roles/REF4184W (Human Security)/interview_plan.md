# Interview Plan — Senior Data Scientist, Analytics/Insights track, non-ML (REF4184W)

**Role:** Senior Data Scientist, per `job_description.md` and `raw_role_brief.md`. Fully remote/hybrid. Client is a global cybersecurity company; this specific team works on a large-scale analytics and detection system within the client's Enterprise product line. **This is explicitly not a machine-learning role.** The first project is analyzing an existing set of detection logic — complex rule-based expressions built on multiple data signals — to understand its structure, find redundancy and unnecessary complexity, and propose ways to simplify and consolidate it without losing detection effectiveness. The product itself does bot/threat detection, but this project is not about analyzing bots or threats directly — it's about the data-engineering and analytical problem of simplifying the detection system itself.

**A note on how this plan was built:** there was no existing interview script for this role — this is a first version, constructed directly from `job_description.md` and `raw_role_brief.md`'s responsibilities/requirements, plus the client's `Interview Criteria and Questions.pdf` (transcribed as `candidates/review_template.md`, which defines the final write-up's scoring bars but has no technical content of its own). No client example exercise was shared, so the task menu in §6 is designed from scratch, directly from the JD's own description of the detection-logic-simplification problem. Revisit once real interviews produce feedback.

---

## 1. What this interview is screening for

| Area | Why it matters here | JD/brief signal |
|---|---|---|
| Rule-logic & redundancy reasoning | The core, named problem: reading and reasoning about complex rule-based/conditional expressions to find redundancy and simplify them | "Analyze existing detection logic... to understand current structure, complexity, and redundancy"; "strong engineering understanding — able to read, navigate, and reason about complex logic/code" |
| Python reasoning | Explicitly required, and this role reads/reasons about code more than it writes new systems | "Proficient in Python and SQL" |
| SQL on large, complex datasets | Explicitly required, and the investigative work runs on production-scale data | "Proficient in Python and SQL, with experience working across large, complex datasets" |
| Data-flow / root-cause investigation | The actual day-to-day work: tracing how signals are gathered and processed, and why | "Investigate underlying data flows: how signals are gathered, processed, and used, and why they were designed that way" |
| Hypothesis-driven analysis | Explicitly named as the operating method, not just general analytics | "Form hypotheses about inefficiencies or redundancies and validate them through data-driven analysis"; "drive analysis to data-backed conclusions with minimal guidance" |
| Communication to non-technical stakeholders | Explicitly required, separate from the technical work itself | "Translate technical findings into clear, actionable recommendations for both technical and non-technical stakeholders" |
| Ambiguity tolerance & independent ownership | Called out repeatedly and heavily weighted — this is a brand-new team with no established process | "High seniority and a strong track record of working independently"; "Ability to work autonomously with minimal supervision"; Personal Profile: "Comfortable working in ambiguous environments," "Strong ownership mentality" |
| JavaScript familiarity | Explicitly named as a requirement, not just a plus | "Familiarity with JavaScript" |
| Snowflake / domain exposure | Named as a plus, not required | "Familiarity with Snowflake is a nice to have"; "Experience in cybersecurity, fraud detection, or risk analytics domains" (plus) |

**Deliberate design choice on the exercise (§6):** since this role is explicitly about reading and reasoning about existing logic rather than building new ML systems, the live exercise centers on logic simplification and live root-cause investigation — not a coding or modeling task. These test exactly what the JD says the first project requires: finding redundancy in rule-based expressions, and investigating why a data system behaves the way it does.

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

Let them drive. Listen for: do they describe themselves investigating systems and forming their own conclusions, or mostly executing analyses someone else scoped for them? Do they talk about tracing a problem back to its root cause unprompted, or only describe final results? First signal on ambiguity tolerance and hypothesis-driven thinking, ahead of the behavioral questions in §5.6.

---

## 4. Segment 2 — Role Context (what to tell the candidate)

Read/paraphrase this to the candidate — built directly from `job_description.md` and `raw_role_brief.md`:

> "Our customer is a cybersecurity company. Part of their product works by flagging suspicious activity using a large set of rules — each rule combines several different pieces of measurable data, which we call signals, using logical conditions, to decide whether something should be flagged. Over time, a lot of these rules have accumulated, and it's not clear anymore which ones are still pulling their weight versus doing the same job as another rule, or combining signals in an overly complicated way that nobody has revisited in a while. To be clear, this specific project is not about analyzing bots or security threats themselves — it's about understanding this rule system and the data flowing through it well enough to simplify it: finding redundant or overly complex rules, tracing how the underlying data is gathered and processed, and proposing changes that make the system simpler without making it worse at catching what it's supposed to catch. You'd need to figure out the downstream impact of any change before it ships, and explain your reasoning clearly to both engineers and people without a technical background. This is a newly forming team, so you'd be working independently much of the time, pulling in engineers, other data scientists, and security researchers for context rather than waiting for things to be handed to you."

**Keep it at this level — do not go further.** Don't describe specific investigative techniques (boolean simplification, subset detection, overlap analysis, upstream tracing) — that's exactly what Segment 4 is designed to see the candidate reach on their own.

---

## 5. Segment 3 — Technical Q&A (20–25 min)

Pick ~6–8 questions total across categories based on time and where the candidate is weak/strong — this is a bank, not a script.

### 5.1 Rule-Logic & Redundancy Reasoning

- **Q:** Imagine you have two separate detection rules. Rule 1 flags something when two particular conditions are both true. Rule 2 flags something when one of those same conditions is true, plus a third condition that, in your data, is almost always true whenever the second condition is true. How would you figure out — using data, not just reading the rules — whether these two rules are actually catching mostly the same cases?
  **A:** Don't reason from the rule text alone — correlation between the inputs doesn't guarantee the outputs overlap, especially once multiple conditions compound. Pull the actual sets of events each rule flags and directly measure the overlap (e.g., what fraction of Rule 2's flags are also flagged by Rule 1), over a time window long enough to be representative. Then check whether the small remaining difference — cases each rule uniquely catches — is operationally meaningful before deciding to retire or merge either one.
- **Q:** Say you've confirmed two rules almost always fire together. One is much simpler to read and cheaper to run than the other. What would stop you from immediately deleting the more complex one and keeping only the simple one?
  **A:** The two rules might overlap heavily on average but diverge specifically on the rare, high-value cases that are the actual reason both exist — you need to look at the cases where they disagree, not just the overall overlap rate, before concluding the complex one is pure redundancy. It's also worth understanding why the more complex rule was built in the first place (recent history, tickets, or asking whoever wrote it) rather than reasoning about it in isolation from the data alone.
- **Q:** These kinds of rules are sometimes written directly in code — for example, as logical expressions in JavaScript using `&&` and `||`. If you saw a rule written with both of those operators mixed together without parentheses to group them, what would you want to check before assuming you understood exactly what it evaluates to?
  **A:** In JavaScript (like most languages), `&&` binds more tightly than `||`, so without parentheses the actual grouping might not match an intuitive left-to-right reading. The right instinct isn't necessarily already knowing that precedence rule by heart — it's recognizing you shouldn't assume the grouping and should check the language's operator precedence, add explicit parentheses, or just test the expression directly before relying on your read of it.

### 5.2 Python Reasoning (not ML)

- **Q:** You're looking at a Python function that loops through a list of detection rules, and for each rule, loops through every event in a dataset to check whether it matches. The dataset has millions of events and there are hundreds of rules. What's the performance concern, and how would you think about addressing it?
  **A:** That's a nested loop — roughly rules × events — so with millions of events and hundreds of rules, potentially hundreds of millions of individual checks, which will be slow. Look for a candidate who reasons toward batch/vectorized evaluation: computing each rule's condition as a single vectorized operation across the whole dataset at once (e.g., using a dataframe library) instead of checking row by row in a Python loop, and/or pre-filtering on the specific signals most rules depend on so most events can be cheaply ruled out before the full rule logic runs on them.
- **Q:** If you inherited a Python codebase with no tests and needed to understand what a specific function actually does before proposing a change to it, what would you actually do, beyond reading the code from top to bottom?
  **A:** Look for concrete investigative habits: tracing where the function is actually called from to see how it's used in practice, running it against known inputs to observe its real behavior, checking version history or related tickets for why it was written that way, and writing tests that capture its current behavior (even if that behavior isn't obviously "correct") before changing anything, so any unintended change is actually detectable. Reading the code top to bottom alone is a weak answer on its own.

### 5.3 SQL on Large, Complex Datasets

- **Q:** You need to compare the output of two detection rules against a very large table of events — tens of millions of rows per day. What would you think about when writing this comparison query, to avoid it running too slowly?
  **A:** Look for: filtering or aggregating early rather than joining full, unfiltered tables; using an indexed or partitioned column (like a date) to avoid scanning the whole table; preferring set-based operations over anything that processes row by row; and being deliberate about join size and type (avoiding an accidental cross join). A strong answer also mentions checking a query execution plan to see where time is actually being spent, instead of guessing.
- **Q:** How would you write a query to find, among all events flagged by either of two different rules, which ones were flagged by both?
  **A:** A join on a shared event identifier between the two rules' flagged-event sets (e.g., an inner join between two subqueries, one per rule, on the event ID), or SQL's `INTERSECT` if both are written as `SELECT` queries returning that same identifier column.

### 5.4 Data-Flow & Root-Cause Investigation

- **Q:** You're told a specific detection rule has been producing more false positives lately, but nobody knows why, and you don't yet know how the data feeding this rule is produced. How would you start investigating?
  **A:** Look for a structured approach: first pin down what "false positive" actually means here and get specific examples; trace the signals the rule depends on back to where they're actually computed and on what schedule; check whether anything changed recently upstream of the rule (a deploy, a schema change, a shift in event volume or distribution); compare the signal's values before and after the point the problem started; and only then form a specific, testable hypothesis — rather than guessing at a fix right away. This should read as a genuine "trace it upstream" process, not a vague "I'd look into it."
- **Q:** Say two systems both produce a count for what's supposed to be the same kind of event, but the numbers never quite match. What are some concrete reasons that could legitimately happen, even if both systems are working correctly?
  **A:** Look for real reasoning, such as: different time windows or timezone handling between the two systems; different definitions of what counts as the same event (deduplication logic differing between them); one system counting at the moment an event arrives and the other counting once processing finishes (causing a lag); or filtering differences, where one excludes a category of events the other includes. Not expecting an exhaustive list — the signal is recognizing a numeric mismatch doesn't automatically mean a bug, and knowing how to reason about where two definitions might diverge.

### 5.5 Hypothesis-Driven Analysis & Stakeholder Communication

- **Q:** Say you've found that a specific detection rule almost never catches anything on its own — most of what it flags is already caught by other rules. How would you decide whether it's actually safe to remove it, and how would you explain that decision to someone without a technical background?
  **A:** On the investigation side: validate the finding over a long enough time window, in case the rule exists for rare or seasonal cases a short sample would miss; check whether removing it would hurt the system's ability to catch something genuinely new that it might be uniquely suited for; and ideally propose a safe rollout — like monitoring the rule without it actually blocking anything for a while, instead of deleting it outright — to catch anything the retrospective analysis missed. On the communication side: look for an answer that drops the internal jargon and leads with outcome and risk — something like "removing this won't reduce what we catch, based on months of data, and here's the safety net if we're wrong" — rather than reciting the technical investigation steps verbatim to a non-technical audience.
- **Q:** You suspect a detection rule might be too strict and is flagging legitimate activity as suspicious. How would you go about confirming or disproving that, using data?
  **A:** Look for real hypothesis-testing structure: define concretely what "legitimate activity getting flagged" would actually look like in the data, pull a sample of what the rule flagged, and look for evidence that those specific cases are legitimate — ideally using an independent signal the rule itself doesn't rely on, or a manual/sample review — rather than reasoning about it only in the abstract. A strong answer explicitly distinguishes validating against an independent source of truth from circular reasoning that reuses the same signals the rule itself depends on.

### 5.6 Ambiguity Tolerance & Ownership (behavioral — no single correct answer; evaluate against criteria)

- **Q:** Tell me about a time you had to investigate a complex system you didn't build yourself, with little or no documentation, in order to make a change to it. How did you approach it, and what did you do when you got stuck?
  **A (criteria):** Look for a concrete, specific example (not a generic description), a real investigative method beyond "I read the code" (tracing usage, testing behavior, checking history), and evidence of resolving blockers independently — reaching out to the right person or inferring behavior from logs/tests/data, rather than passively waiting for an explanation.
- **Q:** Tell me about a time your priorities changed suddenly while you were in the middle of something. What did you do?
  **A (criteria):** Look for a real example of re-scoping effectively, awareness of what got dropped or delayed and why that was the right call, and evidence of staying effective through the change rather than being derailed by it.
- **Q:** Describe a time you had to explain a technical finding to someone without a technical background. What did you do to make it land?
  **A (criteria):** Look for genuine translation — dropping jargon and framing around business/operational impact — rather than a simplified explanation that's still fundamentally technical. Bonus if they mention checking the other person's understanding and adjusting based on their reaction.

---

## 6. Segment 4 — Task (20–25 min): pick ONE

### Option A — Rule Redundancy & Simplification Exercise

Present the rules below on a shared doc. They use placeholder signal names on purpose — the candidate doesn't need (or get) any domain context about what the signals measure; this is a pure logic/engineering reasoning exercise, mirroring the actual first project.

> "Here are five detection rules currently running in production. Each refers to a handful of true/false signals computed elsewhere in the system — treat them as plain inputs, you don't need to know what they measure. Walk through them with me: what do you notice, what would you simplify, and what (if anything) would you want to check with data before changing or removing any of them?"

```
Rule 1: flag if signal_A AND signal_B
Rule 2: flag if signal_A AND signal_B AND signal_C
Rule 3: flag if (signal_A AND signal_B) OR (signal_A AND NOT signal_B)
Rule 4: flag if signal_D AND NOT signal_D
Rule 5: flag if signal_E OR signal_F
```

**Expected reasoning (interviewer-only answer key):**
- **Rule 3** simplifies, by basic Boolean algebra, to just `signal_A` — `(A AND B) OR (A AND NOT B)` is `A AND (B OR NOT B)`, and `B OR NOT B` is always true, so the whole expression reduces to `A`. This is pure logical simplification; no data needed.
- Once Rule 3 is recognized as just `signal_A`, **Rule 1** (`A AND B`) is a strict logical subset of it — anything that satisfies `A AND B` also satisfies `A` — so Rule 1 can never fire without Rule 3 also firing. That makes Rule 1 provably redundant given Rule 3, purely from logic, no data required.
- **Rule 2** (`A AND B AND C`) is, by the same reasoning, a strict subset of Rule 1, and therefore also a subset of Rule 3. So Rules 1 and 2 both collapse into Rule 3 once it's simplified — the strongest candidates notice this full chain, not just the first simplification.
- **Rule 4** (`D AND NOT D`) is a logical contradiction — always false. This rule can never fire at all; it isn't just redundant, it's dead code that should be flagged outright.
- **Rule 5** (`E OR F`) cannot be resolved by logic alone — `E` and `F` are independent signals with no logical relationship to the others shown. This is the one rule in the set where the right move is proposing a data-driven check: pulling the actual flagged-event sets for Rule 5 and the others, and measuring real-world overlap over a representative time window — the same approach as the first Q&A question in §5.1.

**Above bar vs. at bar vs. below bar:**
- **Above bar:** Simplifies Rule 3 unprompted, recognizes that Rules 1 and 2 both collapse into subsets of Rule 3 (the full chain, not just one relationship), flags Rule 4 as dead/always-false, and for Rule 5 proposes a concrete overlap-measurement approach without being prompted.
- **At bar:** Gets the Boolean simplification of Rule 3 and identifies at least one subset relationship or flags Rule 4, with light prompting, and lands on a reasonable (if generic) data-driven approach for Rule 5 after a nudge.
- **Below bar:** Treats all five rules as unrelated even after a nudge (e.g., "does Rule 1 ever fire without Rule 3 also firing?"), or proposes only a vague "look at the data" for Rule 5 with no concrete comparison method.

### Option B — Live Root-Cause Investigation

A deeper, interactive version of the §5.4 questions. Let the candidate drive by asking you questions; answer as someone on the team with access to the relevant systems, improvising reasonable answers consistent with the scenario.

> "A detection rule that normally flags about 2% of daily events suddenly started flagging 15% of events three days ago, and as far as anyone knows, nobody changed the rule itself. Investigate this with me — ask me whatever questions you'd actually ask, and I'll answer as if I were someone on the team."

**Facilitation guide — track U (unprompted) / P (prompted) / M (missing) per topic:**

| Topic | Nudge if it doesn't come up |
|---|---|
| Clarifies what did or didn't change around that time (code, config, deploys) | "Has anything been deployed or changed around when this started?" |
| Identifies which specific signals the rule actually depends on | "What signals does this rule actually use?" |
| Checks whether the signal computation itself changed upstream, not just the rule | "Where does that signal actually get computed, and by what?" |
| Considers whether the underlying event population changed (traffic mix/volume shift) rather than the rule behaving differently on the same population | "Could the makeup of the traffic itself have changed, rather than the rule's behavior?" |
| Segments the spike (by time, source, or another dimension) instead of treating it as uniform | "Is the increase spread evenly, or concentrated somewhere specific?" |
| Forms a specific, testable hypothesis rather than guessing | "What would you expect to see in the data if your theory is right?" |
| Considers immediate downstream impact/urgency | "Would you do anything right away, before finishing the investigation?" |

**Above bar vs. at bar vs. below bar:**
- **Above bar:** Drives the investigation like a real incident — narrows from symptom to a specific, testable hypothesis by asking targeted questions, independently considers both "something changed in the rule's inputs" and "the population itself changed," and raises the question of immediate mitigation vs. root-causing fully before acting.
- **At bar:** Asks reasonable clarifying questions and reaches a plausible hypothesis, but needs 1–2 nudges to get to segmentation or to separate "the rule changed" from "the population changed."
- **Below bar:** Jumps to a guess without asking clarifying questions, or can't form a specific, testable hypothesis even after prompting.

---

## 7. Segment 5 — Candidate Questions

No prep needed. Note whether questions focus on the actual technical/systems challenges vs. purely logistics/compensation.

---

## 8. Scorecard

Score 1 (below bar) – 4 (exceptional) per row. This is the internal technical scorecard used to reach a hire decision — separate from `candidates/review_template.md`, which is the client-facing write-up format.

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
| Snowflake / domain exposure (cybersecurity, fraud, rule-based systems) | Low | | |

**Guidance on weighting:** rule-logic reasoning, root-cause investigation, hypothesis-driven analysis, non-technical communication, and independent ownership are the actual substance of this specific role and the hardest to coach — a candidate can pick up a specific SQL dialect or a new codebase's conventions on the job, but not easily the instinct to trace a problem to its root cause or to work independently through ambiguity. Python and SQL are foundational gates (explicitly required), but secondary to the reasoning/investigation skills themselves. JavaScript is only asked as "familiarity," not fluency — weight accordingly. Snowflake and direct cybersecurity/fraud-detection domain exposure are explicitly named as a plus, not a requirement.

---

## 9. Decision guidance

- **Strong hire:** at or above bar on all High-weight rows; demonstrates hypothesis-driven, independent investigation unprompted (in Q&A or the Segment 4 task); no more than one Medium-weight row below bar.
- **Borderline:** strong on rule-logic/engineering reasoning but weak on data-driven investigation or stakeholder communication, or vice versa; or has the raw skills but limited concrete evidence of genuinely independent, self-directed work in ambiguous situations (e.g., mostly describes managing others' analysis rather than driving their own) — worth a focused follow-up on the specific gap rather than an outright pass.
- **No hire:** below bar on rule-logic reasoning or root-cause investigation (the two things this role is fundamentally about), or shows no real instinct toward hypothesis-driven, data-backed reasoning even after multiple nudges in Segment 4 — both are central and not easily developed on the job for a senior-level hire on a newly forming team with no established process to lean on.

---

## 10. Notes for future rounds

This plan has not yet been run against a real candidate — recalibrate question difficulty and the task menu after the first few interviews, and note here if the client shares their own example exercise so it can be incorporated as a higher-confidence reference point.