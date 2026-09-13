# MLA-C01 Domain 4 — Deployment and Orchestration of ML Workflows

## 1. Workflow Orchestration

### SageMaker Pipelines
**Purpose:** ML-specific workflow orchestration.

Typical flow:

```text
Processing → Training → Evaluation → Condition → Register Model → Deploy
```

Key ideas:
- **ProcessingStep** → preprocessing / evaluation
- **TrainingStep** → model training
- **ConditionStep** → branch based on metrics
- **RegisterModel / Model Registry** → version and govern models
- Best when the workflow is primarily **SageMaker + ML**

**Exam clue:**  
> ML-native workflow → **SageMaker Pipelines**

---

### AWS Step Functions
**Purpose:** Serverless state-machine orchestration across AWS services.

Important states:
- **Task** → execute work
- **Choice** → if/else decision
- **Parallel** → run branches simultaneously
- **Wait** → pause
- **Succeed / Fail** → terminate workflow
- **Retry / Catch** → error handling

Common ML use cases:
- S3 → validate → transform → train → evaluate → register
- Batch inference
- A/B testing coordination
- Cross-service workflows

**Exam clue:**  
> Many AWS services + branching/retries → **Step Functions**

---

### Amazon MWAA
**Amazon Managed Workflows for Apache Airflow**

Best when:
- Teams already use **Airflow**
- Workflows are written as **Python DAGs**
- Complex scheduling and dependencies are required

**Exam clue:**  
> Python DAGs / Airflow → **MWAA**

---

### Quick Orchestration Comparison

| Service | Best For |
|---|---|
| SageMaker Pipelines | ML-specific workflows |
| Step Functions | General AWS service orchestration |
| MWAA | Airflow / Python DAGs |
| CodePipeline | CI/CD release orchestration |

Memory:

```text
SageMaker Pipelines = ML
Step Functions      = AWS services
MWAA                = Python DAGs
CodePipeline        = CI/CD
```

---

## 2. AWS Code Family

### AWS CodeBuild
**Purpose:** Build, test, and package code.

Typical flow:

```text
Source → Install dependencies → Test → Build → Artifact/Image
```

`buildspec.yml` phases:
- `install`
- `pre_build`
- `build`
- `post_build`

Outputs may go to:
- **S3** → build artifacts
- **ECR** → Docker images

**Exam clue:**  
> Compile / run tests / package → **CodeBuild**

---

### AWS CodeDeploy
**Purpose:** Automate application deployment.

Targets:
- EC2
- On-premises servers
- Lambda
- ECS

Deployment strategies:
- **In-place** → update existing instances
- **Blue/Green** → create new environment, then switch traffic
- **Canary** → expose a small percentage first
- **Rolling** → replace instances gradually

`appspec.yml` defines:
- files
- permissions
- lifecycle hooks

Typical hooks:

```text
BeforeInstall → AfterInstall → ApplicationStart → ValidateService
```

**Exam clue:**  
> Deploy application → **CodeDeploy**

---

### AWS CodePipeline
**Purpose:** Orchestrate the entire CI/CD release process.

Typical flow:

```text
Source → Build → Test → Deploy
```

For ML:

```text
Code change
   ↓
CodePipeline
   ↓
CodeBuild
   ↓
Step Functions / SageMaker Pipelines
   ↓
Train → Evaluate → Register
   ↓
Deploy to Staging
   ↓
Manual Approval
   ↓
Production
```

**Manual approval gate:** provides **human oversight before production changes**.

**Exam clue:**  
> Coordinate Source → Build → Test → Deploy → **CodePipeline**

---

### AWS CodeCommit
**Purpose:** Git-based source code repository.

**Exam clue:**  
> Store/version source code → **CodeCommit**

---

### AWS CodeArtifact
**Purpose:** Managed package repository.

Supports packages such as:
- Python / pip
- npm
- Maven / Gradle
- NuGet

**Exam clue:**  
> Store dependencies/packages → **CodeArtifact**

---

### Code Family Memory Trick

```text
CodeCommit   = STORE CODE
CodeBuild    = BUILD + TEST
CodeDeploy   = DEPLOY
CodePipeline = CONNECT EVERYTHING
CodeArtifact = STORE PACKAGES
```

---

## 3. SageMaker Projects and MLOps

### SageMaker Projects
Provides ready-made MLOps templates for:
- CI/CD
- SageMaker Pipelines
- Model Registry
- CodePipeline / CodeBuild integrations
- Automated testing

