# Interview Plan — Senior Data Scientist, Analytics/Insights track, non-ML (REF4184W)

**Role:** Senior Data Scientist, per `job_description.md` and `raw_role_brief.md`. Fully remote/hybrid. Client is a global cybersecurity company; this specific team works on a large-scale analytics and detection system within the client's Enterprise product line. **This is explicitly not a machine-learning role.** The first project is analyzing an existing set of detection logic — complex rule-based expressions built on multiple data signals — to understand its structure, find redundancy and unnecessary complexity, and propose ways to simplify and consolidate it without losing detection effectiveness. The product itself does bot/threat detection, but this project is not about analyzing bots or threats directly — it's about the data-engineering and analytical problem of simplifying the detection system itself.

**Revision note (v2 — supersedes `interview_plan_old.md`, kept for reference):** this version was rewritten after client feedback on the first candidate run through the old plan, Daniel Fogaça (`candidates/daniel_fogaca/clients_feedback.md`). He scored well against the old plan's Q&A bank and exercise, but was rejected by the client's own hiring-manager interview for two specific reasons: he didn't show he could work in a complex environment with vague success criteria, and he struggled to go 2–3 layers deep into a real data issue when pushed. Tellingly, the client's own interview consisted of having him walk through one real past project (his Serasa work) and drilling into specific implementation details — e.g. exactly which data transformation techniques he used to create columns in a table — not abstract detection-rule or cybersecurity topics. The old plan leaned on abstract Q&A and a toy exercise with placeholder rules, and tested ambiguity tolerance with a hypothetical scenario rather than a real example — structurally easy to give smooth, rehearsed-sounding answers to without ever being pushed for genuine depth. This version restructures the interview around a mandatory **Project Deep-Dive** that does what the client's own interview did; the old Q&A bank and exercise survive but are demoted to secondary, time-permitting content.

---

## 1. What this interview is screening for

| Area | Why it matters here | JD/brief signal |
|---|---|---|
| **Depth of real technical ownership under follow-up** | The client's actual, stated bar — a candidate who can only summarize past work at a resume-bullet level, even when pushed, is a hard stop regardless of how the rest of the interview goes | Directly from client feedback on the previous candidate, not just the written JD |
| Ambiguity tolerance & independent ownership, evidenced from a real example | Must come from an actual past situation where success criteria weren't clearly defined, not a hypothetical scenario — the previous candidate's hypothetical answers didn't convince the client | "High seniority and a strong track record of working independently"; "Ability to work autonomously with minimal supervision" |
| Data-flow / root-cause investigation | The actual day-to-day work: tracing how signals are gathered and processed, and why | "Investigate underlying data flows: how signals are gathered, processed, and used, and why they were designed that way" |
| Hypothesis-driven analysis | Explicitly named as the operating method, not just general analytics | "Form hypotheses about inefficiencies or redundancies and validate them through data-driven analysis" |
| Communication to non-technical stakeholders | Explicitly required, separate from the technical work itself | "Translate technical findings into clear, actionable recommendations for both technical and non-technical stakeholders" |
| Python & SQL on large, complex datasets | Explicitly required, hard requirement regardless of project focus | "Proficient in Python and SQL, with experience working across large, complex datasets" |
| Rule-logic & redundancy reasoning | Still a named, core responsibility, but the client's own interview put little weight on this directly — treat as secondary signal, not the main event | "Analyze existing detection logic... to understand current structure, complexity, and redundancy" |
| JavaScript / Snowflake / domain exposure | Named requirements/pluses, but lowest priority — the client's own interview didn't probe these at all for the previous candidate | "Familiarity with JavaScript"; "Familiarity with Snowflake is a nice to have" |

---

## 2. Timeline (rebalanced — Deep-Dive is mandatory and gets priority on time)

| # | Duration | Segment |
|---|---|---|
| 1 | 5 min | Quick candidate intro + role context |
| 2 | 30–35 min | **Project Deep-Dive (mandatory, primary signal)** |
| 3 | 10–15 min | Technical Q&A (trimmed, time-permitting) |
| 4 | 10–15 min | Task (time-permitting, lowest priority) |
| 5 | 5 min | Candidate's questions |

Total: ~60–75 minutes. **If time runs short, cut in this order: Segment 4 first, then trim Segment 3 further. Never shorten Segment 2.**

---

