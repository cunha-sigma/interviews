# Interview Notes — Reinaldo Souza Gomes

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

- About 8 years with software
- Mainly with Python, but databases, containers, APIs, clouds, etc, all of that
- Mostly with automation projects
- Recently have been working with using AI and LLMs to integrate that into the projects, not only for coding assistance.

- At Thoughtful AI
  - They have clients for which they abstract the complexity and bureocracy for medical clients, like medical claims, etc. Quite different systems, a lot of integrations between those systems, we are currently working on automating that worklfow
  - I was working more on the part of the system that was responsible for validating claims
  - That form could be sent to a payer to get payment for a given pacient
  - I had to sit with the stackholders and understad their needs as the briefing was poor
  - I was doing that project from scratch
  - Later on that would be integrated to other applications
>>>>> The candidate is not very good at presenting himself, he strugles to give futher context and details about his experience. Maybe, it is a language barrier. But he is telling a lot about experiences where he needed to get the requirements from the client, like mining it from the client. That is a good sign, but I don't know if he is doing that because that is what the role was asking for.
  - Full responsability and automony to define and develop the agents. 
  - The project apparently was for a given client, similar to consultancy, and it was something like a start up.
  - The business model for that client was more or like from an startup. Custom based development, so not a big scale exactly
  - ***How did you get requirements form a project, someone gave it to you, you had to get it yourself?***
    - Answer: when the project started, the PM would have a first conversation with the client, to understand the overall scenario. We, the SE, would also joing those conversations to help understanding client requirements. 
    >>>> He struggles to give good answers for that question, something like "I had to work with the cleint and get the requiremenst from him". Hi is being prompted to give that kind of report, but he apprently cannot express himself very well in english or maybe he is just nervous and will improve over the course of the interview...
    - Answer: the first interview was just to understand the overview, so during the evolutijn of the proejct, understanding the requirements was part of the project. We had weekly meetings where we would provide the progress, get feedback and align the expectations. So we iterated using agile tactics.
  - ***Remind me why you stopped working with Thouthgful AI***
    - Answer: They have been acquired by another company, but after that the entier company was terminated. Maybe that has something to do with their business model.
  - ***Since then, what have you been doing (he left in April 2026, we are in late August 2026)***?
    - Answer: spent some time doing intervies, but nothing so far.
  - ***What are you preferences right now, what do you want to work with? Stack, technologies, team, environment, etc?****
    - Answer: 
      1. I really like working with Python. Not a problem to work with other languages, but I have a preference
      2. Like working with integrations and migrations
      3. Prefere to work with applications from scratch, getting requirements and etc, rather than working with ad-hoc features here and there.
      4. I have some frontend knowledge, but I rock at backend
      5. I expect the team to be helpful, specially in the begging, for managers I expect them to solve the non-tech stuff I cannot get done.
      6. I like to manage my own time instead of having a fixed clock.
  - *** The teams are expected to be around between 9am and 5pm. If you have kids and need to leave for a while, is ok, but overall the schedule is important?***
    - Answer: I usually prefer to work at night >>>> I don't know, maybe he missunderstood the question?
  - ***This role is for an FDE engineer, so specific custom stuff, for custom clients, fast pacing, short term projects, mostly small changes, fast pacing. So no months duration proejcts, but weekly duration projects. They use FastAPI, Pydantic, React (it is a plus), streamlit, pandas, not AWS, they use Render. Integrations, ERPs, salesforce, this kind of stuff is very common and expected to happen all the time. A lot of attention in clarifying ambiguous requirements, so we usually need to extract the correct requirements from the client, they will never know what they really need, that is the engineer work. We need mature person that can handle this environment. Is more about that rather than technologies specifically.***
    - Answer: I am preffy confortable working with this kind of environment, that is pretty much what I have been doing since the begging of my carrer.

Candiate questions:
- Is it a team of engineers or will I be working alone?
  - The team is consisted of onshore and offshore engineers, about to 5 people with intentions to add 2 more. 
- Do they use agile?
  - They have dailies, weeklies and all, but it is hard to asure that because we don't know it yet, but we can expect Agile ideas working on there.


  

## Segment 2 — Role Context (interviewer-led, 5 min)

Candidate reactions / questions during this segment, if any:

- Not unfamiliar environemnt to me, but that is pretty much what I have been working since the begging, so I that is what I have been doing, prompting clients to understand what they want. I am used to work in such environments. 

## Segment 3 — Technical & Behavioral Q&A (10–15 min)

For each area, note which question(s) you asked and how the candidate did. Leave an area blank if not covered.

### Python / FastAPI / Pydantic
Question(s) asked:
1. Why Pydantic > dataclasses
    - validations
    - automatically get data into your objects and you can make sure that this data is valid
    - Documentation

Notes: He gave a good answer, but at first he just mentioned validation, I had to prompt him to talk more about it so he could name other benefits. His wanswers were fine.


### REST / System Integration (idempotency, reconciliation/sync)
2. How to run long-running operations inside FastAPI without blocking?
   - Dispatch that to a worker or something like this is a possibilit
   - But he also named using background process
   - At this point, I asked him if by background process he meant async functions
   - He said no, that was something else, but he could explain how the async endpoint works and that usually you want to use that to I/O bound operations

