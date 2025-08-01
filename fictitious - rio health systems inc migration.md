Rio Health Systems, Inc.—AWS Migration & Modernization Program
Comprehensive Technical Pitch (Rev 3)
Prepared for the CTO Cohort, Private Equity Investment Board — 30 July 2025

---

# Table of Contents

1. Executive Overview
2. Investment Thesis & Strategic Context
3. Guiding Principles
4. Current‑State Assessment
5. Target‑State Architecture
6. DevSecOps Platform
7. Security, Compliance & Governance
8. Migration Strategy & Ten‑Year Timeline
9. Financial Model & ROI Analysis
10. Key Performance Indicators
11. Risks & Mitigations
12. Organizational Change & Training
13. Conclusion & Board Decision

---

## 1 Executive Overview

Rio Health Systems ("Rio") operates nine mission‑critical workloads in two leased data‑centers (Dallas & North Carolina) that expire within 36 months. A staged migration to Amazon Web Services (AWS) retires \$4.1 M / yr OPEX, avoids \$12 M CapEx, and achieves HIPAA, SOC 2, and ISO 27001 compliance. The 36‑month program establishes a Well‑Architected landing zone, Infrastructure‑as‑Code (IaC), Continuous‑Integration/Continuous‑Delivery (CI/CD), and automated evidence collection.

**Investment ask:** \$28.6 M (labor \$12.4 M @ \$200 / h; cloud & tools \$16.2 M) **Return:** 5‑yr NPV + \$14.8 M, IRR 22 %, break‑even month 35.

---

## 2 Investment Thesis & Strategic Context

• Retire expiring leases and obsolete hardware, eliminating 35 % of annual IT OPEX.
• Enable elastic scale, zero‑trust security, and data‑driven innovation on AWS.
• Create an M\&A‑ready “factory” that onboards bolt‑ons in < 30 days.
• Improve EBITDA by \$2.8 M / yr, lifting enterprise value by \$22 M at an 8× multiple.

---

## 3 Guiding Principles

1. AWS Well‑Architected reviews every six months.
2. Regulated‑cloud controls codified in IaC (HIPAA §164, SOC 2 TSP 100, ISO 27001 Annex A).
3. Twelve‑Factor micro‑services on Amazon EKS and AWS Lambda.
4. Terraform 1.8 + AWS CDK; no console drift.
5. GitLab → OIDC → multi‑account CI/CD; DORA metrics exported.
6. Continuous compliance via AWS Config Conformance Packs & Audit Manager.

---

## 4 Current‑State Assessment

| ID | Application    | Users  | Data           | Key Issues                    | Lease Risk  | Target Pattern           |
| -- | -------------- | ------ | -------------- | ----------------------------- | ----------- | ------------------------ |
| A1 | SAP ERP        | 3 400  | 5 TB           | 4 h backup, no DR             | Dallas & NC | EC2 lift→S/4 HANA        |
| A2 | B2B e‑Commerce | 18 000 | 2 TB           | Sticky sessions, 4 h releases | Dallas      | EKS + Aurora MySQL       |
| A3 | Marketing      | 9 500  | 10 TB          | 27 m cold starts, TLS drift   | Dallas      | .NET 8 on EKS, Aurora PG |
| A4 | CRM (Sugar)    | 850    | 100 GB         | EOL OS, no MFA                | Dallas      | ECS → SaaS option        |
| A5 | Logistics      | 1 200  | 100 GB + files | File‑lock outages             | Dallas      | EKS, S3 events           |
| A6 | Analytics DW   | 600    | 100 + TB       | 3 h ETL, 90 % util            | NC          | Redshift lakehouse       |
| A7 | Recruiting     | 200    | < 1 GB         | No SSL                        | NC          | App Runner               |
| A8 | Scheduler      | 80     | < 1 GB         | Hard‑coded hosts              | Dallas      | Lambda + DynamoDB        |
| A9 | News & Events  | Public | < 1 GB         | Unpatched CVEs                | Dallas      | Amplify static           |

---

## 5 Target‑State Architecture

• **Landing Zone & Org:** AWS Control Tower with Foundation, Workloads‑Prod, Workloads‑NonProd OUs, SCP guard‑rails, central audit account.
• **Network:** Hub‑and‑spoke VPCs via AWS Transit Gateway; 10 Gbps DX during coexistence; Gateway Load Balancer for IDS/IPS.
• **Identity:** Azure AD federation through IAM Identity Center; ABAC tagging; JIT privileged roles.
• **Data Layer:** Aurora MySQL/PG global clusters, Redshift RA3 lakehouse, S3 Iceberg tables, SAP on HANA Large Instances.
• **Observability:** ADOT collectors → X‑Ray, CloudWatch RUM; metrics to Amazon Managed Grafana.
• **DR & Resilience:** Multi‑AZ everywhere; pilot‑light third region; Backup Vault lifecycle to Glacier Deep Archive.

