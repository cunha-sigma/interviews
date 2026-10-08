# Senior Python Engineer — Forward Deployed Engineering (Tabs project)

**Team:** Product Delivery / Forward Deployed Engineering (FDE) — builds custom solutions and integrations for enterprise customers around the core product, not the core platform itself.
**Client / project:** "Tabs" — a billing platform. Merchant/customer integrations span a range of CRM and ERP/finance systems.
**Staffed via:** Sigma Software.

## Position overview

Unlike a traditional product engineering team with well-defined backlogs and long development cycles, this FDE team delivers customer-specific solutions on rapid turnaround — projects are often completed in about a week. Requirements evolve during development: the business outcome is known up front, but technical implementation details get refined as the engineer builds. Engineers are expected to be comfortable scoping further themselves when working with non-technical stakeholders.

## Team structure

- 4 US-based full-time Product Delivery / FDE leads (customer-facing)
- 3 offshore engineers today (2 Colombia, 1 Ukraine), actively expanding onshore and offshore
- Offshore engineers are **not** expected to talk to customers directly — US-based Product Delivery leads own that interface. Offshore engineers collaborate internally and are expected to proactively ask questions of internal stakeholders whenever something is unclear.

## Responsibilities

- Develop customer-specific backend solutions, primarily in Python
- Build custom integrations between the product and external systems
- Design and implement REST APIs
- Work with CRM integrations (Salesforce and similar)
- Investigate customer environments/APIs independently when requirements are unclear
- Participate in rapid development cycles with quick iterations
- Take ownership of technical decisions within assigned work
- Typical work extends existing product capabilities rather than modifying the core product

## Tech stack

**Must-have:**
- Python
- FastAPI, Pydantic
- Agile/Scrum experience
- PR workflow (ticket → PR → review → merge)

**Nice-to-have (not blocking):**
- React (up to ~5-10% of tickets touch it)
- Render (AWS-like platform)
- Streamlit, pandas (used in an adjacent repo)

**Familiar with:**
- CRMs — Salesforce, HubSpot (must-have)
- ERPs generally — NetSuite, QuickBooks, and others (nice-to-have; merchant integrations span a range of ERP/finance systems)

The stack itself is considered relatively straightforward by the Tech Lead — the real challenge is the working style, not the technologies.

## Important technical concepts (P1 — screen for these)

- REST frameworks (~90% of builds are on systems' REST APIs, sometimes building custom ones) and system-integration best practices: moving/syncing data between systems, idempotency
- Working in a SCRUM environment: sprints, one PR per ticket, reviewing PRs
- Writing tests alongside feature work (pytest) and instrumenting/monitoring for exceptions (Sentry) as a normal part of shipping, not an afterthought
- Strict decimal/date-precision discipline — this is a billing platform, so financial calculations use exact decimal arithmetic (never floats), with careful rounding and period-boundary handling. Comfort with that level of rigor matters more than raw framework depth.

## What makes this role different — the FDE mindset

The biggest differentiator is not the technology but the mindset. The ideal engineer is comfortable with:

- Ambiguous requirements and incomplete specifications
- Making technical decisions independently
- Asking questions proactively
- Quickly understanding unfamiliar business domains
- Delivering solutions under tight timelines

Engineers may receive only a brief description rather than a detailed ticket, and are expected to gather additional context from documentation, Slack, project plans, and internal discussions.

## Ideal candidate profile

**Technical:**
- Strong Python + FastAPI expertise
- Experience building APIs
- Comfortable learning new integrations quickly
- Familiar with Agile development

**Personal qualities (the most important part, per the Tech Lead):**
- Self-starter, comfortable making decisions independently
- Thrives in ambiguity
- Strong ownership mindset
- Excellent communication skills
- Curious and proactive
- Comfortable in a fast-paced environment

The hardest quality to find — per the Tech Lead — is comfort working with ambiguity, more so than raw technical expertise.

## Seniority

- **Senior:** communication, autonomy, and ownership are essential
- **Strong Mid-level:** acceptable if technical ability is strong mid-level but communication/autonomy/ownership are already senior-level

## Time zone

- 9:00 AM – 5:00 PM Eastern Time
- EU/Ukraine candidates considered if working until ~8pm EU / 9pm UA
- First priority: LATAM

## Hiring urgency

Immediate need; potentially hiring up to 2 engineers.

## What's known about the actual interview process

No candidate had been presented to the client as of the writing of the original hiring brief, so the first-round format hasn't been directly observed yet. What's known from a shared example: the client's technical exercise has included an algorithmic live-coding task (~30 min) and, sometimes, system-design questions (~10-15 min). One specific past exercise (shared by the client for a different but related role) is documented in `interview_plan.md` as the reference live-coding task, since it's the best concrete signal available on the client's expected difficulty level.