## 3. Segment 1 — Quick Introduction + Role Context (5 min)

A brief "tell me about your background" is enough here — don't let this run long, since real background detail should surface naturally during the Deep-Dive instead of a separate extended intro.

Then read/paraphrase this role context to the candidate:

> "Our customer is a cybersecurity company. Part of their product works by flagging suspicious activity using a large set of rules — each rule combines several different pieces of measurable data, which we call signals, using logical conditions, to decide whether something should be flagged. Over time, a lot of these rules have accumulated, and it's not clear anymore which ones are still pulling their weight versus doing the same job as another rule, or combining signals in an overly complicated way that nobody has revisited in a while. To be clear, this specific project is not about analyzing bots or security threats themselves — it's about understanding this rule system and the data flowing through it well enough to simplify it: finding redundant or overly complex rules, tracing how the underlying data is gathered and processed, and proposing changes that make the system simpler without making it worse at catching what it's supposed to catch. You'd need to figure out the downstream impact of any change before it ships, and explain your reasoning clearly to both engineers and people without a technical background. This is a newly forming team, so you'd be working independently much of the time, pulling in engineers, other data scientists, and security researchers for context rather than waiting for things to be handed to you."

**Keep it at this level — do not go further.** Don't describe specific investigative techniques — that's for the candidate to demonstrate in Segment 2/4.

---

## 4. Segment 2 — Project Deep-Dive (30–35 min): the primary signal

**Opening prompt to give the candidate:**

> "Pick one real project from your own work where you had to dig into a data problem — something that wasn't obvious at first, where you had to investigate, make a judgment call, and see it through. Walk me through what actually happened."

**Why this segment exists:** this is a direct replica of what the client's own hiring-manager interview actually did to the previous candidate, and it's the reason he was rejected despite a strong overall impression in our interview. A fluent, confident, well-organized answer is not sufficient on its own — the client's bar is whether the candidate can sustain real, concrete depth when pushed.

**The follow-up ladder — apply this to every Deep-Dive, not just once:**

1. **Layer 1 — What exactly did you do?** Push past a summary. "What exact technique or tool did you use for that, specifically?" "Walk me through the actual steps, not just the outcome." If they mention a transformation or fix, ask precisely what it was (e.g., which fields, what calculation, what tool).
2. **Layer 2 — Why that way?** "Why that approach specifically, and not something else?" "What else did you consider, and why didn't you go with it?" Listen for real trade-off reasoning worked out at the time, not a justification reconstructed after the fact.
3. **Layer 3 — How did you know it worked?** "How did you know this was actually the right call?" "What would you have done if it hadn't worked?" Specifically ask: "Was there a clear definition of success agreed upfront, or did you have to figure out what success even meant yourself? How did you do that?" — this is the direct, real-example test for working with vague success criteria, replacing the old plan's hypothetical scenario question.

**Interviewer instruction — do not skip this:** if the candidate cannot sustain specific, concrete detail through at least two of the three layers above, write that down plainly as a real signal — don't let a generally pleasant, articulate conversation talk you out of noting it. This exact gap (good surface answers, no real depth under follow-up) is what cost the previous candidate the offer after we'd already rated him a strong hire.

**What a strong answer looks like:** specific tool/technique names tied to a specific step (not generic "I used Python" but "I wrote a script that compared X against Y to find Z"), a real trade-off they can explain in their own words, and a concrete answer for how they knew it worked — ideally a measurable outcome or a specific validation step, not just "it worked out" or "the team was happy."

**What a weak answer looks like:** stays at resume-bullet level even after follow-up ("I improved the pipeline," "I found the issue and fixed it") without ever naming the actual mechanism; can't explain why they chose their approach over an alternative; or has no real answer for how they knew they'd succeeded.

If the candidate's first project choice runs dry quickly, it's fine to ask for a second one — but note in the write-up that it took multiple attempts to find real depth, since that's itself part of the signal.

---

## 5. Segment 3 — Technical Q&A (10–15 min, time-permitting)

The client's own interview put little weight on detection-rule/cybersecurity-specific prep and focused far more on real project depth (now covered in Segment 2). Treat this as a secondary check — **pick only 3–4 questions total**, prioritizing Python/SQL since those are explicit hard requirements regardless of project focus.