---

## 6 DevSecOps Platform

| Domain        | Capability         | AWS Services                          | Tooling            |
| ------------- | ------------------ | ------------------------------------- | ------------------ |
| IaC           | Immutable infra    | Terraform Enterprise, CDK             | Checkov, Atlantis  |
| CI            | Build & test       | GitLab runners on Fargate Spot        | SonarQube, Trivy   |
| CD            | Progressive deploy | ArgoCD (EKS), CodeDeploy (EC2/Lambda) | Argo Rollouts      |
| Security      | Shift‑left scans   | Secrets Manager rotation              | OWASP ZAP, SLSA L2 |
| Observability | SLO dashboards     | ADOT → Grafana                        | FinOps rightsizer  |

---

## 7 Security, Compliance & Governance

| Framework        | Control Theme       | AWS Implementation                              | Evidence              |
| ---------------- | ------------------- | ----------------------------------------------- | --------------------- |
| HIPAA            | Encryption & access | KMS CMK, S3 Block Public, ALB + WAF             | Audit Manager BAA     |
| SOC 2            | Monitoring & change | Config rules, Security Hub                      | SOC 2 template        |
| ISO 27001        | Ops security        | SSM Patch Manager, OpsCenter                    | Conformance Pack      |
| Well‑Architected | Cost & reliability  | Trusted Advisor, WAR tool                       | WAR reports           |
| 12‑Factor        | Disposable services | EKS/Fargate, externalised config                | OPA tests             |
| IaC & CI/CD      | Drift & provenance  | Encrypted Terraform state, Cosign image signing | Sigstore attestations |

Governance includes weekly steering "flash" reports, quarterly W‑A checkpoints, and milestone funding gates.

---

## 8 Migration Strategy & Ten‑Year Timeline

### 8.1 Gantt Summary (Years 1‑4)

| Year | Qtr | Wave                         | Workloads                 | Compliance Gate      |
| ---- | --- | ---------------------------- | ------------------------- | -------------------- |
| 1    | Q1  | Foundation                   | Landing Zone, DX          | ISO 27001 Stage 1    |
| 1    | Q2  | Wave A                       | CRM, Recruiting           | HIPAA pre‑assessment |
| 1    | Q3  | Wave A Finish                | Scheduler                 | SOC 2 design freeze  |
| 1    | Q4  | SAP DR                       | DR pilot us‑east‑2        | ISO 27001 Stage 2    |
| 2    | Q1  | SAP Prod                     | ECC cut‑over              | W‑A Review #1        |
| 2    | Q2  | S/4 HANA PoC                 | X2iedn cluster            | HIPAA validation     |
| 2    | Q3  | Wave B                       | B2B, Marketing            | PCI DSS 4.0 interim  |
| 2    | Q4  | Analytics                    | Redshift lakehouse        | SOC 2 Type I         |
| 3    | Q1  | DC Exit (Dallas)             | Shutdown                  | ISO surveillance #1  |
| 3    | Q2  | AI/ML Layer                  | SageMaker                 | HIPAA audit pass     |
| 3    | Q3  | S/4 HANA Prod                | Cut‑over                  | SAP audit            |
| 3    | Q4  | Cost Opt #1                  | Graviton                  | W‑A Review #2        |
| 4    | All | Continuous WAFR & Compliance | Containers 2.0, FinOps v2 | SOC 2 Type II        |

Detailed ten‑year outlook continues with edge compute pilots, serverless shift, and ISO recertification cycles.

---

## 9 Financial Model & ROI Analysis

| Lever                | Annual Δ      | Impact                        |
| -------------------- | ------------- | ----------------------------- |
| Data‑centre exit     | −\$4.1 M      | Lease & power elimination     |
| Cloud right‑sizing   | −\$0.33 M     | 20 % waste reduction          |
| License optimisation | −\$0.09 M     | Retire SQL EE cores & Jenkins |
| Feature velocity     | +\$0.30 M     | 0.75 % conversion uplift      |
| ML‑driven upsell     | +\$0.70 M     | 2 % attach improvement        |
| **EBITDA delta**     | **+\$2.82 M** | Steady‑state Year 4           |

