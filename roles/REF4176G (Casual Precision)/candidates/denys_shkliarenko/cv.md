# Denys Shkliarenko — CV (parsed)

**Title:** ML Engineer | MLOps | Agentic Systems
**Location:** Spain
**Contact:** shkliarenko@gmail.com · linkedin.com/in/shkliarenko

## Summary

ML Engineer with 5+ years delivering production-grade ML systems, LLM-powered services, and scalable MLOps infrastructure. End-to-end pipelines (data ingestion → training → deployment → monitoring) across on-prem, GCP, and AWS. Hands-on with agentic AI systems, RAG pipelines, multi-modal LLMs, API-driven microservices. CI/CD automation, containerization, cloud-native deployment.

## Technical Skills

- **Languages:** Python, SQL
- **ML & Data:** PyTorch, Scikit-learn, Pandas, NumPy, Dask, PySpark
- **LLM & GenAI:** Anthropic API, OpenAI API, Google Gemini, LiteLLM, HuggingFace, RAG, Agentic Pipelines
- **MLOps:** DVC, Airflow, Kubeflow, Vertex AI Pipelines, GitHub Actions, Docker, Kubernetes, Helm, Terraform
- **Cloud:** GCP (Vertex AI, BigQuery, GCS, Artifact Registry), AWS (S3, EC2, ECR)
- **APIs & Serving:** FastAPI, Asyncio, Streamlit, Gradio
- **Monitoring:** ELK, Prometheus, Grafana, GCP Monitoring
- **Testing:** PyTest, Unittest

## Professional Experience

**ML Engineer — Ecentria Inc. (OpticsPlanet.com)** — *Feb 2021 – present*

*ML Engineering & MLOps:*
- Recommendation system (LightFM, BERT4Rec, FAISS, FastAPI) — real-time personalized suggestions; feature engineering, experimentation, CI/CD; confirmed CTR improvement via A/B tests.
- MLOps role on demand forecasting platform (LightGBM, Dask, Airflow, DVC); CI/CD, Docker image optimization, led migration of distributed workloads GCP→AWS (contributed to dask-cloudprovider OSS library).
- Fine-tuned/deployed transformer LLMs (HuggingFace, Vertex AI) with DVC-based reproducible versioning.
- Built/maintained CI/CD for ML systems (GitHub Actions, Docker, Kubernetes); Terraform infra automation across GCP/AWS.
- Monitoring/logging for deployed models (Elasticsearch, Kibana, GCP Monitoring, Prometheus, Grafana).

*LLM & Agentic Systems:*
- FastAPI microservice for ID document validation using multimodal LLMs (via LiteLLM) — OCR field extraction, fuzzy name matching, PII-safe handling.
- Production pipelines for LLM-based product spec enrichment with web grounding, async fan-out, checkpointing.
- Agentic AI pipeline (Google ADK) autonomously researching product categories, generating citation-backed HTML articles (Vertex AI Search + live web retrieval).
- Document processing service (AWS Lambda, Step Functions, Anthropic API, Amazon Textract) — OCR, classification, extraction, validation; refactored polling-based OCR flow to event-driven SQS callback pattern, reducing latency and cost.
- Built agentic AI tooling layer for Claude Code for the data analytics team, standardizing ad-hoc analysis/reporting.

*Software & API Development:*
- Internal Python libraries and FastAPI microservices for ML model serving/automation.
- A/B testing analysis platform (Streamlit, BigQuery, statsmodels, custom bootstrap engine) — 1-day result latency.
- Cost-efficient GrowthBook query layer (Terraform, BigQuery, GA4) — 20x reduction in BigQuery processing cost for A/B analytics.

**Data Analyst — Ecentria Inc. (OpticsPlanet.com)** — *Feb 2018 – Jan 2021*
- BI reports/dashboards (PowerBI, SQL, SSRS).
- Quantitative analysis on large datasets for business strategy.
- Partnered with data engineers on pipeline optimization.

**Financial Controller — RISOIL S.A.** — *2017–2018*
**Financial Controller / Manager — KONTI** — *2004–2014*

## Education

- MSc, Engineering and Economics — Odesa National University of Technology, Ukraine, 1999–2004

## Languages

English (Advanced), Ukrainian (Native)

## Notable for this role (REF4176G)

- Very strong, direct fit: production LLM pipelines (Anthropic/OpenAI/Gemini), document/OCR extraction with Textract + Anthropic API, RAG, agentic systems, and explicit event-driven refactor for latency/cost — closely mirrors the JD's described work.
- Strong MLOps depth (Airflow, Kubeflow, Terraform, Docker/Kubernetes, CI/CD) and both AWS and GCP hands-on experience.
- A/B testing platform + statistically rigorous bootstrap engine — directly relevant to the JD's "offline evaluation metrics, holdout validation" requirement.
- SQL listed as a core language skill, though depth of usage in the experience bullets is mostly BI/dashboard-oriented (earlier Data Analyst role) rather than shown deeply in the recent ML work — worth a direct SQL question regardless.
- Non-linear career path (Financial Controller roles 2004–2018 before Data Analyst/ML Engineer) — not a concern given the technical depth since, but worth being aware of when framing "years of ML/DS experience" against the JD's 5-year requirement (technical experience reads as roughly Feb 2018–present, ~8 years across Data Analyst + ML Engineer).