3. What indempontent means?
   - Something you run several times and produces the same results as long it uses the same arguments
   - It is importnat because lets say I have an endpoint that creates a record, I don't want to run that endpoint many times and have duplicated records
   >>>> That is not a good explanation I think, is not about duplicated records, more like the stateless operation, but I think is 5/10.

4. How to reconcile records form two systems:
   - If that needs to be done in real time, would need some event-drive system that will run some type of check whenever a new record is created in the second system, assuming system A is ground truth and system B is being synced to it.
   - If not need to do that in real time, I could just run a batch process that will save outputs from both systems, run a join or similar operation and compare the results.

### Decimal / Financial Precision
5. Why shoulnd't float be used for financial calculations?
   - Because of float precision. If you add 0.1 + 0.3, you will likelly have 0.40000000000000009, not 0.4. When talking about money, that matters.
   - To tackle that problem, I would just use Decimal format in Python

### Agile / PR Workflow
6. Tell me about you PR workflow
   - He described create a branch, work on you work, do small and progressive commits, finish your work, create the tests, create the PR, ask for review and merge.

### Ambiguity / Ownership (behavioral)
There wasn't time to prompt him about, but as he explained about his experiences, he used to this type of environment. But to be honest, I did not felt him like very confident, so maybe he was nervous about the interview. So he did not give profound details on what he does, on how he does handle this type of situation and so on. He said that he is ok with that, but from my perspective I wasn't able to assert that for sure.

## Segment 4 — Live Coding Exercise (~30 min)


```
from typing import List, Tuple
 
 
def is_inserted(old_record: dict, new_record: dict) -> bool:
    pass
 
def get_identifiers(identifiers: List[str], record: dict ) -> Tuple[str]:
    return tuple(record[f] for f in identifiers)
 
 
def compare_records(old_records: List[dict], new_records: List[dict], identifiers: List[str]):
    old_by_key = {get_identifiers(identifiers, r): r for r in old_records}
    new_by_key = {get_identifiers(identifiers, r): r for r in new_records}
    inserted_keys = set(new_by_key.keys()) - set(old_by_key.keys())
    inserted_records = [new_by_key[k] for k in inserted_keys]
    deleted_keys = set(old_by_key.keys()) - set(new_by_key.keys())
    deleted_records = [new_by_key[k] for k in deleted_keys]
```


Array reconciliation task given: [X] yes

Notes on approach:

- Clarifying questions asked before coding?
  - Not that much, he just began writing a for loop
  - I had to prompt him a lot and give him tips so he could solve something
- Composite key construction (collision-aware?):
  - I had to prompt him on how to build that, he was struggling. I wanted to see if, after building the {composity_key: record} structure he would be able to get inserted and deleted. I had to tell him to create a specific function to extract those composite keys and then creating the structure as I mentioned. 
- Correctly distinguished updated vs. unchanged?
  - He was onl able to the inserted and deleted, and I had to tell him to use sets. He was not able to get the updated part.
- Naive (nested-loop) vs. hash-map approach — which did they reach for, and did they get to O(N+M) with or without a nudge?
  - His first idea was the naive approach, but as I told him before the Big) notation was important, he tried to do it in another way. But he wasn't capable of doing it without myself prompting him.
- Stated complexity themselves?
  - We did not get to to that time in the live coding, he did not finished it. I have hsi code below.
- Communication while coding:
  - Not very good. Did not feel confident, did not asked correct questions, apparently wasnt very confortable working with pyhton data structures.

Correctness: [ ] Solved cleanly   [ ] Solved with help   [X] Did not solve

## Segment 5 — Optional System Design / Integration Discussion (0–15 min)

Covered: [ ] yes   [X] skipped (time/signal already clear)

Notes:

-

## Segment 6 — Candidate Questions (5 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

- He just asked about the environment working at Sigma. General questions, difficult to answer because that changes from projec tto proecjt.

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|-----------|--------|-------------|-------|
| Ambiguity tolerance / self-directed scoping | High | 2.5         |       |
| Ownership & independent decision-making | High | 2.5         |       |
| Live coding exercise (correctness + complexity) | High | 1           |       |
| Python / FastAPI / Pydantic proficiency | Medium | 3.3         |       |
| REST / integration patterns | Medium | 3           |       |
| Decimal / financial precision discipline | Medium | 3           |       |
| Testing & observability habits | Medium |             | 3     |
| Agile / PR workflow fluency | Medium |             | 3     |
| Communication clarity | High |             | 2.5   |

## Decision Lean

[ ] Strong hire   [ ] Borderline   [X] No hire

Justification:

- He lacks communication skills, but it seems he has FastAPI knowledge and all that. The performance in the live coding could be explained by the lack of training is such situations. Someone is not bad just because it fail live coding. I think he worth a chance, but that depends on other candidates. I would send him through specially because would be the first candidate. Something a "boi de piranha", at least.
- **Final call: strong no hire.** Signal was clear enough (live coding + communication) that no further probing was needed.

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

- His communication skills are not great. Maybe he was nervous, but his vocabulary is not very big, but that is not a problem either, the thing is that he does not feel veryc onfident, so it does not guarantee confidence.