5‑yr NPV + \$14.8 M (12 % discount); IRR 22 %. Sensitivity @ ±10 % cloud price: NPV range \$13.1 M‑\$16.5 M, IRR ≥ 19 %.

---

## 10 Key Performance Indicators

| KPI                | Y0     | Y1     | Y2     | Y3     | Y5 Target |
| ------------------ | ------ | ------ | ------ | ------ | --------- |
| Deploy frequency   | 6 / yr | 24     | 50     | 100    | 120 +     |
| Lead‑time          | 7 d    | 8 h    | 1 h    | 15 m   | < 5 m     |
| MTTR               | 10 h   | 2 h    | 45 m   | 20 m   | < 10 m    |
| Change failure     | 15 %   | 8 %    | 4 %    | 2 %    | < 1 %     |
| Mean query runtime | 15 m   | 5 m    | 2 m    | 1 m    | 30 s      |
| Security incidents | 5      | 3      | 1      | 0      | 0         |
| Unit cost / txn    | \$0.17 | \$0.12 | \$0.08 | \$0.06 | \$0.05    |

---

## 11 Risks & Mitigations

| Risk              | Likelihood | Impact | Mitigation                             |
| ----------------- | ---------- | ------ | -------------------------------------- |
| Data sovereignty  | Med        | High   | Region restrict, CMK per region        |
| Cut‑over downtime | Med        | High   | Blue‑green, DMS CDC                    |
| Skill gaps        | High       | Med    | AWS training, partner co‑delivery      |
| Cost overrun      | Med        | Med    | Budgets, auto‑stop, FinOps guard‑rails |
| Legacy ABAP       | Low        | High   | Code scan PoC, strangler pattern       |

Residual risks tracked in Jira with SLA owners.

---

## 12 Organizational Change & Training

* **Cloud CoE:** 8‑person cross‑functional squad.
* **Training:** 1 200 h AWS instructor‑led sessions; quarterly game‑days.
* **Pair‑Programming:** Partner engineers paired with Rio staff; knowledge in IaC repos.
* **SRE Model:** Follow‑the‑sun pods; \$65 / incident budgeted.

---

## 13 Conclusion & Board Decision

A 36‑month, five‑wave migration creates a secure, compliant AWS foundation that exits expiring data‑centres, reduces costs, and fuels data‑driven growth. Approving **\$28.6 M** now secures AWS credits, mitigates lease risk, and positions Rio for acquisitive expansion.

**Board action requested:** Authorise Phase 0 funding and schedule program kick‑off within 30 days.

---

## Appendix — Detailed Technical Recommendations by Workload

### A1 SAP ERP (Oracle 18c → S/4 HANA)

* **Phase 0 – Discovery** – Run SAP EarlyWatch, Oracle AWR; capture I/O, CPU, memory baselines for r7i sizing.
* **Re‑host** – Lift Oracle 18c to **Amazon RDS Custom** (Multi‑AZ, gp3 9000 Piops) with AWS DMS‑CDC for minimal cut‑over.
* **Re‑platform** – Migrate application tier to **EC2 Auto Scaling** (ASCS + ERS cluster) behind **NLB**; shared file‑systems on **FSx ONTAP**; backups via **Backint Agent** to S3 Vault Lock.
* **Refactor** – Execute **S/4 HANA RISE** PoC (X2iedn mem‑optimized nodes) in Year 2; integrate **SAP BTP** services for Fiori apps.
* **CI/CD** – Git‑based ABAP transports; quality gates on ATC; deployments via **AWS Systems Manager**.
* **Security & Compliance** – KMS CMK‑SAP, IAM fine‑grained roles, GuardDuty suppression for RFC ports; aligns with HIPAA 164.312(e)(1), SOC 2 CC6.8, ISO 27001 A.12.3.
* **DR** – Cross‑Region RDS read‑replica (us‑east‑1 ↔ us‑west‑2), **AWS Elastic Disaster Recovery** for EC2 cluster; RPO < 5 min, RTO < 60 min.

### A2 B2B E‑Commerce (Java / MySQL)

