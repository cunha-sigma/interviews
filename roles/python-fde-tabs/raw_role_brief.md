Hi guys @Andres Arango Alvarez @Heorhii Dyptan @Max Shkutnyk @Oleks Milovskyi,
I wanted to share with you the requirements for Python position, and we'd appreciate if you can help us with interviewing some candidates to the Tabs project:pray: I hope we will close this position very quickly :sweat_smile:

So, here is the description whom client need from tech and soft-skill perspective:

Senior Python Engineer (Forward Deployed Engineering Team)
Position Overview
The team is part of Product Delivery, operating as a Forward Deployed Engineering (FDE) organization rather than a traditional product engineering team. The primary goal is to build custom solutions and integrations for enterprise customers, working around the core product rather than developing the core platform itself.
Unlike traditional software development teams with well-defined backlogs and longer development cycles, the FDE team delivers customer-specific solutions with rapid turnaround times, often completing projects within approximately one week. Engineers work in an environment with evolving requirements, where the desired business outcome is known but technical implementation details are refined during development.

Engineer should be comfortable with FDE-style work: fast, ambiguous environment, quick turnaround on integrations/projects. Since we work with non-technical stakeholders (the client), FDEs sometimes have to do further scoping as they build.

Team Structure
 Current team composition:

4 US-based full-time team members acting as customer-facing Product Delivery / FDE leads
3 offshore engineers
2 in Colombia
1 in Ukraine

The team is actively expanding both onshore and offshore.

Responsibilities
 The engineer will:

Develop customer-specific backend solutions primarily in Python
Build custom integrations between the product and external systems
Design and implement REST APIs
Work with CRM integrations (Salesforce and similar platforms)
Investigate customer environments and APIs independently when requirements are unclear
Participate in rapid development cycles with quick iterations
Collaborate closely with internal Product Delivery team members
Take ownership of technical decisions within assigned work
Typical projects involve extending existing product capabilities rather than modifying the core product itself.

Important Concepts:

P1: REST frameworks (90% of our builds are on systems' REST APIs, and sometimes we build our own), system integrations (best practices for moving/syncing data between systems, idempotency), working in SCRUM environments (sprints, one PR per ticket, reviewing PRs)
P1: Comfortable writing tests alongside feature work (pytest) and instrumenting/monitoring for exceptions (Sentry) — not an afterthought, part of how we ship
P1: Strict decimal/date-precision discipline — this is a billing platform, so financial calculations are done in exact decimal arithmetic (never floats), with careful rounding and period-boundary handling. Comfort with that level of rigor matters more than raw framework depth.


Tech Stack
 Must-have:

Python
FastAPI, Pydantic
Experience working in Agile/Scrum environments
Pull request workflow (ticket → PR → review → merge)
Main technologies:

Python - must have
FastAPI - must have
React - nice to have, but not important, can be skipped, up to 5-10% of tickets can be
Render (similar to AWS) - nice to have, it's okay not to have exp with this technology
Streamlit (used in an adjacent repo), pandas - nice to have, it's okay not to have exp with this technology
Familiar with:

Must Have: CRMs (Salesforce, HubSpot)
Cool to have: ERPs generally (NetSuite, QuickBooks, and others — our merchant integrations span a range of ERP/finance systems)
The Tech Lead specifically noted that the stack itself is relatively straightforward, the challenge lies in the working style rather than the technologies.

What Makes This Role Different (FDE Mindset)
 The biggest differentiator is not the technology but the mindset.
 The ideal engineer should be comfortable with:

ambiguous requirements
incomplete specifications
making technical decisions independently
asking questions proactively
quickly understanding unfamiliar business domains
delivering solutions under tight timelines
Instead of receiving highly detailed tickets, engineers may receive only a brief description and are expected to gather additional context from documentation, Slack conversations, project plans, and internal discussions.

Client Communication
One important clarification from the Tech Lead:
Offshore engineers are NOT expected to communicate directly with customers.
 Instead:

US-based Product Delivery team members act as the interface with customers.
Offshore engineers collaborate internally.
Engineers are encouraged to proactively ask questions and communicate with internal stakeholders whenever something is unclear.
Direct customer communication is optional rather than expected.

Ideal Candidate Profile
 The ideal candidate demonstrates:
 Technical

Strong Python expertise + FastAPI
Experience building APIs
Comfortable learning new integrations quickly
Familiarity with Agile development processes
Personal qualities (most important)

Self-starter
Comfortable making decisions independently
Thrives in ambiguity
Strong ownership mindset
Excellent communication skills
Curious and proactive
Comfortable working in a fast-paced environment
The Tech Lead emphasized that the hardest quality to find is someone who is comfortable working with ambiguity, rather than technical expertise.

Seniority

Senior-level should be - communication, autonomy, and ownership are essential
Strong Mid-level can be as well, but it should be a person with strong mid-level technical ability, but senior-level communication ad ownership


Time Zone

9:00 AM – 5:00 PM Eastern Time
Can consider EU or UA candidates who will work as we agreed before (till 8 pm EU or 9 pm UA)
But 1st priority - LATAM

Hiring Deadline:

Immediate hiring need
Potentially hiring up to 2 engineers


On the interview at the Tabs company, in general, they gave the algorithmic live-coding task (30 mins), sometimes some system-design questions (10-15 mins). But we haven't present any candidate yet, so I don't know how the 1st interview will looks like. I will share what they asked when we will have it.

Do you have any specific questions related to this position? :slightly_smiling_face:
We can also do the small call with you if you want, @Pawel Gorski can share more the minset they are looking for from technical pesrspective. Overall, it's a quick eng who is ready to work in fast-pace env and contribute very quickly. Pawel, feel free to add anything from your side.

And again, huge thank you for your contribution and your help with interviewing people for this position!


Hi guys,
I'd like to share a brief summary of the technical exercise from the interview to another team but for this project. So, it was smth like that:

The task involved comparing 2 arrays of objects and determining which items should be inserted, updated, or deleted.
A composite key had to be built using two fields to uniquely identify records across both lists.
The expected approach was to find differences efficiently using hash maps (Map) rather than nested loops.
The solution was expected to run in linear time O(N) (or O(N + M)) by leveraging constant-time lookups.
It was important to be careful when constructing map keys to avoid collisions and correctly handle updates.
Strong familiarity with arrays, hash maps, and common collection reconciliation patterns was beneficial.
So, hope it will be helpful for you and you can understand the level of complexity of the task on the client interview!