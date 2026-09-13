# AWS Certified Machine Learning Engineer – Associate (MLA-C01)
## Complete AWS Services Exam Review Cheat Sheet

> **Purpose:** Fast final review of the AWS services and major features that can appear on the **MLA-C01** exam.
>
> **Exam mindset:** Do not memorize service definitions only. Memorize **the problem each service solves**, the **keywords that point to it**, and the **services it is commonly confused with**.
>
> **Scope note:** The official AWS in-scope service list is explicitly **non-exhaustive**. This guide includes the official in-scope services plus high-value features and services that commonly appear in MLA-C01 practice questions.

---

# 0. Ultra-Short Service Map

| If the question says... | Think... |
|---|---|
| Object storage / ML datasets / data lake | **Amazon S3** |
| ETL / discover schema / Data Catalog | **AWS Glue** |
| Visual data preparation | **SageMaker Data Wrangler / Glue DataBrew** |
| Distributed Spark/Hadoop | **Amazon EMR** |
| SQL directly on S3 | **Amazon Athena** |
| Streaming events with custom consumers / replay | **Amazon Kinesis Data Streams** |
| Deliver streaming data to S3/Redshift/OpenSearch | **Amazon Data Firehose** |
| Stateful stream processing / SQL-like streaming | **Managed Service for Apache Flink** |
| Feature repository for training + inference | **SageMaker Feature Store** |
| Train ML models | **SageMaker Training Jobs** |
| Hyperparameter search | **SageMaker Automatic Model Tuning (AMT)** |
| Track ML experiments | **SageMaker Experiments** |
| Explainability / bias | **SageMaker Clarify** |
| Monitor drift / model quality | **SageMaker Model Monitor** |
| Orchestrate ML-native workflow | **SageMaker Pipelines** |
| General AWS workflow orchestration | **AWS Step Functions** |
| Airflow DAGs | **Amazon MWAA** |
| Model approval/versioning | **SageMaker Model Registry** |
| Real-time inference | **SageMaker Real-Time Endpoint** |
| Spiky/low traffic inference | **SageMaker Serverless Inference** |
| Long-running / large-payload inference | **SageMaker Asynchronous Inference** |
| Offline predictions on a dataset | **SageMaker Batch Transform** |
| Foundation models / GenAI APIs | **Amazon Bedrock** |
| NLP sentiment/entities/classification | **Amazon Comprehend** |
| Speech → text | **Amazon Transcribe** |
| Text → speech | **Amazon Polly** |
| Translation | **Amazon Translate** |
| Images/video labels/faces/text | **Amazon Rekognition** |
| OCR/forms/tables | **Amazon Textract** |
| Conversational bot | **Amazon Lex** |
| Search enterprise documents | **Amazon Kendra** |
| Recommendations/personalization | **Amazon Personalize** |
| Logs/metrics/alarms | **Amazon CloudWatch** |
| Query logs | **CloudWatch Logs Insights** |
| Who called which AWS API? | **AWS CloudTrail** |
| Resource config/compliance history | **AWS Config** |
| Encrypt with managed keys | **AWS KMS** |
| Credentials/API secrets | **AWS Secrets Manager** |
| Permissions | **IAM** |
| Detect sensitive data in S3 | **Amazon Macie** |
| Infrastructure as code | **CloudFormation / AWS CDK** |
| CI/CD pipeline | **CodePipeline + CodeBuild + CodeDeploy** |
| Docker image registry | **Amazon ECR** |
| Containers with AWS-managed orchestration | **Amazon ECS** |
| Kubernetes | **Amazon EKS** |
| Serverless event processing | **AWS Lambda** |
| VM / custom GPU environment | **Amazon EC2** |
| Cost by project/team | **Cost allocation tags** |
| Spending alert | **AWS Budgets** |
| Historical cost analysis | **AWS Cost Explorer** |

---

# 1. Exam Domains

The MLA-C01 exam is organized into four domains:

1. **Data Preparation for Machine Learning**
2. **ML Model Development**
3. **Deployment and Orchestration of ML Workflows**
4. **ML Solution Monitoring, Maintenance, and Security**

A service can appear in more than one domain.

---

# 2. Analytics Services

## Amazon Athena

**What it is:** Serverless SQL query service.

**Use it when:**
- Data is already in **Amazon S3**.
- You need **ad hoc SQL**.
- You do not want to provision a database or cluster.
- You want to query Parquet/ORC/CSV/JSON files.

**Exam clues:** `SQL on S3`, `serverless analytics`, `ad hoc queries`.

**Remember:**
- Athena **queries data where it lives in S3**.
- Columnar formats such as **Parquet** generally reduce scanned data and cost.

**Do not confuse with:**
- **Redshift:** data warehouse for repeated/large-scale BI workloads.
- **EMR:** Spark/Hadoop distributed processing.

---

## Amazon Data Firehose

**What it is:** Managed service for delivering streaming data to destinations.

**Typical flow:**

`Producer → Data Firehose → S3 / Redshift / OpenSearch`

**Use it when:**
- You want the **easiest managed delivery** of streaming records.
- Destination is commonly S3, Redshift, or OpenSearch.
- You do not need to manage consumers/shards yourself.

**Exam clues:** `streaming delivery`, `ingest-transform-load`, `deliver to S3`.

**Do not confuse with Kinesis Data Streams:**
- **Kinesis Data Streams:** custom consumers, replay, shard-level control.
- **Data Firehose:** managed delivery pipeline.

---

## Amazon EMR

**What it is:** Managed big-data platform for frameworks such as Apache Spark and Hadoop.

**Use it when:**
- Huge datasets need **distributed processing**.
- The question mentions **Spark, Hadoop, Hive**.
- You need scalable preprocessing/analytics over large data.

**Exam clues:** `distributed computing`, `Spark`, `Hadoop`, `big data`.

**Example:** Process terabytes of purchase history using Spark before ML training.

**Do not confuse with Glue:**
- **Glue:** managed/serverless ETL and cataloging.
- **EMR:** more control over large distributed frameworks.

---

## AWS Glue

**What it is:** Serverless data integration and ETL service.

**Main pieces:**
- Glue ETL Jobs
- Glue Crawlers
- Glue Data Catalog
- Glue Studio
- Glue connections