* **Containerize** Tomcat WAR into distroless image; store config in **SSM Parameter Store**.
* **Database** – Convert to **Amazon Aurora MySQL v3 Global** (storage‑autoscaling, parallel query) with **Aurora Global Database** for DR.
* **Compute** – Deploy to **Amazon EKS** managed node groups (Graviton c7g) with **Karpenter** autoscaling; **SQS FIFO** queue decouples order events.
* **Edge** – **CloudFront** + **AWS WAF Bot Control**; PCI SAQ A scope limited to ALB/CloudFront.
* **Deploy** – **Argo Rollouts** progressive (5 %‑50 %) canary; metrics gate (error < 0.3 %).
* **Compliance** – SOC 2 CC7.2 network security; ISO A.14.2.5 secure dev; PCI DSS 4.0 segmentation.

### A3 Marketing Platform (.NET / SQL Server)

* **Re‑compile** APIs to **.NET 8**; publish as Linux containers (Alpine) to enable Graviton savings.
* **Database** – Use **Aurora PostgreSQL + Babelfish** for phased cut‑over from SQL Server Always On.
* **Front‑end** – Host Angular SPA on **S3 + CloudFront** with **Lambda\@Edge** security headers (HSTS, CSP).
* **Mobile CI** – **Expo EAS** builds in CodeBuild; OTA via CodePush.
* **Security** – **ACM** auto‑renew TLS, **Shield Advanced** for DDoS, **AWS Config** TLS rule.
* **Performance** – TTFB drop target < 80 ms; enable **CloudFront Edge Cache** policies; warm‑pool nodes for cold‑start mitigation.

### A4 CRM (Sugar PHP)

* **Lift‑and‑Shift** to **Amazon Lightsail Container Service** (short‑term) for rapid exit.
* **Refactor Option** – Containerize to **ECS Fargate** with NeptuneDB for graph relationships; evaluate SaaS migration (Salesforce) in Year 4 decision gate.
* **Database** – Upgrade to **RDS MySQL 8** with Performance Insights; enable **IAM‑DB Auth**.
* **Security** – **IAM Identity Center** MFA, **SSM Patch Manager** automated.
* **Compliance** – HIPAA 164.312(a)(2) unique IDs; SOC 2 CC6.3 authentication; ISO A.9.2.3 user access.

### A5 Logistics Manager (Java / MySQL + File‑Share)

* **Phase 1** – **EFS‑IA** storage migration with DataSync; use **Lambda** to convert shared‑drive triggers.
* **Phase 2** – Break polling logic into **EventBridge Pipes**; print via **AWS IoT Core FleetWise** printers.
* **Compute** – **EKS** micro‑services; **FSx File Gateway** for on‑prem printer compatibility.
* **Observability** – **CloudWatch Logs Insights** for batch SLAs; alert if job delay > 5 min.
* **Durability** – 11 nines via EFS/TDE + replication to second region.

### A6 Sales Analytics Data Lake (PostgreSQL + Spark)

* **Ingest** – Stream change‑data‑capture from SAP & MySQL via **Database Migration Service** into **S3 Iceberg** tables.
* **Process** – **Glue v4** Spark ETLs, managing job bookmarks; orchestrate with **Airflow on MWAA**.
* **Query** – **Amazon Redshift RA3** with Redshift Spectrum; materialize BI marts via **dbt**.
* **ML** – **SageMaker Pipelines** train XGBoost, register models in **Model Registry**; endpoints on **SageMaker Serverless Inference**.
* **Governance** – **Lake Formation** column‑level security on PHI; **Macie** classification jobs.

### A7 Recruiting Portal (Rails / MySQL)

* **Re‑platform** to **AWS App Runner**; auto‑https, zero‑downtime deploys.
* **Database** – **RDS MariaDB Serverless v2**; enable automatic pause/resume.
* **CI/CD** – GitHub Actions → App Runner OIDC deploy; preview environments per PR.
* **Compliance** – SOC 2 CC6.2 encryption; ISO A.10.1.1 crypto.

### A8 Room Scheduler (ASP.NET / SQL Express)

* **Modernize** to **.NET 6 Lambda** with **Lambda Powertools** observability.
* **State** – Move data to **Amazon DynamoDB** with GSIs for schedule queries.
* **API** – **Amazon API Gateway HTTP API** + **Cognito** user pools.
* **Cost** – 85 % reduction via pay‑per‑request; SLA 99.9 %.

### A9 News & Events (WordPress)

* **Static Export** via **Hugo**; host on **AWS Amplify**/S3 with **CloudFront**.
* **Security** – **AWS WAF** common rule set; auto‑patch via CodePipeline; backups to S3 Glacier Instant.
* **Performance** – 400 % faster page load; use **Image Optimization** in Amplify.

---

**End of Appendix**
