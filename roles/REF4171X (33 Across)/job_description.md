# Senior MLOps / ML Platform Engineer

## Company Description

### Customer
A technology company operating supply-side infrastructure in the programmatic advertising ecosystem — a large ad exchange handling on the order of hundreds of millions of auction requests per day. The company is building an in-house predictive decisioning capability and has engaged Sigma Software to design, build and hand over the platform behind it.

### Project
Join a dedicated Sigma Software team building a predictive modeling and optimization platform on top of a live ad exchange. The platform scores and filters supply in real time, estimates which contexts over-index for an advertiser’s outcomes, builds look-alike audiences from small seed lists, and optimises all of it against several business objectives at once under explicit constraints.

### Job Description
- Build training orchestration across the hourly, daily and weekly planes, with retries, backfills and idempotency
- Stand up the model registry: versioning, lineage, evaluation gates and a promotion path a person can audit
- Implement per-advertiser model instances with namespace and configuration isolation, and produce the isolation evidence the compliance review needs
- Build the refresh pipelines and the publish path to the serving store, with freshness SLOs and the documented fallback behaviour on failure
- Implement shadow mode and champion/challenger mechanics, including the traffic and logging hooks the measurement pipeline depends on
- Build monitoring for the ML system specifically: feature drift, prediction drift, train/serve skew, calibration decay, label arrival delay — with thresholds that page someone
- Guarantee reproducibility: pinned environments, container builds, data snapshots, and the ability to rebuild any served model from its recorded inputs
- Instrument the cost of training and hourly scoring per tenant, because the client will ask what it costs to run before committing to phase two
- Work with the DevOps/SRE engineer on the promotion pipeline rather than duplicating it
- Write the operational runbooks and hand the platform to the client’s engineers

## Qualifications

### Must-have qualifications
- 4+ years in MLOps, ML platform or platform engineering, with production ML you kept running — not deployed once
- Strong Python: you write the platform, not only its YAML
- Kubernetes and Docker hands-on — resource limits, jobs and cronjobs, and debugging a pod that will not start
- CI/CD for ML workloads: automated training, testing and promotion, with no manual notebook-to-production step
- Model registry, versioning and lifecycle management (MLflow, Vertex AI Model Registry or equivalent)
- Orchestration of scheduled, dependent jobs (Airflow or Cloud Composer, Kubeflow, Argo, Vertex Pipelines) including backfills and idempotent reruns
- Monitoring and observability of ML systems — drift and skew as well as ordinary service metrics
- Cloud (GCP preferred), infrastructure-as-code, and solid Linux
- A working understanding of the ML lifecycle: you can read a data scientist’s training code and tell what would break in production
- Upper-Intermediate (B2) English or higher

### Strongly preferred
- Vertex AI: Pipelines, Training, Model Registry, Feature Store
- MLflow or Kubeflow in anger, including their operational rough edges
- Feature stores and keeping training and serving features consistent
- Multi-tenant or per-customer model isolation, and having had to prove it
- Large-scale batch scoring under a freshness SLO
- Experiment tracking platforms and model evaluation gates
- Drift detection in practice — including what you did when it fired
- Terraform

### Will be a plus
- On-prem Kubernetes or bare-metal Linux alongside cloud
- Data versioning (DVC, lakeFS, warehouse snapshots)
- Low-latency key-value serving stores (Bigtable, Redis, Aerospike)
- GPU scheduling and cost control on training workloads
- Audit evidence for SOC 2, ISO 27001 or GDPR-flavoured reviews