**Use it when:**
- Extract from RDS/DynamoDB/S3 and **combine/transform** datasets.
- Discover schema automatically.
- Build ETL pipelines.
- Maintain metadata in the Data Catalog.

**Exam clues:** `ETL`, `crawl schema`, `combine data sources`, `Data Catalog`.

**Typical flow:**

`RDS + DynamoDB + S3 → Glue → S3`

---

## AWS Glue DataBrew

**What it is:** Visual/no-code data preparation tool.

**Use it when:**
- Analysts want to profile, clean, transform, and normalize data **without writing code**.

**Exam clues:** `visual data cleaning`, `no code`, `analyst`.

**Compared with SageMaker Data Wrangler:**
- **DataBrew:** general-purpose visual data preparation.
- **Data Wrangler:** ML-oriented preparation integrated with SageMaker.

---

## AWS Glue Data Quality

**What it is:** Service/features for defining and evaluating data quality rules.

**Use it when:**
- Validate completeness, uniqueness, ranges, schema, and data-quality expectations.
- Stop bad data before training.

**Exam clues:** `data quality rules`, `validate dataset`, `quality score`.

---

## Amazon Kinesis / Kinesis Data Streams

**What it is:** Real-time streaming data service with configurable capacity and consumers.

**Use it when:**
- Need real-time ingestion.
- Need **multiple/custom consumers**.
- Need to replay records.
- Need control over shards and throughput.

**Important exam concept: shards**
- More shards → more provisioned throughput and parallelism.
- If CloudWatch shows throttling/low throughput in provisioned mode → **increase shard count**.
- For unpredictable workloads, consider **on-demand capacity mode**.

**Exam clues:** `real-time`, `shards`, `consumers`, `replay`, `stream`.

---

## AWS Lake Formation

**What it is:** Service for building and governing secure data lakes.

**Use it when:**
- Centralized governance/permissions for data lake resources.
- Fine-grained access to tables/columns.
- S3 + Glue Data Catalog based data lake.

**Exam clues:** `data lake governance`, `fine-grained permissions`, `central data lake`.

---

## Amazon Managed Service for Apache Flink

**What it is:** Managed service for stateful real-time stream processing using Apache Flink.

**Use it when:**
- Need real-time transformations/aggregations.
- Need stateful stream processing or event-time/window processing.
- Input often comes from Kinesis/MSK.

**Exam clues:** `real-time analytics`, `Flink`, `window`, `stateful streaming`.

---

## Amazon OpenSearch Service

**What it is:** Managed search and analytics engine.

**Use it when:**
- Full-text search.
- Log analytics.
- Search/index large collections of documents/events.
- Vector search may be used in modern AI/RAG architectures.

**Exam clues:** `search`, `index`, `log analytics`, `near real-time search`.

---

## Amazon Quick / Amazon Quick Sight

**What it is:** AWS business intelligence and visualization platform. Quick Sight is the BI capability within Amazon Quick.

**Use it when:**
- Dashboards.
- BI reports.
- Visual analysis.
- Share KPIs with business users.

**Exam clues:** `dashboard`, `visualize business metrics`, `BI`.

**Do not confuse with CloudWatch dashboards:**
- **Quick Sight:** business analytics/BI.
- **CloudWatch:** operational monitoring.

---

## Amazon Redshift

**What it is:** Managed cloud data warehouse.

**Use it when:**
- Large structured analytical datasets.
- Repeated BI/SQL analytics.
- Data warehouse workloads.

**Exam clues:** `data warehouse`, `OLAP`, `BI analytics`.

**Do not confuse with:**
- **RDS:** transactional relational database.
- **Athena:** serverless query-on-S3.

---

# 3. Application Integration and Orchestration

## Amazon EventBridge

**What it is:** Serverless event bus and scheduler.

**Use it when:**
- Route AWS/application events.
- Trigger workflows when events occur.
- Schedule periodic jobs.

**Exam clues:** `event-driven`, `rule`, `scheduled event`, `service event`.

**Example:** Start retraining when new approved training data arrives.

---

## Amazon MWAA

**Full name:** Amazon Managed Workflows for Apache Airflow.

**Use it when:**
- Existing Airflow DAGs.
- Complex scheduled data/ML workflows.
- Team already knows Apache Airflow.

**Exam clues:** `Airflow`, `DAG`, `managed Airflow`.

---

## Amazon SNS

**What it is:** Publish/subscribe notification service.

**Use it when:**
- Fan-out notifications.
- CloudWatch Alarm → email/SMS/application.
- One message needs multiple subscribers.

**Exam clues:** `pub/sub`, `notify multiple subscribers`, `email alert`.

---

## Amazon SQS

**What it is:** Managed message queue.

**Use it when:**
- Decouple producers and consumers.
- Buffer work.
- Smooth traffic spikes.
- Reliable asynchronous processing.

**Exam clues:** `queue`, `decouple`, `buffer`, `retry`.

**SNS vs SQS:**
- **SNS:** push/fan-out.
- **SQS:** queue/pull-buffer.

---

## AWS Step Functions

**What it is:** Serverless state-machine workflow orchestration.

**Use it when:**
- Coordinate Lambda, Glue, SageMaker, ECS, Batch, and many AWS services.
- Need retries, branches, parallel steps, failure handling.

**Exam clues:** `state machine`, `retry`, `orchestrate multiple AWS services`.

**Compared with SageMaker Pipelines:**
- **SageMaker Pipelines:** ML-native lifecycle workflow.
- **Step Functions:** broad AWS workflow orchestration.

---

# 4. Cloud Financial Management

## AWS Billing and Cost Management

**Use it for:**
- Cost allocation tags.
- Billing reports.
- Account-level cost controls.

### Cost allocation tag sequence

1. **Create user-defined tags**
2. **Attach tags to resources**
3. **Activate cost allocation tags in Billing**

Use tags such as:

`Project=FraudDetection`

`Department=AI`

`Environment=Prod`

---

## AWS Budgets

**Use it when:**
- Need an alert when spend/usage crosses a threshold.
- Need forecasted-cost alerts.

**Exam clue:** `notify when spending exceeds $X`.

**Do not confuse with Cost Explorer:** Budgets = alerts; Cost Explorer = analysis.

---

## AWS Cost Explorer

**Use it when:**
- Analyze historical AWS spending.
- Filter/group by service, tag, account, region.
- Review cost trends and forecasts.

**Exam clue:** `analyze where money was spent`.

