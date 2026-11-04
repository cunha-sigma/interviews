# Interview Notes — Mateus Matos Rizzi

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md` (v2 — Project Deep-Dive is the primary segment, follow the 3-layer follow-up ladder). When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Daria's Soft Skill interview
Note: `-` means Darias speaking, `*` means Mateus speaking

- This role is not a traditional Data Science position, is more a hybrid of Data Engineer/Analyst and Data Scientist.
- We need someone that is ready to jump and own the project, understand it and improve it without the necessity of 
applying ML on it, but rather maintain it. 
- This position is a one-man-army, you'll need to find out what to do and also do it.
- Needs to be senior and mature to able to drive that role
- You won't be the solo player, you will have other people to relly on and consult, but you onw the process
- Client is US based, the schedule is flexibly, but you should have at least 2 to 3 hours of overlap with the US time

* That sounds good to me. I have more than 9 years in the area and I have been in all the sub-areas of Data Industry.
* 

- You just joined a team, you are a senior and you bring knowledge. Your manager is pretty much in disagree with your
vision and solutions, although you are sure they are the best options. However, your manager keeps disagreeing with you.
How would you proceed with such situation?
* I would try to understand the scenario and the case and do the best work I can do. If I have space to give ideas
and show the gaps I can see, I show what I have, if they cannot agree, I cannot do anything. Basically, I will do
what the boss tells me to do. 
- Lets say you are working on a project, it needs 1 month to be properply implemented, but your manager comes to you
and says it needs to be done in 1 week. What would you do?
* I would try to show the manager what I can deliver in 1 week only, so we could think about an MVP that can be delivered
on that time.


## Résumé Summary (pre-filled from `cv.md` before the interview)

- Staff Data Engineer, São Paulo (remote), 8+ yrs, currently AI/Data Tech Lead at Coforge (since Feb 2025): architected a layered data lake on Databricks/Azure, owns Snowflake platform (access control, performance, data sharing), built AI-powered text-to-SQL features, mentors 4–6 engineers.
- Prior: AI/Data Tech Lead & Senior Data Engineer at Mercado Eletrônico (4 years — built the company's core dbt+Snowflake platform, led a team of 4–6), Data Engineer at Sysvision (Bayer/Life Sciences, SAP HANA), Data Engineer at ADP (payroll/HR pipelines), earlier Data Analyst/BI Intern roles.
- Best deep-dive anchor: the Mercado Eletrônico production-performance story — found missing primary keys causing full-table scans, redesigned the extraction strategy, cut pipeline duration from 4 hours to 1 hour. Concrete, measurable, and recent enough (within the last ~4 years) to probe for real depth.
- Profile shape is different from prior candidates: data-platform/infrastructure engineering (Snowflake, dbt, orchestration, BI semantic layers) rather than investigative analytics — push on whether his work is really about investigating ambiguous problems in existing data, or more about building/operating pipelines. No rule-based detection/cybersecurity experience, consistent tech-lead/mentorship thread in his two most recent roles — worth checking he wants a hands-on IC role, not a continuation of team-lead work.
- See `cv.md` for full detail and the complete "Notable for this role" gap analysis.

## Segment 1 — Quick Introduction + Role Context (5 min)

> "Our customer is a cybersecurity company. Part of their product works by flagging suspicious activity using a large set of rules — each rule combines several different pieces of measurable data, which we call signals, using logical conditions, to decide whether something should be flagged. Over time, a lot of these rules have accumulated, and it's not clear anymore which ones are still pulling their weight versus doing the same job as another rule, or combining signals in an overly complicated way that nobody has revisited in a while. To be clear, this specific project is not about analyzing bots or security threats themselves — it's about understanding this rule system and the data flowing through it well enough to simplify it: finding redundant or overly complex rules, tracing how the underlying data is gathered and processed, and proposing changes that make the system simpler without making it worse at catching what it's supposed to catch. You'd need to figure out the downstream impact of any change before it ships, and explain your reasoning clearly to both engineers and people without a technical background. This is a newly forming team, so you'd be working independently much of the time, pulling in engineers, other data scientists, and security researchers for context rather than waiting for things to be handed to you."

Notes (background, story):

-

## Segment 2 — Project Deep-Dive (30–35 min): the primary signal

Project the candidate chose:

-

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

[ ] Sustained real depth (2–3 layers)   [X] Partial — needed heavy prompting   [ ] Did not sustain depth

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

- In this format of interview, it is difficult to take clear notes. So I will explain here how it went.
- In the beggining, I did some intro on AdTech to give some context, explained what the client does, what the problem
is and what kind of professional they are looking for.
- I prompted him to choose the shinniest project he had ever worked with (I gave him some heads up on what I was trying to evaluate,
that is, his capability of explaining the process comprehensivelly, with details, and giving more information in a clear way when prompted)
- The interview was conducted in English even though he was brazilian because from the first part with Daria, I felt he did not had
very good english, so I wanted to evaluate how he did go.
- I could he see he really could understand and read englihs, but speaking was difficult, not a lot of practice. So he would misspell common jargons
such as Machine Learning, SAP, and other words, so he knew the word in english, but could not pronuntiate it correctly, so it was difficult to understand.So I had at some points to ask him to repeat, sometimes giving him tips in portuguese. I was able to understand him, but I constantly had to stop him, tell him what I had understood and confirm with him that I was correct. 
- On his project:
  - The company was a market place B2B, so companies could shop large quantities from big sellers and so on.
  - They had an issue, the clients could only formalize their shoppings in the platform if they came from an ERP. So shoppings of smaller
  quantities, that some times were done out of the ERP, had to be manually inserted in the platform
  - For some clients, that was prohibitive, as there were far too many invoices of small shopping to be manually inserted
  - Given that context, at some point the company came up with some goals for his team, among which was the manually inserted invoices problem
  - So they selected to work on that project specifically because the team evaluated the trade offs of the presented goals, each was a different project, and evaluate that this one was the one that would be capable of bringing more value to the company and the data team did not had enough resources to
  support high complexity projects
  - For this manually inserted invoices, the solution proposed was to us Gen AI Agents to do that job.
  - So the project worked like this: the customer would send the invoices to be manually inserted over an e-mail to a specific address.
  - Those e-mails were parsed and saved at S3, so at S3 they had, for each e-mail, the e-mail message, subject and the PDF files for the invoices
  - A Python tool (which he could not remember the name - I would guess something like pdf2text) was used to parse text from the invoices
  - The information needed was basically who bouth what from who and when
  - But how those small shops weren't created over the ERP standards, the descriptions, product codes and so on not always were clear, in order to able to be parsed with heuristics. So they would look for those informations using the Agent.
  - The agent would give a score for each extraction, then a threshold would say if it was a perfect extraction or not.
  - If not, the agent would answer the client back on another e-mail with information on what should be clarified. The client then would need to retry it over another e-mail, no e-mail thread. The pontuated that this was done like that because it need to be done in 3 months, so they did it as simple as possible. They did not have anyone with backend knowledge either. And the team was small
  - I prompted him about how the Agent score was callibrated. He told me they used data from previouslly manually inserted cases as groud truth, then they could compare the results and calibrate the score. I asked him about metrics and so on, and he did not go very deep explaining metrics. No mention to recall or precision.
  - I also prompted him about the thresholds for the score, that is, what defines what is perfect extracted or not? He told me they would do an A/B test to define the threshold. Honetsly, it makes not much sense to me. How do you select thresholds with an A/B test? Why not do a AUC curve, use precision and recall metrics and so on? Did not felt very honest to me, I guess.
  - I had no further questions for him. I gave him several tips for how to behave in front of the client, to prepare a nice story and so on,
  but I am not sure if should pass. I think that he is capable engineer, but his english is not very good and he could not go very deep in his reasoning. I think I could have a different idea of that if I had interview him in portuguese, but that on its own is another problem, afterall the client is from the US.

Overall bar level for this segment (if used): [ ] Above bar   [ ] At bar   [X] Below bar

## Segment 5 — Candidate Questions (5 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

-

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Depth of real technical ownership under follow-up | High | 2           | |
| Ambiguity tolerance & independent ownership | High | 2           | |
| Data-flow / root-cause investigation | High | 2           | |
| Hypothesis-driven analysis | High | 2           | |
| Communication to non-technical stakeholders | High | 4           | |
| Python reasoning | Medium | 3           | |
| SQL on large, complex datasets | Medium | 3           | |
| Rule-logic & redundancy reasoning | Medium | 1           | |
| JavaScript familiarity | Low | 1           | |
| Snowflake / domain exposure | Low | 3           | |

## Decision Lean

[ ] Strong hire   [ ] Borderline   [X] No hire

Justification:

-

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-
