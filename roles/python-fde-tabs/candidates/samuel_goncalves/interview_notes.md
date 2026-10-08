# Interview Notes — Samuel Goncalves

Date:
Interviewer:

> Fill this in live during the interview. Structure mirrors this role's `interview_plan.md`. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Segment 1 — Candidate Introduction (5–10 min)

Notes (background, story, how they frame their own experience):

- More then 14 years working with development on Backend and DevOps
- Extensive knowledge on Python development and software engineering in general
- Can work with different tech stacks, but is more used with Python
- He knows a lot of Django, more than FastAPI, but could show solid knowledge about FastAPI as well
- Speaks very well, clear, confident and calm. Very good.


## Segment 2 — Role Context (interviewer-led, 5 min)

Candidate reactions / questions during this segment, if any:

- I described the role and the specifications for FDE and the candidate has shown confort about the role. According to him, that is not different
from what he does in his current role.

## Segment 3 — Technical & Behavioral Q&A (10–15 min)

For each area, note which question(s) you asked and how the candidate did. Leave an area blank if not covered.

### Python / FastAPI / Pydantic
- Why Pydantic?
  - Dict is structured data, get keys, get values, however if we are using any other tools, there is no type hint, you have to manually create docs, another developer will need to udnerstand all that. Using Pydantic you can wrap up everything you need into a single class, we type hints, accessibility, much easier and much cleanier. Easier for validations, easier for documentation and could also handle everything a dataclass can but way better.

- How to handle a long running request without blocking other requests

### REST / System Integration (idempotency, reconciliation/sync)

- What is indempontent and why it matters?
  - In async projects, you many ways of communications, like RabbitMQ, but in some cases, messages can be very critical, so you need to ensure it need to be processed only once. 
  - Necessary to avoid running the same operations many times

- How to sync two systems?
  - If the main database had created_at and updated_at, so I can create a new feature named sync_at, then I can check if updated_at at origin is later than sync_at at other system is enough ok.
  - That runs in a script, out of the database.
  - Outbox pattern: I create a time and I just check updates looking for registers changed after the last sync.


### Decimal / Financial Precision
- Why not Floats in currencty calculations?
  - If we add 0.1 + 0.3 = 0.4000000000001. Float numbers do not work as we tend to expect. There is a precision problem there. Float does not ensure that. 
  - You can use instead Decimal in Python, but there are several ways to handle that. Using decimal, use integers and then scala the values, or you can create a custom type inhering from Decimal.


### Agile / PR Workflow
- There are many GitFlows possibles, many ways to manage and create PRs and so on. Depends on team, context and all that.
The simplest one:
- Get the new ticket
- Check out from main and create a new feature branch
- Start my work. When finised, check again if tehre isn't anything new in the main branch, do a rebase to get that updated, the make a squash commits, or use a merge and send all the commits.

### Ambiguity / Ownership (behavioral)

- Every engineer has done before:
  1. When you have the ack. A vague ticket does not mean the same to me and to you. I have been working in the same project for the last 5 years. If the customer says one vague phrase, like "I need a rule X when creating a customer". As I already know the customer, the client, everything, is much easier to understand the customer. That is OK to get.
  2. In this context, engineer knows anything and gets the same vague ticket. Then, we need to investigate the real intention of the user. Does it really wants what he is requesting? So I need to do good questions to try to extract from him the specifications. "What do you mean by that?" is not enough from the engineer perspective. It needs to fully understand the client

## Segment 4 — Live Coding Exercise (~30 min)

Array reconciliation task given: [X] yes

```
def compare_dicts(old: list, new: list, keys: list):
    old_keys = {_generate_keys(obj, keys) for obj in old}
    new_keys = {_generate_keys(obj, keys) for obj in new}

    deleted_keys = old_keys - new_keys
    inserted_keys = new_keys - old_keys
    changed_keys = []

    indexed_new = {_generate_keys(obj, keys): obj for obj in new}
    indexed_old = {_generate_keys(obj, keys): obj for obj in old}

    for new_obj in new:
        if _generate_keys(new_obj, keys) in inserted_keys:
            continue
        old_obj = indexed_old[_generate_keys(new_obj, keys)]

        if new_obj != old_obj:
            changed_keys.append(_generate_keys(new_obj, keys))


def _generate_keys(obj: dict, keys: list):
    return tuple(obj.get(key) for key in keys)
 
```

Notes on approach:

- Clarifying questions asked before coding?
  - Yes. Did answer good questions, took some time undersatind the problem before writing any code. When writing code, he began writing
  the function signature, with type hints and all that, which is a good sign.
- Composite key construction (collision-aware?):
  - Yes, collision aware. I had to prompt him to use Sets and make a hash map with the composite key, but that was enough for him to grasp the solution, even though we did not have enough time to finish it completely (if we had, he would get there for sure)
- Correctly distinguished updated vs. unchanged?
  - Yes, by directly comparing the dict objects (I gave him a hint)
- Naive (nested-loop) vs. hash-map approach — which did they reach for, and did they get to O(N+M) with or without a nudge?
  - He stated that the naive approach with nested loops was really bad reagarding complexity and did not even try that path.
- Stated complexity themselves?
  - Yes, correctly O(N+M)
- Communication while coding:
  - Very good. Speaks good english, with confidence, asking relevant questions and paying attention to feedback.

Correctness: [ ] Solved cleanly   [X] Solved with help   [ ] Did not solve

## Segment 5 — Optional System Design / Integration Discussion (0–15 min)

Covered: [ ] yes   [X] skipped (time/signal already clear)

Notes:

- There was no need nor time to do that.

## Segment 6 — Candidate Questions (5 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

- There was no time for that, unfortunately. But the candidata was definetly a Hire

## Scorecard (score 1–4 per dimension; see this role's `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|-----------|--------|-------------|-------|
| Ambiguity tolerance / self-directed scoping | High | 4           | |
| Ownership & independent decision-making | High | 4           | |
| Live coding exercise (correctness + complexity) | High | 3           | |
| Python / FastAPI / Pydantic proficiency | Medium | 3           | |
| REST / integration patterns | Medium | 4           | |
| Decimal / financial precision discipline | Medium | 4           | |
| Testing & observability habits | Medium | 4           | |
| Agile / PR workflow fluency | Medium | 4           | |
| Communication clarity | High | 4           | |

## Decision Lean

[X] Strong hire   [ ] Borderline   [ ] No hire

Justification:

- Communicates very well, has shown professional maturity and confidence. Hi is totally capable of assuming the role.

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

- Has worked with overseas projects before, so speaking in english with foreigners is not a problem.