---

# 5. Compute

## AWS Batch

**What it is:** Managed batch computing scheduler.

**Use it when:**
- Queue and execute large numbers of batch jobs.
- Containerized offline data/ML processing.
- Compute requirements vary by job.

**Exam clues:** `batch jobs`, `job queue`, `batch compute`.

---

## Amazon EC2

**What it is:** Virtual machines.

**Use it when:**
- Need full OS/runtime control.
- Need custom GPU/accelerator environment.
- Self-manage inference/training software.
- Long-running services.

### Instance family thinking

- **T** – burstable general purpose
- **M** – general purpose
- **C** – compute optimized
- **R** – memory optimized
- **G** – GPU graphics/inference/training
- **P** – high-performance ML/HPC GPU training

**Exam clue:** Need maximum control → EC2.

---

## AWS Lambda

**What it is:** Serverless functions.

**Use it when:**
- Event-driven transformations.
- Small preprocessing tasks.
- Trigger pipelines.
- Lightweight inference where function constraints are acceptable.

**Exam clues:** `event-driven`, `no servers`, `short task`.

**Not ideal for:** very large models or long-running heavy jobs.

---

## AWS Serverless Application Repository

**What it is:** Repository of reusable serverless applications/templates.

**Exam priority:** Low.

**Use it when:** Reusing/deploying packaged Lambda/serverless applications.

---

# 6. Containers

## Amazon ECR

**What it is:** Managed Docker/OCI container image registry.

**Use it when:**
- Store custom SageMaker training/inference images.
- Store ECS/EKS images.

**Exam clue:** `container image repository`.

---

## Amazon ECS

**What it is:** AWS-native container orchestration.

**Use it when:**
- Run containerized workloads without Kubernetes complexity.
- Need managed scheduling for containers.

**Exam clue:** `containers but not Kubernetes`.

---

## Amazon EKS

**What it is:** Managed Kubernetes.

**Use it when:**
- Kubernetes is required.
- Need portability/custom orchestration.
- Existing Kubernetes ecosystem.

**Exam clue:** `Kubernetes`, `kubectl`, `K8s`.

**SageMaker vs EKS:**
- **SageMaker:** best when you want managed ML lifecycle.
- **EKS:** best when Kubernetes flexibility/control is a hard requirement.

---

# 7. Databases

## Amazon DocumentDB

**What it is:** MongoDB-compatible document database.

**Use it when:** JSON/document-oriented application data.

**Exam clue:** `document database`, `MongoDB compatibility`.

---

## Amazon DynamoDB

**What it is:** Serverless managed NoSQL key-value/document database.

**Use it when:**
- Millisecond-scale access.
- Massive scale.
- Key-value/session/metadata lookup.

**Exam clues:** `NoSQL`, `key-value`, `low latency`, `serverless`.

---

## Amazon ElastiCache

**What it is:** Managed in-memory cache (Redis/Valkey/Memcached family depending on current offering).

**Use it when:**
- Low-latency cache.
- Repeated inference lookup.
- Session or frequently accessed feature/result caching.

**Exam clue:** `cache`, `sub-millisecond/very low latency`.

---

## Amazon Neptune

**What it is:** Managed graph database.

**Use it when:**
- Relationship-heavy graph data.
- Fraud networks, knowledge graphs, recommendations based on graph relationships.

**Exam clue:** `graph`, `relationships`, `nodes/edges`.

---

## Amazon RDS

**What it is:** Managed relational database.

**Use it when:**
- SQL relational transactional data.
- MySQL/PostgreSQL/etc.
- Application database.

**Exam clues:** `relational`, `SQL`, `transactions`.

**RDS vs Redshift:**
- **RDS:** OLTP.
- **Redshift:** OLAP/data warehouse.

---

# 8. Developer Tools and CI/CD

## AWS CDK

**What it is:** Infrastructure as code using programming languages.

**Use it when:**
- Define AWS infrastructure programmatically with Python/TypeScript/etc.
- Generate CloudFormation.

**Exam clue:** `infrastructure as code using programming language`.

---

## AWS CodeArtifact

**What it is:** Managed package/artifact repository.

**Use it when:**
- Store Python/npm/Maven/etc. dependencies.
- Private package management in CI/CD.

---

## AWS CodeBuild

**What it is:** Managed build/test service.

**Use it when:**
- Compile code.
- Run tests.
- Build Docker images.

**Exam clue:** `build and test stage`.

---

## AWS CodeDeploy

**What it is:** Deployment automation service.

**Use it when:**
- Automate application deployment.
- Blue/green or rolling deployment scenarios for supported compute targets.

**Exam clue:** `deployment stage`.

---

## AWS CodePipeline

**What it is:** CI/CD pipeline orchestrator.

**Typical flow:**

`Source → CodeBuild → Tests → Deploy/Register Model`

**Exam clue:** `CI/CD pipeline`, `automate stages`.

---

## AWS X-Ray

**What it is:** Distributed tracing service.

**Use it when:**
- Trace requests through microservices/API/Lambda.
- Identify latency across components.

**Exam clue:** `distributed tracing`, `request path`.

---

# 9. Machine Learning and AI Services

# Amazon SageMaker AI — MOST IMPORTANT SERVICE

SageMaker is the core MLA-C01 service. Know the individual features below.

---

## SageMaker Studio

Integrated ML development environment.

**Use it for:**
- Notebooks.
- Data preparation.
- Training.
- Experimentation.
- Deployment.
- Monitoring.

---

## SageMaker Data Wrangler

Visual ML data preparation.

**Use it when:**
- Import data.
- Visualize distributions.
- Clean and transform features.
- Export flows into SageMaker processing/pipelines.

**Exam clue:** `visual feature engineering in SageMaker`.

---

## SageMaker Processing

Managed processing jobs for preprocessing/postprocessing/evaluation.

**Use it when:**
- Run custom Python/Spark/scikit-learn preprocessing at scale.
- Evaluate model outputs.

**Glue vs SageMaker Processing:**
- **Glue:** general ETL/data engineering.
- **Processing:** ML-specific custom preprocessing/evaluation close to SageMaker workflows.

---

## SageMaker Feature Store

Central store for ML features.

### Online Store
Low-latency features for real-time inference.

### Offline Store
Historical features for training/batch analysis.

**Use it when:**
- Need consistent features between training and inference.
- Avoid feature duplication.
- Reduce training-serving skew.

