# Interview Notes — Gabriel de Almeida Sena

Date:
Interviewer: Rodrigo Cunha

> Fill this in live during the interview. Structure mirrors `interview_plan.md`. When done, ask Claude to run the `evaluate-candidate` skill to turn this into a filled `review_template.md`.

---

## Segment 1 — Candidate Introduction (10 min)

Notes (background, story, how they frame their own experience):

- A project he is proud of:
  - In the banking area he constructed a fraud detection project
  - Python + ML tools + PySpark
  - Risk score + churn alaysis + analytics
  - Integration with the ETL atuomated workflows
  - Operational dashboards
  - His core role was as data engineer, however he took part in developing ML parts as well
- What experience does you have with very large datasets (Big Big Big Data, such as we have in AdTech)
  - His experience is mostly from AbInbev, in the banking sector. Efficient partition, using broadcast, leverage the incremental process to avoid full data scans, optimize file sizes, formats, and all that stuff at the Data Lake
- Why would you like to move from Data Engineering to ML?
  - I joined data industry not only because I liked data engineering, but because I like everything around data itself. So I began as Data Engineer, but I want to keep evolving to ML roles, because I understand is a natural direction. So I want to improve myself in this area.
- Candidate questions:
  - 
     

## Segment 2 — Role Context (interviewer-led, 10 min)

Candidate reactions / questions during this segment, if any:

-

## Segment 3 — Technical Q&A (10–20 min)

For each area, note which question(s) you asked and how the candidate did. Leave an area blank if not covered.

### SWE Fundamentals / Big-O
Question(s) asked:

Successfully solved the challenge, he did knew how to do it, but he was a bit confusing in the execution, did not ask initial questions and so on. But he got it right.

Notes:

### ML Fundamentals
Question(s) asked:
Notes:

### MLOps / DevOps
Question(s) asked:
1. Describe on your own words what is MLOps and what it has to do with the model life cycle
   - Correct description of the ML life cycle (the steps)
   - He could not describe what exactly is MLOps, a bit confused about it
2. Explain accuracy, recall and precision
    - Noted that unballanced datasets make accuracy a bad metric
    - Correctly defined recall and precision

3. Define bias/variance trade off
    - Correclty defined the trade off
    - Correctlyd efined how to tackle each prolem (underfitting or overfitting)

4. Define strategies to safely replace a PRD model
   - Correctly defined what is canary deployment

5. Correctly defined how to scaled stuff in K8s, using HPA and custom metrics with Prometheus

6. 


Notes:

### K8s / Docker / Low-Latency Serving
Question(s) asked:
- Asked about scalability, he was capable of menitoning HPA and increasing number of instances given a metric. He did not mention how to do that, for instance, with Keda + Prometheus, however he did mentioned Prometheus for metric telemetry
- On low latency serving he had no experience or knowledge
Notes:

### Math Foundations
Question(s) asked:

No questions asked
Notes:

### Ownership / Self-Management
Question(s) asked:

1. A project he was proud off, however he could not explain a proper project, with concrete details. He give vague general descriptions. He did not had very good communication skills and his english wasn't very clear. I think he was a bit nervous and not used to do interviews in the recent time.
Notes:

## Segment 4 — Live System Design (10–20 min)

Prompt given (confirm you did NOT say "classifier/score/threshold"): [ ] yes, avoided it

Facilitation tracking — mark **U** (unprompted), **P** (prompted), or **M** (missing) for each topic:

| # | Topic | U/P/M | Notes |
|---|-------|-------|-------|
| 1 | Feature/data pipeline & training-serving skew | P     | |
| 2 | Decision mechanism (score+threshold vs. rule vs. other) | P     | |
| 3 | Per-segment vs. global threshold | P     | |
| 4 | How the threshold value gets chosen | M     | |
| 5 | Low-latency serving stack (runtime, inference server, feature lookup) | M     | |
| 6 | Graceful degradation / fallback | M     | |
| 7 | Deployment safety for new model versions | P     | |
| 8 | Monitoring / drift | M     | |
| 9 | Scaling strategy | P     | |

Overall bar level for this segment: [ ] Above bar   [ ] At bar   [X] Below bar

Narrative notes:

- He was not very good at presenting and his diagrams weren't good at all. It seems he never wrote any diagrams before. 
- He struggled to define the offline pipeline steps correctly, such as basic steps: ETL + feature engineering / encoding + training + threshold selection + validation + model registry upload, or versions of it. Only when I helped him he coudl define it a little better
- I gave him some tips such as: divide the system into parts, like offline, online, so that you cna thinkg about them separately. But even though he struggled to separate things
- On the API part, he did not draw a separate API in the diagram and he was not able to demonstrate solid knowledge on APIs

## Segment 5 — Candidate Questions (5 min)

Focus of questions (technical/systems vs. logistics/comp), and notes:

- How is the environment in the company, is there too much pressure? Does you work extra hours often?

## Scorecard (score 1–4 per dimension; see `interview_plan.md` for weights/rubric)

| Dimension | Weight | Score (1–4) | Notes |
|-----------|--------|-------------|-------|
| SWE fundamentals / Big-O reasoning | High | 4           | |
| ML fundamentals | High | 3           | |
| MLOps / DevOps | High | 2           | |
| K8s / Docker / serving infra | High | 3           | |
| Low-latency / performance instincts | High | 1           | |
| System design synthesis (Segment 4 overall) | High | 1           | |
| Math foundations | Medium | 2           | |
| Ownership / self-management | Medium | 2           | |
| Communication clarity | Medium | 1           | |

## Decision Lean

[ ] Strong hire   [ ] Borderline   [X] No hire

Justification:

-

## Other Notes / Red Flags

(e.g., CV inconsistencies to raise, follow-up round recommendations, English level observations, interpersonal/analytical skill moments worth quoting)

- He has solid experience in Data Engineering. He worked maitaining Data Lakes, so he probably has good knowledge of Spark optmizations, how to make it run smoothly and so, which is a good knowledge in such intensive in terms of data volume
- However, he has little knolwdge about Low Latency serving or any type of serving beyond serving services from MLFlow Databricks platform. He never did that on his own, only simple stuff. API, but for models with no strong requirements regarding thoruput or latency
- He is actively trying to migrate from Data Engineering to ML Engineering, so it makes sense that he does not have solid knowledge on that. So he could be a fast learner and eventually be a good idea. However, the client declined previous candidates, that had solid knowldege on Traffic Shaping, but had no knowledge on DevOps, MLOps, Serving for low latency and so on. The client wants an engineer with end-to-end capabilities.