# Interview Notes — Gabriel Kirst da Rocha

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md` (v2 — Project Deep-Dive is the primary segment, follow the 3-layer follow-up ladder). When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Résumé Summary (pre-filled from `cv.md` before the interview)

- Senior data professional, 15+ yrs in tech (5 in data), first hire on the data team at KTO Group (online betting platform), promoted twice in 3 years: Data Analyst (Aug 2021) → Analytics Engineer (Aug 2023) → Data Lead (Sep 2024, current). Leads a 4-person team but states he stays hands-on in SQL/PySpark.
- Strongest domain fit seen for this role so far: current role implements automated data quality checks that flag non-conformities and auto-correct where possible — structurally close to this role's actual detection-rule-simplification work, and it's current, not years old. Also built the latency reduction that specifically "enables fraud, abuse, and platform-issue alerts," and was the data focal point for a federal regulatory audit/compliance integration (SIGAP).
- Best deep-dive anchors, all fresh (last ~2 years): the automated non-conformity detection/correction system (strongest pick), the change-data-capture platform scaling 1M→50M transactions/day, or the regulatory audit/compliance work.
- No JavaScript anywhere in the CV, no Snowflake (he's Redshift-specific — lean on domain exposure for that scorecard row instead). States daily use of Claude (Anthropic) for SQL/code/documentation — plausible, not a red flag, could make an easy, low-stakes opener about his actual workflow.
- See `cv.md` for full detail and the complete "Notable for this role" gap analysis.

## Segment 1 — Quick Introduction + Role Context (5 min)

> "Our customer is a cybersecurity company. Part of their product works by flagging suspicious activity using a large set of rules — each rule combines several different pieces of measurable data, which we call signals, using logical conditions, to decide whether something should be flagged. Over time, a lot of these rules have accumulated, and it's not clear anymore which ones are still pulling their weight versus doing the same job as another rule, or combining signals in an overly complicated way that nobody has revisited in a while. To be clear, this specific project is not about analyzing bots or security threats themselves — it's about understanding this rule system and the data flowing through it well enough to simplify it: finding redundant or overly complex rules, tracing how the underlying data is gathered and processed, and proposing changes that make the system simpler without making it worse at catching what it's supposed to catch. You'd need to figure out the downstream impact of any change before it ships, and explain your reasoning clearly to both engineers and people without a technical background. This is a newly forming team, so you'd be working independently much of the time, pulling in engineers, other data scientists, and security researchers for context rather than waiting for things to be handed to you."

Notes (background, story):

- Working with tech for 16 years
- Last 5 years at KTO
- First Data Engineer hired, became Data Lead in a few years
- 

## Segment 2 — Project Deep-Dive (30–35 min): the primary signal

Project the candidate chose:

- The candidate choose a project about Data Quality monitoring.
- That project was compsoed of a few pilars of quality:
  - Deduplication
  - Data Delivery Lag
  - Database Schema Changes
  - Third Party Integration Quality Checks
- Each pillar was built on demand over the time, wasnt a closed project developed at once
- During the interview, he went through each pilar and explained to me how it was implemented, how it was tested, how the
problem arrived.
- His english was very easy to understand. I little bit of accent, but it was cristal clear. He could speak clearly
and explain his point with confidence.
- Now on each of the pilars:
  - Deduplication: 
    - He began as the single Data Engineer of KTO. At the beggining, the company was very small, no data warehouse,
    stuff running on excel sheets, poor infrastructure. So it was difficult to guarantee quality, because there was no visibility of anything
    - At some point, BI team came to him complaining that the reports weren't matching.
    - At that point, he began investigating the problem and after running a bunch of queries, he identified that the problem was that some keys used in joins were duplicated. Data was at Redshift and he found out at that point that a primary key on a table from redshif does not guarantee uniqueness on its own. That must be enforced.
    - There for be able to guarantee that, he built a process that read a config file with the name of the tables to be monitored, the monitoring frequency and all configurations required to defined what should be monitored. A process would read that file and run queries across redshift to verify for duplicates and the actually fix the problem. The process was relatively light because it would run on a window, not for the whole database, fixing the problem over the time, not all at once.
    - Testing the solution was mostly simple, consisted in checking results on several reports and dashboards guaranteeting that consistent.
  - Data Delivery Lag
    - As the company grew in size and complexity, the Bet industry in Brazil was also changing. New regulations required the betting companies to provide information for the goverment in the same way Banks do, for regulation, that is all betters, all transactions and all that, for auditing. 
    - At that point, data wasn't anymore used for Reporting and Dashboards anymore, when the impact was relevant, but constrained withing the company, now data was part of the operation, as per the regulations
    - Therefore, if the data KTO should send to the goverment ended up not being set at all, that could cause fines and serious consquences to the business.
    - The cause of the problem: KTO used a third-party for the sport book service, that is, who won, who lost, what games are happening, this kind of stuff.
    - That was utterly important for the whole operation. Basically, the input from that third-party was consumed over an API and saved to redshift. But after analysing the problem, Gabriel noticed that the data from the sports book service was lagging at some points. Lets say info from that provider should arrive constatly at every 10 minutes, but at some points the lag between two new batches of data would go much higher, up to hours, and nobody would notice that. That would propagate downstream and cause several problems.
    - The solution was basically a set of queries that would run periodically on the whole data warehouse verifying the lag on the table, that is, how old was the last insert/update from that table. That monitor would then notify if it noticed the lag was longer that allowed for that table.
    - That improved the data quality and fixed severald downstream problems in the Data Warehouse.
    - To test the solution, he ran the process in PRD (The company did not have a DEV environment back then) but with very short lag constrains. Lets say a table was being updated every 10 minutes when healthy. Then, he configured for that table a constrain of 5 minutes (smaller than it should) so to force the sistem to trigger so that he could make sure the process was working fine.
  - Data Schema Changes:
    - The engineering team would often change table structures. Maybe altering a column that had non-null constrains to not have it.
    - The engineering team would do that without noticing anyone. As at the beggining data was used for report only, that wasn't a big problem
    - But as data became part of the operation and integration with Government, these issues became more serious.
    - So after catching those problems several times, he decided to talk with the engineering team about of the consequences of those changes.
    - That is, explaining for the engineering team that now that data was part of the operation, such issues would have much higher consequences to the company, so they needed to agree on a process to avoid that such problems happen.
    - He proposed a solution where an Agent would watch every PR from the engineering team trying to flag changes on table strucutre.
    - If the agent identified some relevant changes, would then notify the Data team about that so they could anticipate the changes and avoid problems.
    - To test it, they had to wait for such an event to happen, because they did not have a Dev enviroment to test that and did not make any change to force a change like that in PRD just to test the idea.
  - Thridy Party Integration Checks
    - KTO would send data to a third-party that is relevant to the operation
    - However, they began to notice problems on that integrations. At KTO warehouse we had X, but at the client we had Y.
    - What happens is that the queries that ran between Silver and Golden layers on Redshift were basically scheduled queries.
    - Because of that, the golder layer query could run before the relevant tables were ready in the silver layer.
    - So at first, they implemented 2 checks:
      - One before sending data to the third-party, to verify that wathever they were sending was correct
      - A second one were they would fetch data from that third-party and compare with that existing inside KTO's data warehouse and make sure they match
    - Then, after that, they migrated those queries from scheduled queries on Redshift to airflow triggered jobs, in a way that the lag between layers would not have an affect anymore
    - After this, the integration problem seized, but they kept the data quality checks as a safety measure

- Overall, Gabriel was able to provide deep details on hwo things were implemented, with confidence and clear ownership of things
- His english was clear to understand and he demonstrade hability to navigate difficult enviroments, show ownership and handle the solution of problems on his own. 

**Layer 1 — What exactly did you do?** (specific technique/tool/steps, not a summary)

Candidate's answer:

-

**Layer 2 — Why that way?** (trade-offs considered, alternatives ruled out)

Candidate's answer:

-

**Layer 3 — How did you know it worked?** (incl. direct probe: was success defined upfront, or did they have to figure that out themselves — the real-example test for ambiguity tolerance)

Candidate's answer:

-

**Did the candidate sustain concrete depth through at least 2 of these 3 layers, or did answers stay at resume-bullet level even after follow-up?**

[X] Sustained real depth (2–3 layers)   [ ] Partial — needed heavy prompting   [ ] Did not sustain depth

Notes:

-

Needed a second project to find real depth? [X] No   [ ] Yes — note why:

-

## Segment 3 — Technical Q&A (10–15 min, time-permitting — pick only 3–4 total)

### Python Reasoning

- [ ] **Q:** You're looking at a Python function that loops through a list of detection rules, and for each rule, loops through every event in a dataset to check whether it matches. The dataset has millions of events and there are hundreds of rules. What's the performance concern, and how would you address it?
  > Expected: Nested loop (~rules × events) is slow at this scale; reasons toward vectorized/batch evaluation, and/or pre-filtering on key signals before running full rule logic.
  Candidate's answer:

- [ ] **Q:** If you inherited a Python codebase with no tests and needed to understand what a specific function actually does before changing it, what would you do beyond reading the code top to bottom?
  > Expected: Trace actual call sites, run it against known inputs, check version history/tickets, write characterization tests before changing anything.
  Candidate's answer:

### SQL on Large, Complex Datasets

- [ ] **Q:** You have two queries, each returning the set of event IDs flagged by one detection rule. You need to measure the overlap between them — how many events both rules flagged, and how many only one of them flagged — against a table with tens of millions of rows per day. What would you think about when writing this, to avoid it running too slowly?
  > Expected: Filter/aggregate early, use partitioned/indexed columns, compare the two ID lists in a single query (join/INTERSECT) rather than pulling them out and comparing row by row in code, avoid accidental cross joins, check the query plan.
  Candidate's answer:

### Rule-Logic Reasoning

- [ ] **Q:** You're reading someone else's code and find an `if` statement with a long condition — several `and`/`or` checks chained together, no parentheses, not nicely formatted. What would you do to make sure you actually understand what it evaluates to before relying on it?
  > Expected: Don't trust a quick left-to-right read — `and` binds tighter than `or` in most languages, so grouping can differ from intuition. Rewrite with explicit parentheses and/or test against concrete inputs, don't assume.
  Candidate's answer:

### Data-Flow & Root-Cause Investigation

- [ ] **Q:** You're told a specific detection rule has been producing more false positives lately, but nobody knows why, and you don't yet know how the data feeding it is produced. How would you start investigating?
  > Expected: Pin down what "false positive" means with examples, trace the rule's signals back to where they're computed, check for recent upstream changes, compare before/after, then form a specific testable hypothesis.
  Candidate's answer:

## Segment 4 — Task (10–15 min, time-permitting, lowest priority)

Used? [X] No — ran out of time   [ ] A — Rule Redundancy & Simplification   [ ] B — Live Root-Cause Investigation

Notes on approach:

-

Overall bar level for this segment (if used): [X] Above bar   [ ] At bar   [ ] Below bar

## Segment 5 — Candidate Questions (5 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

-

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Depth of real technical ownership under follow-up | High | 4           | |
| Ambiguity tolerance & independent ownership | High | 4           | |
| Data-flow / root-cause investigation | High | 4           | |
| Hypothesis-driven analysis | High | 3           | |
| Communication to non-technical stakeholders | High | 4           | |
| Python reasoning | Medium | 3           | |
| SQL on large, complex datasets | Medium | 4           | |
| Rule-logic & redundancy reasoning | Medium | 2           | |
| JavaScript familiarity | Low | 1           | |
| Snowflake / domain exposure | Low | 2           | |

## Decision Lean

[X] Strong hire   [ ] Borderline   [ ] No hire

Justification:

-

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-