**Exam clue:** `reuse features`, `online + offline`, `consistent features`.

---

## SageMaker Training Jobs

Managed infrastructure to train models.

**Know:**
- Training image/container
- Input data channels
- Hyperparameters
- Instance type/count
- Output model artifacts to S3

**Exam clue:** managed model training.

---

## SageMaker Automatic Model Tuning (AMT)

Hyperparameter optimization.

**Use it when:**
- Search hyperparameter combinations automatically.
- Optimize a chosen objective metric.

**Exam clue:** `best hyperparameters`, `HPO`.

---

## SageMaker Experiments

Track experiments, trials, parameters, artifacts, and metrics.

**Use it when:** Compare multiple training runs.

**Exam clue:** `track experiments`, `compare runs`.

---

## SageMaker Debugger

Inspect training behavior and detect issues.

**Use it when:**
- Debug gradients/weights/training performance.
- Identify bottlenecks or bad training behavior.

---

## SageMaker Clarify

Bias detection and explainability.

### Pre-training bias
Examples:
- **Class Imbalance (CI)** – representation imbalance between groups.
- **Difference in Proportions of Labels (DPL)** – difference in positive label rates.
- **Total Variation Distance (TVD)** – disparity between distributions.
- **KL divergence** – divergence between probability distributions.

### Explainability
- SHAP-based feature attribution.

**Exam clues:** `bias`, `fairness`, `explain predictions`, `feature attribution`.

---

## SageMaker Model Monitor

Monitor deployed models.

### Major monitor types

1. **Data Quality Monitor**
   - Input feature/data distribution changes.
   - Missing values/schema/statistical drift.

2. **Model Quality Monitor**
   - Prediction quality after ground truth becomes available.
   - Accuracy, precision, recall, RMSE, etc.

3. **Bias Drift Monitor**
   - Fairness/bias changes over time.

4. **Feature Attribution Drift Monitor**
   - Changes in feature importance/attribution.

**Quick rule:**
- Input distribution changed → **Data Quality**
- Prediction performance degraded → **Model Quality**
- Fairness changed → **Bias**
- Explainability/importance changed → **Feature Attribution Drift**

---

## SageMaker Pipelines

ML-native workflow orchestration.

**Common steps:**
- Processing
- Training
- Tuning
- Condition
- RegisterModel

**Use it when:** End-to-end repeatable ML workflow inside SageMaker.

**Exam clue:** `ML pipeline`, `training workflow`, `model registration`.

---

## SageMaker Model Registry

Model versioning, metadata, and approval status.

**Use it when:**
- Register model versions.
- Approve/reject models before production.
- Track model lineage/version lifecycle.

**Exam clue:** `model approval`, `model versioning`.

---

## SageMaker Endpoint Inference Modes

### Real-Time Inference

Use when:
- Persistent endpoint.
- Low latency.
- Steady or predictable traffic.

**Clue:** `milliseconds`, `always available`.

---

### Serverless Inference

Use when:
- Intermittent/spiky traffic.
- Want no instance management.
- Can tolerate cold starts unless using provisioned concurrency.

**Clue:** `unpredictable traffic`, `low operations`, `pay for use`.

---

### Asynchronous Inference

Use when:
- Requests take longer.
- Large payloads.
- Client does not need immediate response.
- Queueing is acceptable.

**Clue:** `large payload`, `long processing`, `async`.

---

### Batch Transform

Use when:
- Entire dataset needs offline predictions.
- No persistent endpoint.
- Latency is not interactive.

**Clue:** `millions of records overnight`, `offline inference`.

### Fast inference decision

| Requirement | Mode |
|---|---|
| Interactive + low latency | Real-Time |
| Intermittent/spiky | Serverless |
| Long-running / large request | Asynchronous |
| Offline full dataset | Batch Transform |

---

## SageMaker Multi-Model Endpoints

Host multiple models behind one endpoint.

**Use it when:**
- Many similar models.
- Models are not all heavily used simultaneously.
- Need cost savings.

---

## SageMaker Inference Recommender

Helps select suitable instance types/configurations for model inference.

**Exam clue:** optimize endpoint cost/performance automatically.

---

## SageMaker Neo

Model optimization/compilation for target hardware.

**Exam clue:** compile/optimize model for specific device/processor.

---

## SageMaker Canvas

No-code ML environment.

**Use it when:** Business analysts need to build predictions without coding.

**Exam clue:** `no-code ML`.

---

## SageMaker JumpStart

Prebuilt models, solution templates, foundation models, and examples.

**Exam clue:** `pretrained model`, `quick start`.

---

## SageMaker Ground Truth

Human labeling service/workflow for training data.

**Use it when:** Need labeled datasets at scale.

---

## Amazon Augmented AI (A2I)

Human review workflows for ML predictions.

**Use it when:**
- Low-confidence predictions need human review.
- Human-in-the-loop after inference.

**Ground Truth vs A2I:**
- **Ground Truth:** label data for training.
- **A2I:** human review of model predictions/workflows.

---

## Amazon Bedrock

Managed access to foundation models and GenAI capabilities.

**Use it when:**
- Build GenAI apps without managing model infrastructure.
- Prompt foundation models.
- RAG/knowledge bases.
- Agents/guardrails where appropriate.

**Bedrock vs SageMaker:**
- **Bedrock:** easiest managed foundation-model API/application layer.
- **SageMaker:** more control over training, fine-tuning, hosting, custom ML lifecycle.

---

## Amazon CodeGuru

ML-powered developer tooling for code/performance analysis.

**Exam priority:** Low.

---

## Amazon Comprehend

Managed NLP.

**Use it for:**
- Sentiment.
- Entities.
- Key phrases.
- Language detection.
- Text classification.

**Exam clue:** `NLP analysis of text`.

---

## Amazon Comprehend Medical

NLP for extracting medical information from clinical text.

**Exam priority:** Low unless healthcare context is explicit.

---

## Amazon DevOps Guru

ML-powered operational anomaly detection/insights.

**Use it when:** Detect unusual application/infrastructure behavior and receive operational recommendations.

---

## Amazon Fraud Detector

Managed service for building fraud-detection models/rules.

**Use it when:** Explicit fraud-risk use case and managed fraud service is desired.

---

## AWS HealthLake

FHIR-based health data lake/service.

