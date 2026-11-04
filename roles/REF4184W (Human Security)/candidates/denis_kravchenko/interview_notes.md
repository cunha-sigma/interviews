# Interview Notes — Denis Kravchenko

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md` (v2 — Project Deep-Dive is the primary segment, follow the 3-layer follow-up ladder). When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Résumé Summary (pre-filled from `cv.md` before the interview)

- Senior Data Analyst / acting Team Lead at Tilting Point (mobile games) since Nov 2023: reshaped segmentation/offers (+20% revenue metric), built a new-user conversion model (+20% conversion, 6x for lowest tier). Manages 2 direct reports.
- Prior: Senior Data Analyst at WhaleApp (built analytics from zero as sole analyst — events → dashboards → hypothesis; ML-driven return-on-ad-spend optimization, +10%), Data Analyst/Data Scientist at Bank Vostok (fintech app, SQL/Python, CNNs, product/attribution analytics), Lead Data Analyst & Business Development at Roshen (8 direct reports, built reporting system from scratch, 8-figure EUR sales region).
- Standout vs. the last two candidates: explicit "fraud prediction" ML experience (3 years, likely from the Bank Vostok fintech role) — the closest direct domain adjacency to this role's cybersecurity/fraud-plus we've seen. Also Ukraine-based, matching the role's actual stated location.
- No rule-based detection/cybersecurity-product experience specifically, no JavaScript in the CV. Consistent people-management thread (2 then 8 direct reports) — worth directly checking he wants a hands-on IC role, not a path back toward management. Heavy gaming-industry domain in his two most recent, most senior roles.
- Best deep-dive anchors: his two current Tilting Point projects (below) — prioritize recency over topical fit. A fresher ML-flavored story beats an older "on-theme" one, since the signal wanted is general investigative depth under ambiguity, not literally data-investigation-specific content. His WhaleApp "built analytics from zero" story is a strong backup if Tilting Point runs dry, but it's ~2 years old — check whether it's actually fresh in his memory before leaning on it.
- See `cv.md` for full detail and the complete "Notable for this role" gap analysis.

## Deep-Dive prep — Tilting Point projects (prioritize these for recency)

Two headline bullets from his current role, both currently just resume-bullet-level ("reshaped segmentation... → +20%", "created a model... → +20%, 6x Tier 3"). Let him pick whichever he's more comfortable going deep on — don't pick for him. Push on both the mechanics and, most importantly, how the numbers were actually validated.

**Project A — Segmentation & offer reshape → +20% ARPDAU** (average revenue per daily active user)
- *Layer 1:* What told him the old segmentation/offers were underperforming? How were users grouped before vs. after — what specific variables defined the new groups? What concretely changed in the offer system (prices, offer content, timing/triggers)?
- *Layer 2:* Why segmentation specifically, over other levers (e.g. broad pricing changes)? What did he rule out? Who decided these variables mattered — his own hypothesis, or was he told to investigate this?
- *Layer 3:* How exactly is ARPDAU calculated, and over what window was the +20% measured? Was this a proper A/B test (control group) or a before/after comparison on the whole population — if before/after, how did he rule out seasonality, a concurrent marketing push, a new content drop? **Was the +20% target set for him upfront, or did he define what success would even look like himself?** (The most important question — the direct test for vague success criteria, ask it regardless of which project he picks.)

**Project B — New-user conversion model → +20% conversion, 6x for Tier 3**
- *Layer 1:* What does "conversion" mean precisely (free → paying user)? What is "Tier 3" and who defined that tiering? What fed the model (data/features, model type)? How was the output actually used operationally (different offer, onboarding, ad targeting)?
- *Layer 2:* Why a predictive model instead of a simpler rule (e.g. "users who do X on day one convert more")? This tests the same simplicity-vs-complexity judgment the role actually needs, even though the artifact is a model. Who set the "+20%, 6x Tier 3" target — handed to him, or proposed after seeing early results?
- *Layer 3:* How is "conversion" measured, over what window, and what's the "6x" relative to (a baseline period, a control group)? A/B test or before/after — same confound check as above. Any ongoing monitoring after launch — did accuracy hold up, any retraining?

## Segment 0 - Daria Riumina (Project Manager)

Daria will begin the interview with behavorial questions and general information abot the project.

### General Overview of the company
- Company works with AdTech, identifying cybersecurity issues, protecting from attacks and so on
- A lot of data, a lot of things running on background, behind the ads in user's screens
- This is not a regular Data Scientist/Data Engineering position, but rather a one-man-army. Someone that will be able
to join the project, investigate how it works, identify problems, errors and so on, and improve it.
- A lot of digging deep
- A position for someone ready to be responsible and to be the leader where it is directed
- The client someone to be on this position so they can relly on
- The position is focused on investigation and improvement of the whole system rather than a ML project.
- You will have a manager and other engineers/data scientists alongside yourself to help you and work together, but probably their
objetives will be slightly different

#### Answers from Denis:
- This person would be responsible for investigating and improving the system. Will I be completely alone taking accountability
for everything that happens?
  - No, you will have a manager and other people together, so you wont be accoutable for all that happens there, it is a team.
- I am a data analysit, so my work has been detail based since the beggining, so that kind of environment is not new for me.

### Daria again:
- This project is not about developing ML solutions to problems, but rather investing an existing system and improve it.
Not about creating a new ML-based system, but improving the existing one by investigating it, udnerstanding data flow and so on.
- If eventually that position will required ML development isn't something I can say now. Could be, but no guarantees.

- Tell me about an experience when something was going really wrong, a challenging case, and how you managed it:
  - In my last experience in gaming, we decided to change to move the monetization system from a thyrd party to internal
  - It looks ok, but eventually problems arise and I had to investigate what happent, what was going wrong
  - That took a lot of time, took around 2 months, and I had to figure out what was happening with our revenue
  - Finally I found the issue and fixed it
  - I the middle there was a lot of meetings with C-level people, product, engineers to understand what goes wrong
  - Basically a wrong parameter for recencny was being used, so instead of increasing the price after the purchase, it wasn doint it
  - Therefore the user would have enough, no need to rebuy anything and there it goes our revenue
  - I was the owner of monetization, so I had to lead and execute the debugging.

- Imagine that you joined a team in an caotic environment, where you are a one-man-army, a lot of responsiblity and ownership.
It turns out that your manager is very caotic and his organization was bad. You plan you tasks for the next 2 weeks, but suddenly
you manager comes and says nothing of that is valid anymore, you need to switch. That happens constatly. How would that work for you?
  - The best is to talk and understand companies priorities. Everyone wants to gain more and spend less. So my position would be to 
stand with the companies objetvie and try to negotiate with the manager. Like, if we do not this to do something else,
this will put other stuff at risk, so we need to prioritize and choose what should be done. Explain limitations and problems and try
to find a solution.
- Now lets say you talked with your manager and he said ok, not a problem, but the chaos still there. Nothing changes. What would be your
step 2?
  - It would depend on the situation, like understanding company context and all that. But if the problem is the organization, and nothing
can be changed, then eventually I could not live with it and I cannot change how it works, I would just move away and find another project to work.
  - One way of doing: I suggest something and he approves what to do. So I manage the manager somehow. 
  - If that is not possible, I can jsut show my tasks, why the did not finished, and protect my vision of the problem 
regarding the company. The company cannot be successful in this environemtn. If I cant change, I cant live with the problem forever. I will
try the best I can to help to improve the situation.
  - If the problem is manageble, I will try my best to fix it. If it isn't, I would jsut try to move to another company.


## Segment 1 — Quick Introduction + Role Context (5 min)

> "Our customer is a cybersecurity company. Part of their product works by flagging suspicious activity using a large set of rules — each rule combines several different pieces of measurable data, which we call signals, using logical conditions, to decide whether something should be flagged. Over time, a lot of these rules have accumulated, and it's not clear anymore which ones are still pulling their weight versus doing the same job as another rule, or combining signals in an overly complicated way that nobody has revisited in a while. To be clear, this specific project is not about analyzing bots or security threats themselves — it's about understanding this rule system and the data flowing through it well enough to simplify it: finding redundant or overly complex rules, tracing how the underlying data is gathered and processed, and proposing changes that make the system simpler without making it worse at catching what it's supposed to catch. You'd need to figure out the downstream impact of any change before it ships, and explain your reasoning clearly to both engineers and people without a technical background. This is a newly forming team, so you'd be working independently much of the time, pulling in engineers, other data scientists, and security researchers for context rather than waiting for things to be handed to you."

Notes (background, story):

- I am from Ukraine, living outside ukraine.
- I have been working in various spheres, game dev, HR, advertising models, big international companies, like Danone
- I also worked with business development, but I was not enjoying.
- What I like more is digging out data, udnerstand signals, investigation, all that. So not very fit for business, more for technical
- I decided to switch to analytics in 2017. I used to be a manager, 8 people.
- I like to work remote, so office work is not the best option for me, so I switched to tech to earn in foreign company and stay at home.
- My first one was a fintech bank, developing a fintech app. Doing the dashboarding part, digging into the database, create dashboards
based on needs and all that.
- So I was also working alongside data scientists, and I lied it. Eventually I began working with fraud detection, using computer vision.
- I was truing to identify if pictures were suspicious or not. So I used python, tensorflow, all that neural network model stuff
- Later I joined WhaleApp, it is a game dev company. I used to be the Senior Analyst. Several project, in on of them I was the single analyst.
- Everything from setup to analysis was being do by myself. Ingestion, analytics, metrics.
- That project was canceld, then I moved to another one where I was working with monetization on gaming.
- Basically try to extract revenue from the gamers playing the games
- Bascillay predict client value based on previous behavior and preferences
- Marketing guys were lookin on a seven looking days, but cohort behaves different based on different geos, like in US they churn,
but eastern europe stays. That affects ROAs on high horizonts, and as the marketing campaing does not pay quickly, so it is importnat.
- Finally I switched to another company, it was bought by a big US gaming company.
- So I had to step up to lead, as our lead went to army, so I had to deal with that.
- This also requires fraud activies and regular activies, progression on the levels, influx/outflux, paywalls, etc etc.
- I finished this assingment a couple months ago, so I joined a gaming company and working with t

## Segment 2 — Project Deep-Dive (30–35 min): the primary signal

> "Pick one real project from your own work where you had to dig into a data problem — something that wasn't obvious at first, where you had to investigate, make a judgment call, and see it through. Walk me through what actually happened."

Project the candidate chose:

- Project was basically in terms of gaming offers, the system was complex and looking on the first purchase. 
- That purchase marks everything, so that is used to give further prices based with offer you bouthg first
- So in the day one you receive expensive option, so if you do not by the price goes low
- So we had a rule based model that would work fine, like eastern europe, but problems arise with users from other countries.
- For instance, India the price was 6 cents, in US it was 10 dollars. 
- So my idea is how could we suggest a good offer for guys in BRazil or India, but without hurting the users from US that already paid 10 dollars
- So we need to get lower paying people to the funnel
- I studied the problem and location was very important, and also the platform (android and iOS), the producer of the device (samsung, xiaomi, etc)
- I understaood in our data that those are different levels, so Geo, Platform, Device.
- I can count on these parameters, also with other metrics, like ram, stream capacity and other signals
- I had to find features, clean data and eventually train the model on that
- Eventually I presented the results, the model worked. 
- The idea was: doing segmentation on this prediction conversion (if high prediciton, user is valuable). 
- With that segmentation, we would know which users to send which opportunities. 
- 


**Project A — Segmentation & offer reshape → +20% ARPDAU** (average revenue per daily active user)
- *Layer 1:* What told him the old segmentation/offers were underperforming? How were users grouped before vs. after — what specific variables defined the new groups? What concretely changed in the offer system (prices, offer content, timing/triggers)?
- *Layer 2:* Why segmentation specifically, over other levers (e.g. broad pricing changes)? What did he rule out? Who decided these variables mattered — his own hypothesis, or was he told to investigate this?
- *Layer 3:* How exactly is ARPDAU calculated, and over what window was the +20% measured? Was this a proper A/B test (control group) or a before/after comparison on the whole population — if before/after, how did he rule out seasonality, a concurrent marketing push, a new content drop? **Was the +20% target set for him upfront, or did he define what success would even look like himself?** (The most important question — the direct test for vague success criteria, ask it regardless of which project he picks.)


**Layer 1 — What exactly did you do?** (specific technique/tool/steps, not a summary)

Candidate's answer:

- How did the problem came to you?
  - I was reasearching conversion by countries. We had 70% US revenue and 20% US audience. So I was thinking on why that was happening.
  - Then google shared the article about currency segmentations, so basically what was the average ticket per region
  - That was part on how to use Google tools better
  - The problem was how to distinguish rich and poor users in different countries 
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

- It was really difficult to pay attention to his explanation and take notes at the same time (his english was great,
that is not the problem). So I will explain here what happened and the places we touched and we can evaluate from there:

- The problem: gaming platform that has clients across the globe was having problems with revenue.
- Denis began to investigate the revenue issue and found that 70% of revenue comes from US while on 20% of traffic was from US.
- Based on that signal, he began to investigate the problem further and realized that the willingnes to pay was different in different places.
- So a user from US was willing to pay 10 dollars on his first boutgh, but an Indian one would not pay nothing more that 50 cents.
- So that explains the problem: they were making the same offers to every player ignoring their backgrounds. 
- So only the users from US, with more money would be able to pay. The rest of the words would not buy anything.
- So he discovered that detailed on his own, by investigating data
- With the problem in hand, the proposed a solitution: use a method to personalize the price of actions based on some selected features, the ones he identified as important in his analysis (geografy, device and device OS)
- With those features, he fitted a simple linear regression and that was enough to correctly personalize the problems
- So we was capable of selecting the simplest model type because of explainability and maintenance easeness.
- The success criteria was not clear, because revenue was being lost, so any improvement would be a good one
- After developing and deploying the solution, they ran an A/B test and concluded the solution improved revenue by 20%.
- The deployment of the model was completely manual,including retraining and other processes
- He did not have an ML platform where to build things, so he had a notebook he used to train and update the model

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

Overall bar level for this segment (if used): [ ] Above bar   [ ] At bar   [ ] Below bar

## Segment 5 — Candidate Questions (5 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

- I have some friends at sigma, I have more or less understanding on hwo Sigma works
- Maybe you could tell me something about that cleint?
- 

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|---|---|-------------|---|
| Depth of real technical ownership under follow-up | High | 3           | |
| Ambiguity tolerance & independent ownership | High | 3           | |
| Data-flow / root-cause investigation | High | 3           | Adjusted from 4 — same pricing-personalization story also backs Hypothesis-driven analysis below; scores aligned rather than split. |
| Hypothesis-driven analysis | High | 3           | Adjusted from 2 — same story as Data-flow investigation above (he found the willingness-to-pay pattern in the data and tested a solution), so scored consistently with it rather than lower. |
| Communication to non-technical stakeholders | High | 3           | |
| Python reasoning | Medium | 2           | Not directly tested (no Segment 3 questions asked) — based on overall impression from the Deep-Dive (he personally built/trained the regression model in a notebook). |
| SQL on large, complex datasets | Medium | 2           | Not directly tested — based on overall impression / career history (dashboarding, database work), not a direct question. |
| Rule-logic & redundancy reasoning | Medium | 3           | Not directly tested — based on overall impression of his analytical reasoning generally, not a direct question. |
| JavaScript familiarity | Low | 2           | Not directly tested — no JavaScript evidence anywhere in the notes or CV; scored as a default low estimate, not an observed answer. |
| Snowflake / domain exposure | Low | 2           | Not directly tested — no Snowflake-specific evidence; scored from general gaming/fintech domain exposure, not a direct question. |

## Decision Lean

[ ] Strong hire   [X] Borderline   [ ] No hire

Justification:

- He is not above the bar, but fits the description and should go to the next interview

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-
