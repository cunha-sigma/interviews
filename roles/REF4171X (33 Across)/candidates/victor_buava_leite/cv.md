# Victor Buava Leite — CV (parsed)

**Title:** MLOps Engineer / ML Engineer
**Location:** São Paulo, Brazil
**Contact:** victorbleite@outlook.com · linkedin.com/in/lbvictor

## About

5+ years in Data Science, ML Engineering, and MLOps. Focus on architecting and implementing cloud-based MLOps solutions for end-to-end machine learning model lifecycle management.

## Experience

**MLOps Engineer — GCB Investimentos** (São Paulo, Brazil) — *Oct 2023 – Oct 2025*
GCB Investimentos: Brazilian financial markets holding company, 16+ years, 200,000+ clients, R$3B+ transaction volume. Responsible for transitioning ML engineering capabilities into a full ML platform approach across the group's companies.
- Led technical development of MLOps solutions focused on reliability, cross-functional collaboration on deployment/monitoring/governance processes.
- Architected/implemented end-to-end solutions on AWS (SageMaker + in-house tools).
- Built/managed ML platform infrastructure as code (AWS, GitHub Actions, Terraform).
- Implemented a **Feature Store** for consistent feature management.
- Implemented a **Model Registry** as centralized lifecycle manager — production variants, shadow variants, A/B testing, model versioning, auto-retraining.
- Developed a custom **ML Deployment Service** orchestrating rollouts across real-time, serverless, and batch deployment strategies.
- Built a custom **Ground Truth Service** for data labeling.
- Designed/deployed a **Model Performance Monitoring System** (evaluation metrics, proactive governance).
- Implemented a **Data Drift Detection Service** (feature distribution shift monitoring).

**Machine Learning Engineer — PeerBR** (integrated into GCB Investimentos, São Paulo) — *Mar 2025 – Oct 2025*
Tokenized private-credit/crowdfunding platform, 200k+ clients, 400k+ transactions, R$771M+ volume. Implemented the company's first ML models — batch investment recommendations and qualified lead generation.

**Machine Learning Engineer — Adiante Recebíveis** (São Paulo, Brazil) — *Mar 2023 – Oct 2025*
SME invoice-financing fintech, 15-second credit analysis, R$900M+ financed, 150k+ users. Built and led the ML Engineering function from prototype to production.
- Drove alignment between technical/product teams across the ML workflow.
- Managed the full ML model lifecycle: training, retraining, deployment, monitoring.
- Implemented **CI/CD pipelines** to operationalize experimentation and promote validated models to production.
- Built automated feature, training/retraining, deployment, and inference pipelines.
- Managed code/data/pipelines/models for reproducibility, versioning, governance.
- Served models across real-time, serverless, and batch architectures.
- Implemented **end-to-end test suites** for ML systems.
- Implemented functional/operational monitoring across APIs, training pipelines, inference pipelines, model endpoints.

**Data Scientist — Adiante Recebíveis** (credit fintech of GCB Investimentos) — *Sep 2022 – Mar 2023*
ML models for credit applications, feature engineering through production deployment.

**Data Scientist — Comunidade DS** (São Paulo, Brazil) — *Aug 2021 – Sep 2022*
Data science school/consulting firm — ML models for varied business use cases in project-based environments.

**Operational Analyst — Neoenergia** (Atibaia, São Paulo, Brazil) — *Feb 2014 – Apr 2020*
Data analysis for voltage meters, third-party contract management, engineering-project tracking/reporting — pre-ML career.

**Note on overlapping dates:** GCB Investimentos (Oct 2023–Oct 2025), PeerBR (Mar–Oct 2025), and Adiante Recebíveis ML Engineer (Mar 2023–Oct 2025) overlap substantially. Unlike some other candidates' unexplained CV overlaps seen in this project, this one is self-explained in the text — PeerBR is explicitly "integrated into GCB Investimentos" and Adiante is explicitly "Credit Fintech of GCB Investimentos," so these read as concurrent responsibilities across one corporate group's related entities rather than an inconsistency. Still fine to confirm directly in conversation.

## Education

- MBA in Machine Learning — Institute of Management and Information Technology (IGTI), Minas Gerais, 2022–2023
- Associate's degree in Financial Management — City University of São Paulo (UNICID), 2016–2018

## Communities & Memberships

Tech Leads Club (senior eng/leadership), ScoraS Academy (AI engineering/agents), Jornada de Dados (data engineering/platforms), MLOps Community, Feature Store for ML.

## Languages

Brazilian Portuguese (Native), English (Professional working proficiency)

## Notable for this role (REF4171X — Senior MLOps / ML Platform Engineer)

Exceptionally strong on-paper match — his own CV language mirrors this role's job description almost point for point: a model registry with production/shadow variants and A/B testing (directly matches the JD's registry + shadow-mode/champion-challenger asks), a custom deployment service spanning real-time/serverless/batch (matches the refresh/publish-path requirement), a drift-detection service, a performance-monitoring system, IaC with Terraform, CI/CD, and end-to-end test suites. Given how closely the wording tracks the JD's own phrasing, it's worth treating this as a strong signal to verify in depth during the interview — confirm he can speak concretely about *how* these systems actually worked, not just recite what's on the page.

Gaps/open questions relative to the JD:
- **No Kubernetes mentioned anywhere.** Deployment work is described via SageMaker + a custom deployment service — worth directly asking whether any of this ran on Kubernetes, or whether it was entirely SageMaker-managed infrastructure (a real but different skill set from hands-on K8s/Docker work).
- **AWS-only — no GCP experience mentioned**, though the JD states GCP only as "preferred," not required.
- **No explicit mention of multi-tenant/per-customer isolation** or compliance/audit evidence specifically, despite deep registry/governance experience generally — worth probing whether GCB's platform served multiple business units with isolation guarantees, given the parent company's multiple related entities.
- **Cost instrumentation per tenant** not explicitly mentioned — worth asking directly.