**Use it when:** Healthcare/FHIR data ingestion and analytics.

---

## Amazon Kendra

Enterprise intelligent search.

**Use it when:**
- Search across internal documents.
- Need enterprise search relevance/connectors.

**Exam clue:** `enterprise document search`.

---

## Amazon Lex

Build conversational chatbots/voice bots.

**Use it when:** Intent recognition + slot filling + conversation management.

**Do not confuse with Comprehend:**
- **Lex:** interactive bot.
- **Comprehend:** analyze text.

---

## Amazon Lookout for Equipment

ML anomaly/predictive-maintenance service for industrial equipment.

---

## Amazon Lookout for Metrics

Detect anomalies in business/operational metrics.

---

## Amazon Lookout for Vision

Detect visual defects/anomalies in manufacturing images.

---

## Amazon Mechanical Turk

Human workforce marketplace.

**Exam priority:** Low.

**Use it when:** Human tasks/data labeling requiring external workforce.

---

## Amazon Personalize

Managed recommendation/personalization service.

**Use it when:** Product/content recommendation.

**Exam clue:** `recommendation engine`, `personalized ranking`.

---

## Amazon Polly

Text → speech.

**Mnemonic:** **Polly speaks.**

---

## Amazon Q

Generative AI assistant family for AWS/business/developer use cases.

**Exam priority for MLA-C01:** Lower than SageMaker/Bedrock, but officially in scope.

---

## Amazon Rekognition

Computer vision service.

**Use it for:**
- Image/video labels.
- Object/scene detection.
- Face-related analysis.
- Moderation.
- Text in images in some use cases.

**Exam clue:** `analyze image/video`.

---

## Amazon Textract

OCR + document structure extraction.

**Use it for:**
- Printed/handwritten text.
- Forms.
- Tables.
- Key-value pairs.

**Rekognition vs Textract:**
- **Rekognition:** what's in the image/video?
- **Textract:** what text/form/table is in the document?

---

## Amazon Transcribe

Speech → text.

**Mnemonic:** **Transcribe listens and writes.**

---

## Amazon Translate

Machine translation between languages.

---

# 10. Management and Governance

## AWS Auto Scaling

Automatically adjusts capacity.

**Use it when:** Scale EC2/ECS/SageMaker-related infrastructure based on load.

**Exam note:** Dynamic/target-tracking scaling is better than scheduled scaling when traffic is unpredictable.

---

## AWS Chatbot / Amazon Q Developer in chat applications

Used to surface AWS notifications and operational information in chat platforms.

**Exam priority:** Low.

---

## AWS CloudFormation

Declarative infrastructure as code.

**Use it when:** Define AWS resources in templates.

**CDK vs CloudFormation:**
- **CDK:** program in Python/TypeScript/etc., synthesizes CloudFormation.
- **CloudFormation:** direct IaC templates.

---

## AWS CloudTrail

Records AWS API activity.

**Answers questions like:**
- Who deleted a resource?
- Which IAM principal changed the bucket policy?
- What API call was made and when?

**Mnemonic:** **CloudTrail = audit trail / WHO did WHAT.**

---

## Amazon CloudWatch

Operational monitoring.

**Use it for:**
- Metrics.
- Alarms.
- Dashboards.
- Logs.
- Events/operational observability.

**Examples:**
- CPU high.
- Endpoint invocation latency rising.
- Error count increasing.

---

## Amazon CloudWatch Logs

Central log storage.

### CloudWatch Logs Insights

Query and analyze logs.

**Exam clue:** `search logs`, `query application logs`, `analyze log patterns`.

### Critical comparison

- **CloudWatch:** WHAT is happening operationally?
- **CloudTrail:** WHO made an AWS API call?
- **AWS Config:** WHAT configuration did a resource have and was it compliant?

---

## AWS Compute Optimizer

Provides rightsizing recommendations.

**Use it when:** Optimize compute resources based on utilization.

---

## AWS Config

Tracks resource configuration and compliance over time.

**Use it when:**
- Was a security group configured correctly?
- Did a resource become noncompliant?
- What did the configuration look like yesterday?

**Exam clue:** `configuration history`, `compliance rule`.

---

## AWS Organizations

Multi-account governance.

**Know:**
- Organizational Units (OUs)
- Service Control Policies (SCPs)
- Central account governance

**Important:** SCPs set permission guardrails; they do not directly grant permissions.

---

## AWS Service Catalog

Approved portfolio of deployable products/templates.

**Use it when:** Organizations want users to deploy only pre-approved architectures.

---

## AWS Systems Manager

Operational management of AWS resources.

**Know common features:**
- Parameter Store
- Run Command
- Session Manager
- Patch Manager

**Exam relevance:** configuration/operations and secure parameter storage.

---

## AWS Trusted Advisor

Best-practice checks and recommendations.

**Areas:** cost, performance, security, fault tolerance, service limits.

---

# 11. Media

## Amazon Kinesis Video Streams

Ingest and store streaming video from devices/cameras.

**Use it when:** Video stream ingestion for downstream analytics/ML.

---

# 12. Migration and Transfer

## AWS DataSync

Online accelerated/managed data transfer between on-premises storage and AWS storage.

**Use it when:**
- NFS/SMB/object storage → S3/EFS/FSx.
- Recurring or one-time migrations.
- A usable network connection exists.

**Exam clue:** `automated online migration`.

---

## AWS Data Transfer Terminal — Useful Practice-Question Extra

Physical AWS facility where you bring storage devices and use high-speed connectivity to upload/download data.

**Use it when:**
- Very large dataset.
- On-premises internet is too slow or impractical.
- Physical transport to a facility is acceptable.

**Exam clue:** `100 TB`, `limited internet`, `online transfer impractical`.

---

# 13. Networking and Content Delivery

## Amazon API Gateway

Managed API front door.

**Use it when:**
- Expose inference/application APIs.
- Authenticate, throttle, monitor API requests.
- Integrate with Lambda or backend services.

---

## Amazon CloudFront

Content delivery network (CDN).

**Use it when:**
- Cache and deliver content globally with low latency.
- Place edge distribution in front of S3/API/app.

---

## AWS Direct Connect

Dedicated private network link from on-premises to AWS.

**Use it when:**
- Predictable, high-bandwidth, private network connectivity.
- Long-term hybrid connection.

