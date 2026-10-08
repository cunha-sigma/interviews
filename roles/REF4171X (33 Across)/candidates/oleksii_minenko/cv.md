# Oleksii Minenko — CV (parsed)

**Title:** Senior Machine Learning Engineer / Senior AI Engineer
**Location:** Kyiv, Ukraine
**Contact:** collinlikas@gmail.com · +380633327637

## Work Experience

**Machine Learning Engineer — DataZ** (Kyiv, Ukraine) — *Jan 2025 – Present*
- Designing large-scale data and ML pipelines for structured/unstructured OSINT data ingestion and feature extraction.
- Developing and evaluating custom LLM-based classifiers for identity-matching, credit-scoring, and content detection.
- Distributed data processing via PySpark; optimizing model training throughput on AWS.
- Building internal evaluation/monitoring frameworks to measure precision/recall and improve model performance over time.
- Collaborating with data engineers on schema design and data-quality consistency across pipelines.
- Fine-tuning LLMs and optimizing prompts using automated evaluation pipelines (e.g., MLflow), improving classification quality/consistency across OSINT tasks.
- Tech stack: Python, PySpark, Airflow, TensorFlow, Scikit-learn, AWS, Docker, LangChain, OpenAI API, MLflow, Gemini.

**Machine Learning Engineer — CaboodleAI** (Remote / Kyiv, Ukraine) — *Jan 2020 – Dec 2024*
- Developed/maintained production-grade ML pipelines for training, evaluation, and monitoring.
- Built and deployed a personal recommendation system, improving engagement/conversion metrics.
- Designed and managed Airflow DAGs for automated ingestion, feature generation, and retraining workflows.
- Integrated data flows from Google Analytics and internal databases for unified tracking/consistent model inputs.
- Supported/optimized existing ML services in production, improving latency and reliability.
- Collaborated cross-functionally to align ML outcomes with product/business objectives.
- Tech stack: Python, Airflow, TensorFlow, Scikit-learn, PySpark, AWS, Docker, SQL, Google Analytics API.

**Data Scientist — Jiji** (Kyiv, Ukraine) — *Aug 2018 – Aug 2022*
- Built recommendation systems, traffic control, and anti-fraud models for an online marketplace.
- Developed new features and contributed to model improvements across multiple business lines.

**Data Scientist — Active Wizards** (Kyiv, Ukraine) — *Jan 2018 – Aug 2018*
- Exploratory data analysis, prediction models, and R Shiny applications.
- R&D evaluating new ML methods/modeling techniques.

**Data Scientist — SynergyOne** (Kyiv, Ukraine) — *Jul 2017 – Jan 2018*
- SME credit-scoring project — regression and classification models.
- Feature engineering pipelines, clustering algorithms.
- Authored project documentation, contributed to model validation processes.

**QC Game Tester — Ubisoft Ukraine LLC** (Kyiv, Ukraine) — *Dec 2016 – Jul 2017*
- Tested game mechanics, created bug reports/documentation via Jira.

**Note — overlapping dates:** Jiji is listed as Aug 2018 – Aug 2022, which overlaps with Active Wizards (Jan–Aug 2018, adjacent not overlapping) but runs concurrently with most of CaboodleAI (Jan 2020 – Dec 2024). Worth a quick clarifying question — could be a part-time/transition overlap or a CV date error.

## Education

- M.Sc. in Actuarial and Financial Mathematics — Taras Shevchenko National University of Kyiv (2016)
- B.Sc. in Mathematics — Taras Shevchenko National University of Kyiv (2014)

## Skills

- **Programming & ML:** Python, SQL, Scikit-learn, TensorFlow, PySpark
- **Data Engineering:** Airflow, Docker, AWS, Data Wrangling
- **Machine Learning:** Recommender Systems, Classification, LLM Fine-tuning, Model Monitoring
- **Tools & Libraries:** Pandas, NumPy, LangChain, OpenAI API
- **Other:** Statistics, Feature Engineering, CI/CD for ML

## Languages

English – Fluent; Ukrainian – Native

## Notable for this role (REF4171X — Senior MLOps / ML Platform Engineer)

This role is platform/infrastructure-engineering-heavy (training orchestration, model registry with audit gates, multi-tenant isolation, Kubernetes, Terraform, ML-specific monitoring) rather than modeling-focused. Oleksii's CV reads as a strong **ML engineer / data scientist** — recommenders, classifiers, LLM fine-tuning, precision/recall evaluation frameworks — but has real gaps against this specific job description:

- **No Kubernetes, Docker orchestration depth, or Terraform/IaC** mentioned anywhere — Docker is listed but only as a tech-stack bullet, not described as something he operates (jobs/cronjobs, debugging pods, resource limits).
- **No model registry / versioning / lifecycle tooling** named (no MLflow model registry, Vertex AI Model Registry, or equivalent registry-with-promotion-gates experience — MLflow appears only as an "automated evaluation pipeline" tool, not registry/lifecycle).
- **No multi-tenant or per-customer model isolation** experience evidenced — directly named in the job description as "strongly preferred" and something he'd need to "have had to prove."
- **No explicit GCP experience** — CV is AWS-only; job description states "Cloud (GCP preferred)."
- Does show real, relevant adjacent strength: production ML pipelines "kept running" (not just deployed once) across two multi-year roles, Airflow DAG ownership (orchestration, retraining workflows — relevant to backfills/idempotency questions), and built internal evaluation/monitoring frameworks (relevant to the job description's drift/monitoring responsibilities, though framed as model-quality monitoring rather than full ML-system observability).

Net read: worth interviewing, but this is a genuine profile mismatch to probe carefully — is his experience closer to "the data scientist whose code the platform engineer has to production-harden" or has he actually done the platform-engineering side himself? The interview should test this distinction directly rather than assume it from the CV.
