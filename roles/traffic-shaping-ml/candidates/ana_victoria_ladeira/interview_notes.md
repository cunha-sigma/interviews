# Interview Notes — Ana Victória Ladeira

Date:
Interviewer: Rodrigo Cunha

> Fill this in live during the interview. Structure mirrors `interview_plan.md`. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Segment 1 — Candidate Introduction (10 min)

Notes (background, story, how they frame their own experience):

- 12+ years of exeperience
- Very well trained in ML, AI agents, math skills, probability
- Little experience with actual deployment (in her experiences, she always handled over to someone else to do it)
- Looking for new opportunity where she could improve her MLOps skills

## Segment 2 — Role Context (interviewer-led, 10 min)

Candidate reactions / questions during this segment, if any:

- She demonstrated interest in working with AdTech industry, specially in a project that will begin from scratch without legacy tech debt

## Segment 3 — Technical Q&A (10–20 min)

For each area, note which question(s) you asked and how the candidate did. Leave an area blank if not covered.

### SWE Fundamentals / Big-O
Question(s) asked: 
Notes: She has senior level on that

### ML Fundamentals
Question(s) asked:
Notes: Very well trained

### MLOps / DevOps
Question(s) asked:
Notes: Intermediate

### K8s / Docker / Low-Latency Serving
Question(s) asked:
Notes: Has no experience

### Math Foundations
Question(s) asked:
Notes: Very good

### Ownership / Self-Management
Question(s) asked:
Notes: Had managed teams before, she likes doing so

## Segment 4 — Live System Design (10–20 min)

Prompt given (confirm you did NOT say "classifier/score/threshold"): [yes] yes, avoided it

Facilitation tracking — mark **U** (unprompted), **P** (prompted), or **M** (missing) for each topic:

| # | Topic | U/P/M | Notes |
|---|-------|-------|-------|
| 1 | Feature/data pipeline & training-serving skew | P     | |
| 2 | Decision mechanism (score+threshold vs. rule vs. other) | P     | |
| 3 | Per-segment vs. global threshold | M     | |
| 4 | How the threshold value gets chosen | M     | |
| 5 | Low-latency serving stack (runtime, inference server, feature lookup) | M     | |
| 6 | Graceful degradation / fallback | M     | |
| 7 | Deployment safety for new model versions | P     | |
| 8 | Monitoring / drift | U     | |
| 9 | Scaling strategy | P     | |

Overall bar level for this segment: [ ] Above bar   [X] At bar   [ ] Below bar

Narrative notes:

- She is very well trained in ML and AI in general. But lacks other knowldege regarding DevOps, deployment, K8s, infrastructure, etc. Has built APIs before with FastAPI, but never with low latency requirements.

## Segment 5 — Candidate Questions (5 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

- Focused on thecnical system, if we worked after hours very often, what was the biggest head ach in ADTech

## Scorecard (score 1–4 per dimension; see `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|-----------|--------|-------------|-------|
| SWE fundamentals / Big-O reasoning | High | 2           | |
| ML fundamentals | High | 4           | |
| MLOps / DevOps | High | 3           | |
| K8s / Docker / serving infra | High | 1           | |
| Low-latency / performance instincts | High | 1           | |
| System design synthesis (Segment 4 overall) | High | 3           | |
| Math foundations | Medium | 4           | |
| Ownership / self-management | Medium | 4           | |
| Communication clarity | Medium | 4           | |

## Decision Lean

[ ] Strong hire   [X] Borderline   [ ] No hire

Justification:

- She lacks part of the knowledge regarding DevOps, MLOps, this kind of stuff

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

-