**Compare:**
- Internet/VPN → quicker setup.
- Direct Connect → dedicated connection.

---

## Amazon VPC

Network isolation for AWS resources.

**Know:**
- Public/private subnets
- Route tables
- Security groups
- Network ACLs
- VPC endpoints
- NAT gateway concept

### Important ML security pattern

To keep SageMaker/data traffic private:
- Place resources in a VPC as required.
- Use **VPC endpoints/PrivateLink** for S3/SageMaker-related service access where supported.
- Restrict internet access.

---

# 14. Security, Identity, and Compliance

## AWS KMS

Managed encryption key service.

**Use it when:**
- Encrypt S3, EBS, RDS, SageMaker artifacts, logs, etc.
- Need customer-managed key control/auditability.

### AWS-managed key vs customer-managed key

Choose **customer-managed KMS key** when you need:
- Explicit key policy control.
- Rotation/administrative control.
- Cross-account patterns.
- Stronger separation of duties.
- Specific compliance requirements.

---

## Amazon Macie

Uses ML/pattern matching to discover sensitive data in S3.

**Use it when:** Find PII/sensitive data in S3 buckets.

**Exam clue:** `discover sensitive data in S3`.

---

## AWS Secrets Manager

Stores and rotates secrets.

**Use it for:**
- Database passwords.
- API keys.
- Credentials.

**Do not hardcode credentials in notebooks/code.**

---

## AWS IAM

Identity and access management.

**Know:**
- Users
- Groups
- Roles
- Policies
- Resource-based policies
- Least privilege
- Temporary credentials

### Critical exam rule

**AWS service needs permission → use an IAM role/execution role.**

Example:

`SageMaker execution role → S3 GetObject`

Do not attach service permissions to a human user when the compute environment itself needs access.

### Managed vs inline policy

- **Managed policy:** reusable across identities/roles.
- **Inline policy:** tightly coupled to one specific identity/role.

---

# 15. Storage

## Amazon EBS

Block storage for EC2.

**Use it when:** Persistent disk attached to EC2, usually one AZ.

**Exam clue:** `block volume`, `EC2 disk`.

---

## Amazon EFS

Managed elastic NFS file system.

**Use it when:**
- Shared POSIX/NFS file system.
- Multiple compute instances need shared files.

**Important:** EFS is a filesystem, not an object store.

---

## Amazon FSx

Managed high-performance file systems.

Examples may include:
- FSx for Lustre
- FSx for Windows File Server
- FSx for NetApp ONTAP

### FSx for Lustre

Especially relevant for high-performance ML/HPC file workloads and can integrate with S3.

**Exam clue:** `high-performance shared filesystem`, `Lustre`, `HPC`.

---

## Amazon S3

The most important general-purpose ML data storage service.

**Use it for:**
- Training datasets.
- Model artifacts.
- Data lakes.
- Logs.
- Batch inputs/outputs.

**Strengths:**
- Massive scalability.
- High durability.
- Object storage.
- Lifecycle policies.
- Versioning.
- Encryption.

### Common ML architecture

`Sources → Glue/EMR → S3 → SageMaker → S3 model artifacts`

### S3 Transfer Acceleration

Speeds long-distance uploads using AWS edge infrastructure.

**Important:** It still requires network/internet connectivity.

Use when:
- Uploaders are far from the S3 Region.
- Online transfer is feasible but needs acceleration.

Do **not** choose when the question says internet connectivity is too limited for online transfer.

---

## Amazon S3 Glacier storage classes

Low-cost archival storage.

**Use it when:**
- Data is rarely accessed.
- Long-term retention.
- Retrieval delay is acceptable.

**Exam clue:** `archive`, `rare access`, `lowest storage cost`.

---

## AWS Storage Gateway

Hybrid storage integration between on-premises environments and AWS storage.

**Use it when:**
- On-prem applications still need file/volume/tape-like access while AWS provides cloud-backed storage.

**Do not confuse with DataSync:**
- **Storage Gateway:** ongoing hybrid storage interface.
- **DataSync:** data movement/migration.

---

# 16. Data Formats — Frequently Tested With AWS Services

## CSV

Best for:
- Simple tabular data.
- Human readability.
- Wide compatibility.

Weakness:
- No rich schema/types.
- Less efficient at huge analytics scale.

---

## JSON / JSON Lines (JSONL)

### JSONL

One JSON object per line.

Best for:
- Streaming events.
- Logs.
- Semi-structured records.
- Record-by-record parsing.

**Exam clue:** `real-time parsing`, `streaming diverse records`.

---

## Parquet

Columnar, compressed format.

Best for:
- S3 data lakes.
- Athena/Spark/Glue analytics.
- Large historical datasets.
- Reading selected columns efficiently.

**Exam clue:** `columnar`, `analytics`, `compression`.

---

## ORC

Columnar format common in Hadoop/Hive ecosystems.

Best for:
- Big-data analytics, especially Hive-oriented pipelines.

---

## Quick rule

- **Streaming/semi-structured → JSONL**
- **Large analytical dataset → Parquet**
- **Simple small table → CSV**
- **Hive/Hadoop columnar → ORC**

---

# 17. Visualization Cheat Sheet

| Goal | Visualization |
|---|---|
| Compare categories | **Bar chart** |
| Distribution of continuous numeric values | **Histogram** |
| Smooth probability distribution | **Density plot** |
| Relationship between two numeric variables | **Scatter plot** |
| Trend over time | **Line chart** |
| Pattern across two dimensions | **Heatmap** |
| Feature relationship/correlation matrix | **Heatmap** |
| Outliers / quartiles | **Box plot** |

### Bar vs Histogram

- **Bar chart:** categorical values.
- **Histogram:** continuous numerical ranges/bins.

---

# 18. Deployment Strategy Cheat Sheet

## Blue/Green

Maintain old and new environments separately, then switch traffic.

Choose when:
- Need strong rollback safety.
- Can afford duplicate environments.

---

## Canary

Send a small percentage of traffic to the new model first.

Choose when:
- Want to reduce deployment risk.
- Validate behavior with real production traffic before full rollout.

---

## A/B Testing

Send different traffic groups to different model variants and compare business/model outcomes.

Choose when:
- Need to decide which model/version performs better.

---

# 19. Scaling Cheat Sheet

## Target Tracking / Dynamic Scaling

Use when demand changes unpredictably.

Example:
- Keep invocations per instance around a target.