### 5.1 Python Reasoning
- **Q:** You're looking at a Python function that loops through a list of detection rules, and for each rule, loops through every event in a dataset to check whether it matches. The dataset has millions of events and there are hundreds of rules. What's the performance concern, and how would you address it?
  **A:** That's a nested loop — roughly rules × events — so with millions of events and hundreds of rules, potentially hundreds of millions of individual checks, which will be slow. Look for a candidate who reasons toward batch/vectorized evaluation: computing each rule's condition as a single vectorized operation across the whole dataset at once (e.g., using a dataframe library) instead of checking row by row in a Python loop, and/or pre-filtering on the specific signals most rules depend on so most events can be cheaply ruled out before the full rule logic runs on them.
- **Q:** If you inherited a Python codebase with no tests and needed to understand what a specific function actually does before proposing a change to it, what would you actually do, beyond reading the code from top to bottom?
  **A:** Look for concrete investigative habits: tracing where the function is actually called from to see how it's used in practice, running it against known inputs to observe its real behavior, checking version history or related tickets for why it was written that way, and writing tests that capture its current behavior before changing anything, so any unintended change is actually detectable.

### 5.2 SQL on Large, Complex Datasets
- **Q:** You have two queries, each returning the set of event IDs flagged by one detection rule. You need to measure the overlap between them — how many events both rules flagged, and how many only one of them flagged — against a table with tens of millions of rows per day. What would you think about when writing this, to avoid it running too slowly?
  **A:** Look for: filtering or aggregating early rather than joining full, unfiltered tables; using an indexed or partitioned column (like a date) to avoid scanning the whole table; doing the comparison as a single query (a join or similar) rather than pulling both lists of IDs out and comparing them one by one in code; and being deliberate about join size and type. A strong answer also mentions checking a query execution plan to see where time is actually being spent.

### 5.3 Rule-Logic Reasoning
- **Q:** You're reading someone else's code and find an `if` statement with a long condition — several `and`/`or` checks chained together, no parentheses, not nicely formatted. What would you do to make sure you actually understand what it evaluates to before relying on it?
  **A:** The key instinct: don't trust a quick left-to-right read. In most languages (including Python), `and` binds more tightly than `or`, so without parentheses the real grouping can differ from an intuitive reading. A strong answer rewrites the expression with explicit parentheses to make the grouping unambiguous, and/or tests it directly against a few concrete input combinations, rather than assuming the first read is correct. This isn't about already knowing any language's precedence rules by heart — the signal is recognizing you shouldn't assume and should verify.

### 5.4 Data-Flow & Root-Cause Investigation
- **Q:** You're told a specific detection rule has been producing more false positives lately, but nobody knows why, and you don't yet know how the data feeding this rule is produced. How would you start investigating?
  **A:** Look for a structured approach: first pin down what "false positive" actually means here with specific examples; trace the signals the rule depends on back to where they're actually computed; check whether anything changed recently upstream; compare before and after the problem started; and only then form a specific, testable hypothesis.

---

## 6. Segment 4 — Task (10–15 min, time-permitting, lowest priority)

Only use this if Segments 2–3 leave time. Present one option, not both.

### Option A — Rule Redundancy & Simplification Exercise

> "Here are five detection rules currently running in production. Each refers to a handful of true/false signals computed elsewhere in the system — treat them as plain inputs, you don't need to know what they measure. Walk through them with me: what do you notice, what would you simplify, and what (if anything) would you want to check with data before changing or removing any of them?"

```
Rule 1: flag if signal_A AND signal_B
Rule 2: flag if signal_A AND signal_B AND signal_C
Rule 3: flag if (signal_A AND signal_B) OR (signal_A AND NOT signal_B)
Rule 4: flag if signal_D AND NOT signal_D
Rule 5: flag if signal_E OR signal_F
```

**Expected reasoning:**
- **Rule 3** simplifies, by basic Boolean algebra, to just `signal_A` — `(A AND B) OR (A AND NOT B)` is `A AND (B OR NOT B)`, and `B OR NOT B` is always true, so the whole expression reduces to `A`.
- Once Rule 3 is recognized as `signal_A`, **Rule 1** (`A AND B`) is a strict subset of it, and **Rule 2** (`A AND B AND C`) is a subset of Rule 1 — the strongest candidates notice this full chain, not just the first simplification.
- **Rule 4** (`D AND NOT D`) is a logical contradiction — always false, dead code.
- **Rule 5** (`E OR F`) can't be resolved by logic alone — the right move is proposing a data-driven overlap check against the others.