**Exam clue:**  
> MLOps out-of-the-box / pre-built CI/CD templates → **SageMaker Projects**

Difference:

| Feature | Purpose |
|---|---|
| SageMaker Projects | End-to-end MLOps structure |
| SageMaker Experiments | Track runs, parameters, metrics |
| SageMaker Studio | ML development workspace |
| SageMaker Notebooks | Write/experiment with ML code |

---

### SageMaker Model Registry
Used to:
- version models
- track model packages
- approve/reject models
- govern production promotion

**Exam clue:**  
> Model approval/version/governance → **Model Registry**

---

### SageMaker Experiments
Tracks:
- experiment runs
- hyperparameters
- metrics
- artifacts

**Exam clue:**  
> Compare training experiments → **SageMaker Experiments**

---

## 4. CI/CD for Machine Learning

Traditional CI/CD asks:

> Does the code work?

ML CI/CD asks:

> Does the code work, and is the model good enough to deploy?

Typical ML CI/CD:

```text
Source
  ↓
Build
  ↓
Test
  ↓
Train
  ↓
Evaluate
  ↓
Deploy
  ↓
Monitor
```

Deployment should often be **conditional**:

```text
Accuracy >= threshold?
   ├─ Yes → Register / Deploy
   └─ No  → Stop / Reject
```

Tests can include:
- unit tests
- integration tests
- data validation
- model performance tests
- regression tests

---

## 5. Git Workflows for ML Teams

### GitFlow
Branches:
- `main` → production
- `develop` → integration
- `feature/*` → features
- `experiment/*` → model experiments
- `release/*` → release preparation
- `hotfix/*` → urgent fixes

Best for:
- larger teams
- stricter release processes
- multiple environments

---

### GitHub Flow
Simpler process:

```text
main
 ↓
feature branch
 ↓
Pull Request
 ↓
Test + Model Evaluation
 ↓
Merge
 ↓
Deploy
```

Best for:
- small teams
- faster iteration
- continuous delivery

---

### Git + MLflow
Track the Git commit/branch with experiment results.

This answers:

> Which code produced this model?

**Exam memory:**
- Git → code/version history
- MLflow → experiments/metrics/models
- Together → reproducibility

---

## 6. Model Deployment Strategies

### In-place
Updates the existing environment directly.

Pros:
- low additional cost

Cons:
- possible downtime
- rollback can be harder

**Clue:** same servers/resources.

---

### Blue/Green
Two environments:

```text
Blue  = current production
Green = new version
```

Deploy and test Green, then switch traffic.

Pros:
- near-zero downtime
- easy rollback

Cons:
- higher cost because both environments exist

**Clue:** entirely new environment before switching traffic.

---

### Canary
Send a small percentage of traffic to the new version.

Example:

```text
90% old
10% new
   ↓
Monitor
   ↓
Increase gradually
```

Monitor:
- error rate
- latency
- business metrics
- user feedback

**Clue:** small traffic first.

---

### Linear
Shift traffic in fixed increments.

Example:

```text
10% → 20% → 30% → ... → 100%
```

**Clue:** increase traffic by a fixed percentage every few minutes.

---

### Rolling
Replace instances gradually.

Example:

```text
25% → 50% → 75% → 100%
```

**Clue:** replace a batch of instances at a time.

---

### Shadow
Mirror production traffic to the new model, but users still receive responses from the old model.

```text
Users → Old model → response
      ↘ New model → test only
```

**Clue:** copied traffic, new output is not shown to users.

---

### Deployment Strategy Cheat Sheet

| Clue | Strategy |
|---|---|
| Update same instances | In-place |
| Entire new environment | Blue/Green |
| Small traffic first | Canary |
| Fixed traffic steps | Linear |
| Replace instances gradually | Rolling |
| Mirror traffic silently | Shadow |

---

## 7. SageMaker Inference Options

### Real-Time Inference
Use when:
- sub-second response needed
- steady/sustained traffic
- endpoint must always be available

**Memory:** `NOW + steady traffic`

---

### Serverless Inference
Use when:
- traffic is sporadic/unpredictable
- cost optimization matters
- occasional cold starts are acceptable

Provisioned concurrency can reduce cold-start impact.

**Memory:** `NOW + sporadic traffic`

---

### Asynchronous Inference
Use when:
- requests are large
- processing takes a long time
- immediate response is not required
- requests should be queued

Typical flow:

```text
Request → Queue → Process → S3 result → SNS notification
```