## Scheduled Scaling

Use when traffic pattern is known and predictable.

Example:
- Scale up every weekday at 8 AM.

### Exam rule

If the question says traffic is **unpredictable**, prefer **dynamic/target-tracking**, not scheduled scaling.

---

# 20. ML Monitoring Decision Table

| Problem | Choose |
|---|---|
| Input feature distribution changed | **Data Quality Monitor** |
| Prediction accuracy degraded | **Model Quality Monitor** |
| Fairness metrics changed | **Bias Drift Monitor** |
| Feature importance changed | **Feature Attribution Drift Monitor** |
| Endpoint CPU/GPU/memory/latency | **CloudWatch** |
| Search application logs | **CloudWatch Logs Insights** |
| Audit AWS API actions | **CloudTrail** |
| Check resource configuration/compliance | **AWS Config** |

---

# 21. Bias and Explainability Cheat Sheet

## Class Imbalance (CI)

Question:
> Are demographic groups represented equally in the dataset?

Example:
- 8,000 male records
- 2,000 female records

---

## Difference in Proportions of Labels (DPL)

Question:
> Is the positive label rate different between groups?

Example:
- Group A approval = 80%
- Group B approval = 60%

---

## Total Variation Distance (TVD)

Question:
> How different are outcome distributions / what is the maximum disparity?

---

## Kullback-Leibler Divergence (KL)

Question:
> How much does one probability distribution diverge from another?

Important:
- KL is asymmetric.
- Used to compare probability distributions.

---

## Partial Dependence Plot (PDP)

Shows how predicted outcome changes as one/few features change while averaging over other features.

Example:
> How does churn probability change as age increases?

---

# 22. Encryption and Security Cheat Sheet

| Need | Service/Feature |
|---|---|
| Encrypt data with managed keys | **AWS KMS** |
| Store API/database secrets | **Secrets Manager** |
| Give SageMaker access to S3 | **IAM execution role** |
| Detect sensitive data in S3 | **Macie** |
| Private network | **VPC** |
| Private access to AWS services | **VPC endpoints / PrivateLink** |
| Audit API actions | **CloudTrail** |
| Resource compliance | **AWS Config** |

### Principle of least privilege

Grant only:
- Required actions.
- Required resources.
- Required conditions.

Example:
- Prefer `s3:GetObject` on one training-data prefix over `s3:*` on all buckets.

---

# 23. CI/CD for ML — Typical Architecture

```text
Git / Source
   ↓
CodePipeline
   ↓
CodeBuild
   ↓
Tests
   ↓
SageMaker Pipeline / Training
   ↓
Model Registry
   ↓
Approval
   ↓
Deploy Endpoint
   ↓
CloudWatch + Model Monitor
```

Possible supporting services:
- **ECR** for images.
- **S3** for artifacts.
- **EventBridge** for triggers.
- **Step Functions** for cross-service orchestration.
- **CloudFormation/CDK** for infrastructure.

---

# 24. High-Value Service Comparisons

## Glue vs EMR vs SageMaker Processing

| Service | Best choice |
|---|---|
| **Glue** | Serverless ETL, catalog, schema discovery |
| **EMR** | Spark/Hadoop distributed big-data jobs |
| **SageMaker Processing** | Custom ML preprocessing/evaluation |

---

## Kinesis Data Streams vs Data Firehose

| Kinesis Data Streams | Data Firehose |
|---|---|
| Custom consumers | Managed delivery |
| Replay records | Minimal management |
| Shards / throughput control | Delivery to destinations |
| Real-time application logic | S3/Redshift/OpenSearch delivery |

---

## Athena vs Redshift

| Athena | Redshift |
|---|---|
| Query S3 directly | Data warehouse |
| Serverless/ad hoc | Repeated high-performance analytics |
| Pay per scanned data | Provisioned/serverless warehouse capacity |

---

## S3 vs EFS vs EBS

| Storage | Type | Typical use |
|---|---|---|
| **S3** | Object | datasets, artifacts, data lake |
| **EFS** | Shared file | NFS/shared POSIX files |
| **EBS** | Block | EC2 disk |

---

## DataSync vs Storage Gateway vs Transfer Acceleration vs Data Transfer Terminal

| Need | Choose |
|---|---|
| Automated online migration | **DataSync** |
| Ongoing hybrid storage | **Storage Gateway** |
| Faster long-distance internet uploads to S3 | **S3 Transfer Acceleration** |
| Huge transfer + internet impractical | **Data Transfer Terminal** |

---

## Step Functions vs SageMaker Pipelines vs MWAA

| Service | Best choice |
|---|---|
| **SageMaker Pipelines** | ML-native training/processing/register workflow |
| **Step Functions** | General AWS service orchestration |
| **MWAA** | Existing/required Apache Airflow DAGs |

---

## CloudWatch vs CloudTrail vs Config

| Service | Think |
|---|---|
| **CloudWatch** | Metrics/logs/alarms — WHAT is happening |
| **CloudTrail** | API audit — WHO did WHAT |
| **Config** | Resource configuration/compliance — HOW was it configured |

---

## Comprehend vs Lex

| Service | Use |
|---|---|
| **Comprehend** | Analyze NLP text |
| **Lex** | Build conversational bots |

---

## Rekognition vs Textract

| Service | Use |
|---|---|
| **Rekognition** | Understand images/video |
| **Textract** | Extract document text/forms/tables |

---

## Transcribe vs Polly vs Translate

- **Transcribe:** speech → text
- **Polly:** text → speech
- **Translate:** language A → language B

---

# 25. Official MLA-C01 In-Scope Service Checklist

Use this as a final checklist.

## Analytics
- [ ] Amazon Athena
- [ ] Amazon Data Firehose
- [ ] Amazon EMR
- [ ] AWS Glue
- [ ] AWS Glue DataBrew
- [ ] AWS Glue Data Quality
- [ ] Amazon Kinesis
- [ ] AWS Lake Formation
- [ ] Amazon Managed Service for Apache Flink
- [ ] Amazon OpenSearch Service
- [ ] Amazon Quick / Quick Sight
- [ ] Amazon Redshift

## Application Integration
- [ ] Amazon EventBridge
- [ ] Amazon MWAA
- [ ] Amazon SNS
- [ ] Amazon SQS
- [ ] AWS Step Functions

