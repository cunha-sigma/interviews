---
name: evaluate-candidate
description: Turns a filled-in roles/<role>/candidates/<name>/interview_notes.md into a completed roles/<role>/candidates/<name>/review.md following that role's review_template.md, scored against that role's interview_plan.md rubric and decision guidance. Use when the user asks to evaluate, grade, score, or summarize a candidate's interview, or to "fill out the review template" for someone.
---

# Evaluate Candidate

Turns live interview notes into the final candidate review, grounded strictly in what the interviewer actually wrote down — never invent observations.

This project runs interview loops for multiple roles at once, each self-contained under `roles/<role-slug>/`. This skill is role-agnostic: it locates the candidate first, then uses whichever role that candidate belongs to for its rubric and templates.

## Steps

1. **Identify the candidate and their role.** Search `roles/*/candidates/<name>/` for a folder matching the name/slug the user gave. If it matches under exactly one role, proceed. If it matches under more than one role, or isn't found at all, ask the user to disambiguate.
2. **Read the inputs, in this order, all from the matched role's folder:**
   - `roles/<role>/candidates/<candidate>/interview_notes.md` — the filled notes (source of truth for what happened)
   - `roles/<role>/interview_plan.md` — that role's screening areas, scorecard weights, bar levels, decision guidance (Strong hire / Borderline / No hire criteria) — dimension names and weights vary by role, so always pull them from this file, not from memory of another role's plan
   - `roles/<role>/candidates/review_template.md` — exact output structure/fields to produce
   - `roles/<role>/candidates/<candidate>/*.pdf` (the CV) only if notes reference something needing CV context (e.g. a claimed background detail) — don't re-derive fit from the CV alone.
3. **Sanity check the notes against every section actually present in `review_template.md`.** Templates differ per role and can have far more than 5 sections (a role's template may list a dozen-plus competencies, several of which the live interview never touches). For each template section, decide if the notes give you real evidence to responsibly assign a rating.
   - If a section has clear supporting evidence in the notes, score it.
   - If a section is empty, thin, or only inferable from a bare scorecard number with no narrative behind it, **do not fabricate or guess a score.** Instead, collect it as an open question for the user: something specific and answerable, e.g. "Was Samuel asked anything about database design/normalization? Nothing about it is in the notes." Batch all such questions together and ask the user in one pass before writing the review — don't invent plausible-sounding feedback to paper over a gap, and don't silently drop the section either.
   - Once the user answers, fold their answer into that section as if it had been in the original notes (still attributed to what actually happened in the interview, not invented embellishment).
4. **Map notes (plus any answers gathered in step 3) → every section in `review_template.md`, in the template's own order.** Common ones and how to approach them (exact section list/order always comes from the template file itself, not from this list):
   - **Overall rating**: synthesize from the scorecard and Decision Lean in the notes, plus the per-section ratings below. Weight the role's High-weight dimensions (per that role's `interview_plan.md`) more heavily than Medium ones when they conflict.
   - **English level**: only from explicit notes/answers about fluency/communication.
   - **Interpersonal skills** (receptiveness to feedback, collaboration, attitude to work, communication): draw from the intro segment, Q&A demeanor, any live-exercise/design-discussion collaboration, and closing-questions segment.
   - **Analytical skills** (collect/analyze information, problem-solving, decision making): draw primarily from the technical Q&A and the live exercise / system-design segment(s).
   - **Candidate's Level** (Intern/Junior/Middle/Senior/Principal + justification): use the role's own guidance on which dimensions are hardest to coach (stated in that role's `interview_plan.md` scorecard section) as the primary gate, not a generic assumption.
   - **Any other named competency section** (e.g. OO Design, Testing, Documentation, Algorithms & Data Structures, Databases, Security, Code Quality, AI-Augmented Development, etc.): score strictly from whatever the notes/answers actually say was asked and observed about that specific competency — these sections often won't map to any segment in the role's `interview_plan.md` at all, which is expected; that's exactly why step 3's question pass matters.
5. **Add a short unscored addendum** below the template's own sections (not part of its star system) that states: the Decision Lean from the notes (Strong hire/Borderline/No hire), any red flags from the notes (e.g. CV inconsistencies), and any recommended follow-up round if signal was thin — this mirrors that role's `interview_plan.md` decision guidance, which `review_template.md` itself has no field for.
6. **Write the result** to `roles/<role>/candidates/<candidate>/review.md`. Preserve `review_template.md`'s full section list, order, and rating scale exactly; the addendum from step 5 becomes a plain "Recommendation" section.
   - **This file is read by people outside this project.** Never reference internal artifacts by name or structure — no "interview_plan.md," "review_template.md," "the scorecard," "the notes," segment numbers, "tracking table," "U/P/M," weights, role-folder paths, or any other meta-description of the evaluation process itself. Write it as if you personally watched the interview and are giving plain, direct feedback on the candidate's skills. Translate internal concepts into plain language (e.g. "needed prompting to consider X" instead of "scored P on the tracking table"; "the coding exercise" instead of "Segment 4").
7. **Report back concisely** (to the user, not in the file): which sections had strong evidence vs. thin/missing evidence in the notes (net of whatever they answered in step 3), so the user knows what to double check before treating the review as final.