**Above bar vs. at bar vs. below bar:**
- **Above bar:** Simplifies Rule 3 unprompted, recognizes the full Rule 1/2/3 chain, flags Rule 4 as dead, and proposes a concrete overlap-measurement approach for Rule 5 without prompting.
- **At bar:** Gets the Boolean simplification and at least one subset relationship or Rule 4 with light prompting, lands on a reasonable data-driven approach for Rule 5 after a nudge.
- **Below bar:** Treats all five rules as unrelated even after a nudge, or proposes only a vague "look at the data" for Rule 5.

### Option B — Live Root-Cause Investigation

> "A detection rule that normally flags about 2% of daily events suddenly started flagging 15% of events three days ago, and as far as anyone knows, nobody changed the rule itself. Investigate this with me — ask me whatever questions you'd actually ask, and I'll answer as if I were someone on the team."

Let the candidate drive by asking questions; improvise reasonable answers. Look for: clarifying what changed, tracing the rule's specific signals, checking whether the signal computation changed upstream vs. the underlying event population changing, segmenting the spike instead of treating it as uniform, forming a specific testable hypothesis, and considering immediate mitigation vs. full root-causing.

---

## 7. Segment 5 — Candidate Questions (5 min)

No prep needed. Note whether questions focus on the actual technical/systems challenges vs. purely logistics/compensation.

---

## 8. Scorecard

Score 1 (below bar) – 4 (exceptional) per row. This is the internal technical scorecard used to reach a hire decision — separate from `candidates/review_template.md`, which is the client-facing write-up format.

| Dimension | Weight | Score (1–4) | Notes |
|---|---|---|---|
| Depth of real technical ownership under follow-up | High | | The single most important new signal — can the candidate sustain concrete detail through at least 2 of 3 follow-up layers into their own real past work, not just give a polished summary. |
| Ambiguity tolerance & independent ownership | High | | Must come from the real Deep-Dive example (did they have to define success themselves under vague criteria) — not a hypothetical scenario answer. |
| Data-flow / root-cause investigation | High | | |
| Hypothesis-driven analysis | High | | |
| Communication to non-technical stakeholders | High | | |
| Python reasoning | Medium | | |
| SQL on large, complex datasets | Medium | | |
| Rule-logic & redundancy reasoning | Medium | | Demoted from High in v1 — the client's own interview put little weight on this directly. |
| JavaScript familiarity | Low | | |
| Snowflake / domain exposure | Low | | |

**Guidance on weighting:** the Deep-Dive's follow-up depth and the real-example evidence of ambiguity tolerance are now the two highest-priority rows, directly because of client feedback — not because the underlying rule-logic/data-investigation content stopped mattering, but because a candidate who can't survive follow-up on their own real work is a hard stop regardless of how well they do elsewhere, and our previous plan didn't catch that.

---

## 9. Decision guidance

- **Strong hire:** at or above bar on all High-weight rows; specifically, sustains concrete, specific depth through at least 2 of the 3 Deep-Dive follow-up layers, with a real, non-generic answer for how they defined or recognized success under ambiguous criteria; no more than one Medium-weight row below bar.
- **Borderline:** reasonable Deep-Dive detail but needs real prompting to get past a first-pass summary, or needed a second project attempt to find genuine depth; otherwise strong elsewhere — worth a focused follow-up specifically re-testing depth on a different real example rather than an outright pass.
- **No hire:** below bar on data-flow/root-cause investigation or hypothesis-driven analysis; **or — the explicit, client-validated trigger — cannot sustain concrete depth beyond a resume-bullet-level summary of their own real past work even after follow-up, regardless of how strong or pleasant the rest of the interview was.** This is exactly the failure mode that cost the previous candidate the offer, and it must be treated as a hard stop here rather than softened by a generally good overall impression.

---

## 10. Notes for future rounds

This is v2, rewritten after one real piece of client feedback (Daniel Fogaça, rejected at the client's hiring-manager stage despite a strong internal review — see `candidates/daniel_fogaca/clients_feedback.md`). Keep iterating this plan as more client feedback comes in from subsequent candidates; `interview_plan_old.md` is kept as the pre-feedback baseline for reference, not as an alternate current option.