## Cloud Financial Management
- [ ] AWS Billing and Cost Management
- [ ] AWS Budgets
- [ ] AWS Cost Explorer

## Compute
- [ ] AWS Batch
- [ ] Amazon EC2
- [ ] AWS Lambda
- [ ] AWS Serverless Application Repository

## Containers
- [ ] Amazon ECR
- [ ] Amazon ECS
- [ ] Amazon EKS

## Database
- [ ] Amazon DocumentDB
- [ ] Amazon DynamoDB
- [ ] Amazon ElastiCache
- [ ] Amazon Neptune
- [ ] Amazon RDS

## Developer Tools
- [ ] AWS CDK
- [ ] AWS CodeArtifact
- [ ] AWS CodeBuild
- [ ] AWS CodeDeploy
- [ ] AWS CodePipeline
- [ ] AWS X-Ray

## Machine Learning
- [ ] Amazon Augmented AI (A2I)
- [ ] Amazon Bedrock
- [ ] Amazon CodeGuru
- [ ] Amazon Comprehend
- [ ] Amazon Comprehend Medical
- [ ] Amazon DevOps Guru
- [ ] Amazon Fraud Detector
- [ ] AWS HealthLake
- [ ] Amazon Kendra
- [ ] Amazon Lex
- [ ] Amazon Lookout for Equipment
- [ ] Amazon Lookout for Metrics
- [ ] Amazon Lookout for Vision
- [ ] Amazon Mechanical Turk
- [ ] Amazon Personalize
- [ ] Amazon Polly
- [ ] Amazon Q
- [ ] Amazon Rekognition
- [ ] Amazon SageMaker
- [ ] Amazon Textract
- [ ] Amazon Transcribe
- [ ] Amazon Translate

## Management and Governance
- [ ] AWS Auto Scaling
- [ ] AWS Chatbot
- [ ] AWS CloudFormation
- [ ] AWS CloudTrail
- [ ] Amazon CloudWatch
- [ ] Amazon CloudWatch Logs
- [ ] AWS Compute Optimizer
- [ ] AWS Config
- [ ] AWS Organizations
- [ ] AWS Service Catalog
- [ ] AWS Systems Manager
- [ ] AWS Trusted Advisor

## Media
- [ ] Amazon Kinesis Video Streams

## Migration and Transfer
- [ ] AWS DataSync

## Networking and Content Delivery
- [ ] Amazon API Gateway
- [ ] Amazon CloudFront
- [ ] AWS Direct Connect
- [ ] Amazon VPC

## Security, Identity, and Compliance
- [ ] AWS KMS
- [ ] Amazon Macie
- [ ] AWS Secrets Manager
- [ ] IAM

## Storage
- [ ] Amazon EBS
- [ ] Amazon EFS
- [ ] Amazon FSx
- [ ] Amazon S3
- [ ] Amazon S3 Glacier
- [ ] AWS Storage Gateway

---

# 26. Priority Ranking for Final Review

## 🔥 Tier 1 — Know Extremely Well

- Amazon SageMaker AI and its features
- Amazon S3
- AWS Glue
- Amazon Kinesis Data Streams
- Amazon Data Firehose
- Amazon EMR
- AWS Lambda
- AWS Step Functions
- Amazon EventBridge
- Amazon CloudWatch / CloudWatch Logs
- AWS CloudTrail
- IAM
- AWS KMS
- Amazon ECR
- Amazon ECS / EKS
- AWS CodePipeline / CodeBuild / CodeDeploy
- AWS CloudFormation / CDK

## 🟠 Tier 2 — Know Well

- Athena
- Redshift
- Lake Formation
- Managed Service for Apache Flink
- RDS
- DynamoDB
- EFS / EBS / FSx
- DataSync
- Storage Gateway
- VPC
- API Gateway
- Budgets / Cost Explorer
- Config
- Organizations
- Secrets Manager
- Macie

## 🟡 Tier 3 — Know the Use Case

- Bedrock
- Comprehend
- Lex
- Rekognition
- Textract
- Transcribe
- Polly
- Translate
- Personalize
- Kendra
- Fraud Detector
- Lookout services
- HealthLake
- A2I
- Mechanical Turk
- CodeGuru
- DevOps Guru
- Quick Sight
- X-Ray
- Trusted Advisor
- Service Catalog
- Systems Manager
- Kinesis Video Streams

---

# 27. Final 30-Second Memory Map

```text
DATA
S3 = storage
Glue = ETL
EMR = Spark/Hadoop
Athena = SQL on S3
Kinesis = real-time stream
Firehose = stream delivery
Flink = stream processing

ML
Data Wrangler = prepare visually
Feature Store = reusable features
Training Job = train
AMT = tune
Experiments = track runs
Clarify = bias/explainability
Pipelines = ML workflow
Registry = versions/approval
Model Monitor = drift/quality
Endpoint = real-time
Serverless = spiky
Async = long/large
Batch Transform = offline predictions

OPS
CloudWatch = metrics/logs
CloudTrail = API audit
Config = resource configuration
EventBridge = events
Step Functions = workflow
CodePipeline = CI/CD

SECURITY
IAM = permissions
KMS = encryption keys
Secrets Manager = secrets
Macie = sensitive S3 data
VPC = network isolation

COST
Tags = allocate cost
Budgets = alert
Cost Explorer = analyze
```

---

# 28. Sources and Exam-Version Note

This cheat sheet is built for **AWS Certified Machine Learning Engineer – Associate (MLA-C01)** using the official AWS Certification exam guide and in-scope service list current in September 2026.

AWS states that its in-scope service list is **non-exhaustive and subject to change**.

Official references:

- AWS MLA-C01 exam guide:  
  https://docs.aws.amazon.com/aws-certification/latest/machine-learning-engineer-associate-01/
- Official in-scope services:  
  https://docs.aws.amazon.com/aws-certification/latest/machine-learning-engineer-associate-01/mla-01-in-scope-services.html
- AWS certification page:  
  https://aws.amazon.com/certification/certified-machine-learning-engineer-associate/

### 2026 transition note

AWS has announced the **MLA-C02 beta**. For the English MLA-C01 exam, AWS currently lists **September 28, 2026** as the last testing date, with MLA-C02 beta delivery beginning **September 29, 2026**.

---

**Best final-review strategy:** Learn the **decision words** in each scenario rather than memorizing every service description.