**Memory:** `LATER + large/slow REQUEST`

---

### Batch Transform
Use when:
- you already have an offline dataset
- predictions can be processed in bulk
- no persistent endpoint is required

Typical flow:

```text
S3 dataset → Batch Transform → Predictions → S3
```

Resources are cleaned up after the job.

**Memory:** `LATER + whole DATASET`

---

### Inference Decision Table

| Requirement | Choose |
|---|---|
| Low latency + steady traffic | Real-Time |
| Low latency + sporadic traffic | Serverless |
| Large/slow individual requests | Async |
| Offline whole dataset | Batch Transform |

---

## 8. Deployment Targets

### SageMaker Endpoints
Best when:
- easiest managed ML serving is desired
- minimal operations overhead
- autoscaling and monitoring are needed

**Exam clue:** managed ML inference / team new to ML deployment.

---

### Amazon ECS
Use for:
- containerized applications
- no Kubernetes requirement
- simpler container orchestration than EKS

**Exam clue:** containers without Kubernetes.

---

### Amazon EKS
Use for:
- Kubernetes workloads
- maximum container control
- advanced networking/GPU scheduling
- portability of Kubernetes workloads

**Exam clue:** Kubernetes.

---

### AWS Lambda
Best for:
- lightweight event-driven inference
- short-lived serverless workloads
- smaller models and limited runtime requirements

---

## 9. Error Handling and Recovery

### Step Functions
Two important mechanisms:

**Retry**
- repeat failed operation
- useful for temporary/transient errors

**Catch**
- move to a failure-handling state when retry fails

Typical flow:

```text
Task fails
  ↓
Retry
  ↓
Still fails?
  ↓
Catch
  ↓
HandleFailure / Notify
```

### Exponential Backoff
Example:

```text
30 sec → 60 sec → 120 sec
```

Prevents repeatedly hammering a failing service.

### CodePipeline Recovery
Use:
- automatic retry
- manual approval gates
- rollback
- notifications

**Exam memory:**

```text
Retry    = try again
Backoff  = wait longer
Catch    = failure handler
Rollback = return to previous version
```

---

## 10. Best Practices for ML Orchestration

### Idempotency
An operation can be repeated safely without creating incorrect duplicate effects.

Use:
- state checks
- unique resource identifiers
- deterministic behavior

**Exam clue:** safe to repeat.

---

### Monitoring and Observability
Collect:
- logs
- metrics
- alerts
- pipeline/job status

Important services:
- CloudWatch
- Model Monitor
- CloudTrail
- X-Ray

---

### Resource Management
Use:
- automatic cleanup
- tags for cost tracking
- timeouts
- right-sized compute
- limits and quotas

---

### Parallelization
Run independent tasks at the same time.

Example:

```text
Train Model A ─┐
               ├→ Evaluate
Train Model B ─┘
```

Use a Step Functions **Parallel** state.

---

### Step Functions Express
Best for:
- high-volume
- short-duration workflows
- event-driven execution at scale

---

## 11. Monitoring and Observability

### Amazon CloudWatch
Provides:
- metrics
- logs
- alarms
- dashboards

**Exam clue:** monitoring, logs, alarms.

---

### AWS X-Ray
Provides distributed request tracing.

**Exam clue:** trace a request across services.

---

### AWS Config
Tracks:
- resource configuration
- compliance
- configuration history

**Exam clue:** resource configuration/compliance.

---

### AWS CloudTrail
Records:
- AWS API activity
- who performed an action
- when it happened

**Exam clue:** audit / who did what.

---

### SageMaker Model Monitor
Detects:
- data quality issues
- model quality degradation
- drift

**Exam clue:** production ML drift/quality monitoring.

---

## 12. Event-Driven Automation

### Amazon EventBridge
Used to:
- trigger pipelines from events
- schedule workflows
- respond to AWS service events

Example:

```text
Model Monitor detects drift
      ↓
EventBridge
      ↓
Step Functions / SageMaker Pipeline
      ↓
Retrain model
```

**Exam clue:** event/schedule trigger.

---

### Amazon SNS
Used for notifications.

Examples:
- training complete
- deployment failed
- approval required
- inference result ready

**Exam clue:** send notifications.

---

## 13. Infrastructure as Code

### AWS CloudFormation
Declarative IaC using YAML/JSON.

