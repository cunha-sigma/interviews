# Candidate Review — Andrii Oshlianskyi

## English level

- Rating: 4 / 5 bars (Upper-Intermediate)
- Feedback: No communication difficulties came up across an extensive technical conversation — he explained nuanced ideas clearly throughout. Treat this as an estimate based on the absence of any noted issues rather than a directly scored language assessment.

## Interpersonal skills

- Rating: 3 / 5 bars (Good)
- Feedback:
  - **Receptiveness to feedback:** He was receptive for feedbacks given at the system design task, quick learner.
  - **Collaboration:** Not directly observed in this interview.
  - **Attitude to work:** Described taking real ownership of a genuinely messy problem — cleaning up conflated language data, generating synthetic data for underrepresented cases — with clear engagement in the details rather than a surface-level account.
  - **Communication:** Came across as someone who understood what he was talking about and could walk through his reasoning clearly, even while visibly unfamiliar with the diagramming tool used for the hands-on exercise — the friction there was toolchain-related, not conceptual.

## Analytical skills

- Rating: 4 / 5 bars (Strong)
- Feedback:
  - **Ability to collect and analyze information:** Strong across the board — correctly explained calibration, described a rigorous approach to proving a change is actually an improvement (a held-out reference set, a baseline, tracking cost and latency), gave a thoughtful answer on selecting representative examples for a prompt (including bringing up retrieval-based example selection unprompted), and correctly identified that matching near-duplicate values is fundamentally an entity-resolution problem rather than something a database query alone can solve. One clear gap: couldn't distinguish precision from recall, which is a basic classification concept — a real miss given how strong the rest of his reasoning was.
  - **Problem-solving:** Very good. In a live system-design exercise he produced a genuinely sound design: confidence-tiered routing to either automatic output or human review, a dedicated store for human corrections, and a controlled path for turning those corrections into production improvements. Most of this came out on his own, without needing to be prompted.
  - **Decision making:** Gave a strong, specific example — owning a language-classification effort across many languages, including deciding to reconstruct and clean a dataset (catching language data that had been mixed together incorrectly) and choosing to generate synthetic data for languages that were underrepresented.

## Self-Organization

- Rating: 3 / 5 bars (Good)
- Feedback: Not directly tested, so this is inferred from how he described his own work rather than observed live. The project example he gave showed real attention to detail (catching a subtle data-quality issue where two languages' data had been mixed together) and self-directed initiative (proactively generating synthetic data to cover gaps rather than waiting to be told to). No signal either way on multitasking specifically.

## Candidate's Level

- Rating: 4 / 5 bars (Senior)
- Feedback: Solid Senior-level profile. Strong depth in LLM engineering, cloud infrastructure, and system design — the live design exercise in particular showed him reasoning about confidence-based routing, human review, and a safe path back to production largely on his own. The main gap is on the classical ML/statistics side (missing precision vs. recall), consistent with someone earlier in that specific area relative to his broader engineering strength.

---

## Recommendation

Strong hire. His system-design thinking and LLM/infrastructure depth were genuinely impressive, especially given it was his first time doing a live design exercise and he wasn't comfortable with the diagramming tool. The one real gap — basic classification metrics — is narrow and coachable, and didn't hold back the overall quality of his reasoning elsewhere.
