# Interview Notes — Débora Ribeiro Doimo de Campos

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md` (v2 — Project Deep-Dive is the primary segment, follow the 3-layer follow-up ladder). When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

# Daria questions (- for Daria, * for Debora):

- Currently you are a freelancer, how much difference does it make when compared with a full time job, I mean in terms of
responsibility. What is the difference.
* I always trying to do my best, so from my perspective, freelancer works the same for me.

- Tell me about your previous experience and how they communicate with that opportunity
* (She began talking about the Vivo experience, not her first one) I worder with Audit team, I was basically analyzing
different projects and trying to understand if they were bringing real value to the company. So the idea was to support
the company on being more efficient.
* She also worked as data engineer, basically moving data and transforming it.

- You participated in very different activities. A lot of analysis and team work as well. Can you tell me your biggest
challenge so far, considering your carrer experience.
* Any new project is a big challenge for me. Two months ago I was working on a project for Itau Unibanco, the idea was
to improve the architecture of the data solution and also bring more information from the clients.

- Lets imagine that you recently joined a team, still figuring out things about the project and workflow, but you already
se that there are many flaws in the project, it is obivous for you. You could fix and bring better results. But for some
reason you manager is reluctant for changes and he sees that there is no point in it. What would you change.
* First I would try to understand the problems that exist, that is, diagnosis the issues that could be improved. The second
part for me would be understanding the priorities. When I talk with the manager, I must show him what is less and most important
for the project and try to defend that position with data, results and so on, and show what would happen in case we did
not fix the problem and what would be benefit otherwise. 

- Lets imagine that you did your 3 step plan, but the solution turns out not to be that simple. The fix would take a lot
of resource and time. What is your vision in this case?
* I would try to split the problem in smaller problems and find out what we cannot miss, the "must"s. It is a tradeoff,
we can make it quickier, but we would need to cut weight.


## Résumé Summary (pre-filled from `cv.md` before the interview)

- Data & Analytics Engineer, Campinas (Brazil), 13+ yrs, currently at Oktopuz (data consultancy) since Feb 2022: ETL/ELT pipelines, analytical data models for executive dashboards, direct client/stakeholder work. Project highlights: analytics foundation for a forestry-management client, pipelines for retail/marketing clients, automated workflows/bots for an entrepreneurial-ecosystem client — all currently resume-bullet-level, need real pushing for depth.
- Prior: Adjunct Postgraduate Lecturer at PUC-Campinas (NoSQL/Data Science courses), Analytics Specialist at beOn Claro (Claro's innovation hub — hypothesis validation for new business pilots), Senior Data Scientist at Telefônica Vivo (cut data latency from 24 hours to 2 hours — concrete and measurable, but ~7 years old, check freshness before leaning on it).
- Standout vs. every prior candidate: JavaScript listed as a core competency (not just "familiarity"), and a formal academic CS/Computer Engineering background (B.S. + M.Sc.) rather than a bootcamp-driven pivot — a different profile shape.
- No rule-based detection/cybersecurity/fraud experience in the CV — the usual gap for this role. There's an unexplained ~13-month employment gap (Telefônica Vivo ends Jun 2018, beOn Claro starts Jul 2019) worth asking about directly — could simply be her M.Sc. (2017–2021, concurrent with her jobs). Currently employed — normal check on motivation/timing for a move.
- See `cv.md` for full detail and the complete "Notable for this role" gap analysis.

> "Our customer is a cybersecurity company. Part of their product works by flagging suspicious activity using a large set of rules — each rule combines several different pieces of measurable data, which we call signals, using logical conditions, to decide whether something should be flagged. Over time, a lot of these rules have accumulated, and it's not clear anymore which ones are still pulling their weight versus doing the same job as another rule, or combining signals in an overly complicated way that nobody has revisited in a while. To be clear, this specific project is not about analyzing bots or security threats themselves — it's about understanding this rule system and the data flowing through it well enough to simplify it: finding redundant or overly complex rules, tracing how the underlying data is gathered and processed, and proposing changes that make the system simpler without making it worse at catching what it's supposed to catch. You'd need to figure out the downstream impact of any change before it ships, and explain your reasoning clearly to both engineers and people without a technical background. This is a newly forming team, so you'd be working independently much of the time, pulling in engineers, other data scientists, and security researchers for context rather than waiting for things to be handed to you."


## Segment 1 — Quick Introduction + Role Context (5 min)

Notes (background, story):

-

## Segment 2 — Project Deep-Dive (30–35 min): the primary signal



Project the candidate chose:

- I worked at claro, and in one of the proejcts I worked as a Data Engineer
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

[ ] Sustained real depth (2–3 layers)   [ ] Partial — needed heavy prompting   [ ] Did not sustain depth

Notes:

-

Needed a second project to find real depth? [ ] No   [ ] Yes — note why:

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

Used? [ ] No — ran out of time   [ ] A — Rule Redundancy & Simplification   [ ] B — Live Root-Cause Investigation

Notes on approach:

-

Overall bar level for this segment (if used): [ ] Above bar   [ ] At bar   [ ] Below bar

## Segment 5 — Candidate Questions (5 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

- I would like to know how is to work with this client, is it more formal, or maybe informal? Sometimes I am a bit informal,
so I am not sure how would be to handle it.
  - No black tie environment, e-mail with communication and so on, so very easy to handle the process, easier.
  - Formal, because they are our clients, but no need for that kind of formality.

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Depth of real technical ownership under follow-up | High | 2           | |
| Ambiguity tolerance & independent ownership | High | 4           | |
| Data-flow / root-cause investigation | High | 3           | |
| Hypothesis-driven analysis | High | 2           | |
| Communication to non-technical stakeholders | High | 3           | |
| Python reasoning | Medium | 2           | |
| SQL on large, complex datasets | Medium | 4           | |
| Rule-logic & redundancy reasoning | Medium | 2           | |
| JavaScript familiarity | Low | 3           | |
| Snowflake / domain exposure | Low | 2           | |

## Decision Lean

[ ] Strong hire   [X] Borderline   [ ] No hire

Justification:

- Debora choose one project from her experience at Claro. In her view, the project was very similar to what the client was looking for,
that is why she picked that project. The problem is that she worked on that project 2 to 3 years ago, LLM weren't even avaiable yet.
I advised her to pick a recent project to preset to the client, because for that case she might struggle to remember stuff.
- Anyways, the context was this: Claro is a telecom company from Brazil, huge one. They have several data silos, owned by different teams,
nobody knows what other teams owns, everybody builds their own source of data, nobody shares nothing. A very big mess.
- She was working on a project that needed data for a given usage, but she could not find the data source or maybe a data steward that could 
explain the data to her, or tell where it was, how it was produced, what was the schema and so on.
- The point is that everytime she needed to look for some different data, was a very big struggle just to find the table that she wanted.
- Then, she tought about an idea: create a catalog of the data we know, with the help of the data stewards and other engineers, that could
serve as a catalog, with all tables available recorded with name, owner, schema, location, description and all that. Then, that catalog would be 
ingested by an Elastic Search engine that would work as a data source search engine.
- She proposed the solution to her PM and convinced him to allow her to work on it as a POC. The benefits would be great,
as anyone that worked with data inside claro would face similar problems.
- So she built herself the first version. She contacted a Data Steward that was her friend and asked him for help. Then they used
the data sources that were under that friend and built the POC. They would get information from different sources, including e-mail converstations
and other sources, non structured data, done manual.
- She would look at those sources that contained information about such tables and fill a CSV file witht he relevant infromation required for each table. Name, location, size, schema, description and so on. That CSV file would be ingested to Elastic Search using Python.
- To deliver the solution to the users, she built a simple frontend using PHP and Angular. It was basically a blank page with a search bar and auth on it. Simple.
- In order to put the POC to test, they used a Hackaton that was happning at Claro. They provided the catalog search engine as 
a tool to the hackaton participants to look for the data they wanted. The users were given a form after participation to give
feedback on system. That is how they mearured success. 
- I asked her if they would not calculate metrics to evaluate ranking or user satisfcation (if users are actually clicking the results or not), but she said they did not
- Overall, I think she picked a bad project. Was an old one, she did not remembered the details very well so I had to prompt her several times to try to understand the context. When I prompted for more complex questions, I did not feel a lot confidence on her answers.
- If she had choosen a fresher project, she might have done better.
- Another issue for her is her english. It is not bad, but is not very fluid. So I advised her to train that story using a fresh project instead of an old one. 
- In resume, I think she is borderline. She is a seasoned engineer, with a lot of experience in different domains within Data Industry, she is nice to talk with, seems to be a kind person and easy to handle. She just need to reharse her story a little bit more and pick a recent project instead of an old one. My lean is borderline. If she can think of a better story with a more recent project, she might be able to succeed.

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-
