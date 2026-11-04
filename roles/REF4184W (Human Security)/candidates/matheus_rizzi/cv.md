# Mateus Matos Rizzi — CV (parsed)

**Title:** Staff Data Engineer
**Location:** São Paulo, Brazil (available 100% remote)
**Contact:** mateusmrizzi@hotmail.com · (11) 97487-3855 · linkedin.com/in/mateus-matos-rizzi

## Summary

Staff Data Engineer with 8+ years of end-to-end experience architecting and delivering scalable data and reporting platforms. Deep expertise in Snowflake (modeling, role-based access control, data sharing, performance optimization), dbt (a SQL-based data-transformation tool: transformations, tests, lineage), Power BI (semantic layer design, embedded analytics), and the Azure ecosystem (Databricks, Azure Data Lake Storage Gen2, Azure Data Factory). Experience bridging upstream data platforms with downstream BI consumption layers using a layered data-architecture pattern (raw → cleaned → business-ready, sometimes called "Bronze → Silver → Gold"). Experienced with Kafka/change-data-capture, Apache Iceberg (a table format for large analytic datasets), Apache Airflow, and REST/GraphQL APIs. Track record of mentoring engineers and driving data governance standards. Holds an MBA in Data Science (University of São Paulo). Fluent in English.

## Experience

**Coforge (formerly Encora) — AI / Data Tech Lead, Staff Data Engineer** (remote, international clients: USA, India) — *Feb 2025 – Present*
- Architected a layered data lake on Databricks Delta Lake and Azure (Azure Data Lake Storage Gen2 / Azure Data Factory) with governance, lineage, and quality controls, feeding Power BI and API-driven reporting.
- Designed scalable dbt data models on Snowflake and Databricks — reusable, tested, documented transformations powering dashboards and self-service BI.
- Owned the Snowflake platform: access-control design, query performance tuning, data-sharing configuration, monitoring.
- Designed the Power BI semantic (consumption) layer for Snowflake-to-Power-BI reporting.
- Implemented data quality/observability frameworks (dbt tests, data freshness monitors, pipeline alerting via Airflow).
- Built AI-powered data product features (LangChain agents, text-to-SQL) bridging natural-language queries to Snowflake and reporting layers.
- Mentored 4–6 engineers in data modeling, Snowflake performance tuning, dbt practices, and BI system design.
- Describes operating with full autonomy across ambiguous, cross-functional environments, aligning Data Platform, Product, and Analytics teams.

**Mercado Eletrônico — AI / Data Tech Lead, Senior Data Engineer** — *Apr 2021 – Feb 2025*
- Built and operated the company's core dbt + Snowflake analytics platform, modeling complex B2B marketplace business rules as tested, documented SQL transformations serving Power BI and Looker.
- Designed GCP data pipelines (Dataflow, Dataform, Cloud Composer/Airflow) and Kafka/change-data-capture integrations for real-time ingestion into Snowflake.
- Implemented data governance standards: column-level lineage, dbt documentation, access controls, data quality validation.
- Built Python-based REST API integrations, consuming upstream data-sharing endpoints for reporting and client-facing products.
- Resolved a production performance issue: identified missing primary keys causing full-table scans, redesigned the extraction strategy, cut pipeline duration from 4 hours to 1 hour.
- Led a team of 4–6 data engineers, mentoring on data modeling, Snowflake performance, and BI practices.

**Sysvision International — Data Engineer** — *Feb 2020 – Mar 2021*
- Built REST/SOAP API integrations and SAP HANA data pipelines for Bayer (Life Sciences), in a regulated environment.
- Designed Hadoop/Hive-based data flows and Azure-compatible storage patterns for international client projects.

**ADP — Data Engineer** — *Jun 2019 – Jan 2020*
- Built and maintained HR/payroll/benefits data pipelines using Talend Data Integration and Python, working with Workday HR system data.
- Automated rule-based financial benefits processing logic, improving pipeline reliability and data accuracy.

**Deal Group — Data Analyst** — *Feb 2019 – Jun 2019*
- Designed data flows from SAP HANA to SQL Server using SSIS and Pentaho, built data models for operational reporting.

**Gol Airlines — BI Intern** — *Sep 2018 – Feb 2019*
- Developed QlikSense dashboards and maintained data integration routines for operational analytics.

## Skills

**Data platform & modeling:** Snowflake (modeling, access control, data sharing, performance tuning), dbt (transformations, tests, documentation, lineage), Great Expectations/dbt tests (data quality frameworks), star schema, layered/"Medallion" architecture, Apache Iceberg, semantic layer design for Power BI.

**Programming & APIs:** Python (8+ years — PySpark, pandas, scripting, automation), SQL (advanced — window functions, common table expressions, optimization), REST/GraphQL APIs (build and consume), JavaScript (familiarity), Git/GitHub Actions CI/CD.

**Cloud & orchestration:** Azure (Databricks, Azure Data Lake Storage Gen2, Azure Data Factory — primary cloud), Apache Airflow (6 years production orchestration), Kafka/Debezium (change-data-capture ingestion), Apache Iceberg (table management, time travel), AWS (Redshift, S3, Glue, Athena), GCP (BigQuery).

**BI, governance & leadership:** Power BI (semantic models, DAX, embedded analytics), Looker/Amazon QuickSight, data governance/quality/observability frameworks, team mentorship (4–6 engineers), fluent English for international remote teams.

## Certifications

- Claude Certified Architect — Anthropic (valid June–December 2026)
- LangChain Agents Certification — LangChain

## Education

- MBA in Data Science — Universidade de São Paulo (USP) (Feb 2020 – Jan 2021)
- B.Eng. in Information Engineering — Universidade Federal do ABC (UFABC) (Sep 2014 – May 2019)

## Notable for this role (REF4184W — Senior Data Scientist, Analytics/Insights track, non-ML)

His technical profile is a different shape than the three prior candidates for this role: he's a data **engineering** platform specialist (Snowflake, dbt, Azure, orchestration, BI semantic layers) rather than an analytics/data-science investigator. Strong on the explicit Python and SQL requirements, and the JD's "strong engineering understanding, able to read/reason about complex logic/code" maps reasonably well to his dbt-transformation and data-modeling work. JavaScript is listed as "familiarity," matching the JD's own bar exactly.

Gaps/open questions relative to the JD:
- **No rule-based detection, cybersecurity, or fraud/risk-analytics-system experience** anywhere in the CV — same gap as every candidate seen so far for this role.
- **Profile reads as data-platform/infrastructure engineering, not investigative analytics** — his achievements are about building and operating pipelines/platforms (Medallion architecture, RBAC, orchestration) rather than forming hypotheses and investigating ambiguous problems in existing data, which is this role's actual core ask. The one clear exception is the Mercado Eletrônico production-performance story (found missing primary keys causing full-table scans, 4 hours → 1 hour) — a genuinely concrete root-cause example, worth making the anchor for the deep-dive.
- **Heavy people-management/tech-lead thread** in his two most recent, most senior roles (mentoring 4–6 engineers, "AI/Data Tech Lead" titles) — similar consideration to Denis Kravchenko; worth directly checking he wants a hands-on individual-contributor role on a newly forming team, not a continuation of a tech-lead/management trajectory.
- **"Full autonomy across ambiguous, cross-functional environments"** is asserted directly in his own bullet points — worth testing this claim specifically rather than taking it at face value, same as with any candidate's self-description.