Important concepts:
- **Template** → blueprint
- **Stack** → deployed resources
- **Parameters** → inputs
- **Resources** → infrastructure to create
- **Outputs** → values returned/shared
- **Mappings** → lookup tables
- **Conditions** → conditional resource creation
- **Change Sets** → preview changes
- **StackSets** → deploy across accounts/regions

**Exam clue:** declarative YAML/JSON infrastructure.

---

### AWS CDK
Define infrastructure using programming languages such as Python or TypeScript.

Important commands:

```bash
cdk synth
cdk diff
cdk deploy
cdk destroy
```

CDK synthesizes to CloudFormation.

Construct levels:
- **L1** → low-level direct CloudFormation resources
- **L2** → higher-level AWS abstractions with defaults/best practices
- **L3** → complete architecture patterns

**Exam clue:** programming language IaC → CDK.

---

## 14. Containers and Deployment Artifacts

### Amazon ECR
Stores Docker/container images.

Features:
- image versioning
- vulnerability scanning
- lifecycle policies

**Exam clue:** container registry.

---

### Custom vs Prebuilt SageMaker Containers

Use **SageMaker prebuilt containers** when:
- standard framework
- normal dependencies
- less maintenance desired

Use **custom containers** when:
- non-standard framework
- custom runtime/dependencies
- special preprocessing
- full control required

---

## 15. Edge Deployment

### SageMaker Neo
Compiles/optimizes ML models for specific edge hardware.

**Memory:** Neo = **prepare/optimize**.

### AWS IoT Greengrass
Runs ML workloads on edge devices.

**Memory:** Greengrass = **run at edge**.

---

## 16. Security Best Practices

Use:
- **IAM roles** instead of hardcoded access keys
- **least privilege**
- **AWS Secrets Manager** for passwords/API keys
- encrypted artifact storage
- vulnerability scanning
- secure container images

**Exam memory:**

```text
Secrets     → Secrets Manager
Permissions → IAM least privilege
Audit       → CloudTrail
Encryption  → KMS / encrypted storage
```

---

## 17. Performance and Cost Optimization

### Build/Pipeline Optimization
- cache dependencies
- parallelize independent actions
- minimize artifact size
- optimize Docker layers
- fail fast when tests fail

### Compute Selection
- right-size instances
- use Spot where interruptions are acceptable
- autoscale production endpoints
- remove idle resources

### Key Delivery Metrics
Track:
- build success rate
- build duration
- deployment frequency
- deployment success rate
- lead time
- MTTR

---

# Ultimate Exam Cheat Sheet

## Orchestration

```text
SageMaker Pipelines → ML workflow
Step Functions      → AWS service workflow / state machine
MWAA                → Airflow / Python DAGs
CodePipeline        → CI/CD orchestration
```

## Code Family

```text
CodeCommit   → Store source code
CodeBuild    → Build + Test
CodeDeploy   → Deploy
CodePipeline → Connect CI/CD stages
CodeArtifact → Store packages
```

## SageMaker MLOps

```text
Projects       → MLOps templates
Experiments    → Track experiments
Model Registry → Version/approve models
Model Monitor  → Detect drift
```

## Inference

```text
Real-Time  → NOW + steady traffic
Serverless → NOW + sporadic traffic
Async      → LATER + large/slow request
Batch      → LATER + whole dataset
```

## Deployment

```text
In-place   → Same instances
Rolling    → Replace instances gradually
Canary     → Small traffic first
Linear     → Fixed traffic increments
Blue/Green → New environment then switch
Shadow     → Mirror traffic silently
```

## Observability

```text
CloudWatch → Logs + metrics + alarms
X-Ray      → Distributed traces
Config     → Resource configuration/compliance
CloudTrail → API audit / who did what
```

## Automation

```text
EventBridge → Trigger workflow
SNS         → Notify
Retry       → Try again
Catch       → Handle failure
Rollback    → Previous working version
```

---

# Final Memory Map

```text
Developer commits code
        ↓
CodeCommit / GitHub
        ↓
CodePipeline
        ↓
CodeBuild
Build + Test
        ↓
Step Functions / SageMaker Pipelines
        ↓
Process → Train → Evaluate
        ↓
Condition
        ↓
Model Registry
        ↓
Deploy
Real-Time / Serverless / Async / Batch
        ↓
CloudWatch + Model Monitor
        ↓
Drift / Failure detected
        ↓
EventBridge
        ↓
Retrain / Rollback / Notify
        ↓
SNS
```

## One-Line Domain 4 Memory

> **Build → Orchestrate → Evaluate → Register → Deploy → Monitor → Retrain**
