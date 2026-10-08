# Candidate Review — Denys Shkliarenko

## English level

- Rating: 4 / 5 bars (Upper-Intermediate)
- Feedback: No communication or language difficulties were noted during the interview. Treat this as an estimate based on the absence of any noted issues rather than a directly scored language assessment.

## Interpersonal skills

- Rating: 3 / 5 bars (Good)
- Feedback:
  - **Receptiveness to feedback:** When he was stuck on a question, walking him through an analogous, simpler example helped him engage with it further — some responsiveness to guidance, though limited independent recovery.
  - **Collaboration:** Not directly observed in this interview.
  - **Attitude to work:** Described a broad, hands-on career across a recommendation system, early LLM fine-tuning work, and two real production LLM projects, with genuine detail about how each was actually built — engaged and forthcoming about his own work.
  - **Communication:** No specific issues noted, though several answers were somewhat unfocused and touched on multiple loosely related points rather than directly addressing what was asked.

## Analytical skills

- Rating: 2 / 5 bars (Basic)
- Feedback:
  - **Ability to collect and analyze information:** This was the weak point of the interview. On how to determine an appropriate confidence threshold, he didn't independently reference using a labeled reference set and struggled to explain the mechanism once prompted toward one. On how to rigorously confirm a new version is actually better, he couldn't answer independently and needed to be walked through how the same idea works for a simpler, traditional ML model first. Asked specifically about grounding — anchoring a model's output to its actual source — he described general prompt-engineering practices (defining classes, rules, examples, output format) rather than the concept itself, suggesting the specific idea wasn't familiar. On selecting example inputs/outputs for a prompt, he couldn't identify that there's a practical limit to how many should be used. On the positive side, he gave a solid, correct answer on why a reference dataset needs balanced, representative coverage across classes, and reasoned soundly about when raw files should land in object storage versus a database.
  - **Problem-solving:** Also weak. Given a hands-on system-design exercise, he wasn't able to work through the core pieces of the design — recognizing what happens at each stage, how confidence and fallback should work, how to safely roll out and evaluate a change — even with direct prompting on each of them.
  - **Decision making:** Not assessed — not covered in the time available.

## Self-Organization

- Rating: Not assessed in this interview
- Feedback: Nothing in this interview gave a clean read on self-education, attention to detail, follow-through, or multitasking specifically.

## Candidate's Level

- Rating: 2 / 5 bars (Junior)
- Feedback: His career background — a recommendation system built end-to-end, real production LLM projects, and a shift into MLOps and cloud migration work — reads as broadly senior in scope on paper. But on the skills this role depends on most, the interview told a different story: reasoning about evaluating and trusting changes to an LLM-based system, grounding model output, and designing the end-to-end system itself were all genuine, consistent struggles, even with direct prompting on each point. For this specific role, his demonstrated level today is well below what his years of experience would suggest.

---

## Recommendation

No hire. Solid, credible experience on paper and genuine engagement describing his own work, but a real and repeated gap on the reasoning this role depends on most — evaluating whether an LLM-based system is actually improving, grounding its output, and designing the system end-to-end — that didn't resolve even with direct prompting on every part of it.
