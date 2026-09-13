# AWS MLA-C01 — Domain 5
# ML Solution Monitoring, Maintenance, and Security

> Exam-focused summary based on our full Domain 5 discussion.

---

## 1. Model Drift and Monitoring

Production ML models can degrade because input data, prediction quality, fairness, or feature importance changes over time.

### Four Main Drift Types

| Drift Type | Main Question | What Changes? | Key Detection |
|---|---|---|---|
| **Data Quality Drift** | Did the input data change? | Feature distributions, missing values, ranges | Statistics, constraints, distribution tests |
| **Model Quality Drift** | Are predictions getting worse? | Prediction performance vs actual outcomes | Accuracy, Precision, Recall, F1, AUC, MAE, MSE, R² |
| **Bias Drift** | Is the model becoming unfair? | Fairness across groups/facets | Bias/fairness metrics |
| **Feature Attribution Drift** | Why is the model predicting differently? | Feature contribution / importance | SHAP / feature attribution |

### Memory Trick

- **Data Quality** = INPUT changed
- **Model Quality** = OUTPUT became worse
- **Bias Drift** = FAIRNESS changed
- **Feature Attribution Drift** = WHY the prediction changed

---

## 2. SageMaker Model Monitor

**Amazon SageMaker Model Monitor** continuously monitors deployed models and detects deviations from expected behavior.

### Core Workflow

```text
Training / Reference Data
        ↓
Create Baseline
        ↓
Capture Production Data
        ↓
Compare Against Baseline
        ↓
Detect Violations
        ↓
Reports in S3
        ↓
CloudWatch Metrics / Alarms
        ↓
Notification or Automated Action
```

### Four Monitoring Types

1. **Data Quality Monitoring**
2. **Model Quality Monitoring**
3. **Bias Drift Monitoring**
4. **Feature Attribution Drift Monitoring**

---

## 3. Data Quality Monitoring

Data Quality Monitoring compares **production input data** with the training/reference baseline.

### Main Steps

1. Enable endpoint data capture.
2. Create a baseline.
3. Generate statistics and constraints.
4. Schedule monitoring jobs.
5. Detect violations.
6. Send metrics to CloudWatch.

### Important Files

- `statistics.json` → statistical characteristics of features
- `constraints.json` → expected rules/limits
- `constraints_violations.json` → detected violations

### Common Checks

| Check | Meaning |
|---|---|
| `data_type_check` | Wrong/unexpected data type |
| `completeness_check` | Missing/null values |
| `baseline_drift_check` | Distribution changed |
| `missing_column_check` | Expected column missing |
| `extra_column_check` | Unexpected column |
| `categorical_values_check` | Unexpected category |

### Exam Clue

> Production feature distribution changed / missing values increased / mean or standard deviation changed / outliers appeared

→ **Data Quality Monitor**

---

## 4. Model Quality Monitoring

Model Quality Monitoring evaluates whether predictions still match **ground truth**.

### Requires

- Baseline performance
- Captured model predictions
- Ground truth labels

### Important Concept: `eventId`

`eventId` matches a prediction with its actual ground truth outcome.

### Time Window

If labels arrive later, use:

- `start_time_offset`
- `end_time_offset`

This ensures only predictions with available ground truth are evaluated.

### Classification Metrics

- Accuracy
- Precision
- Recall
- F1
- AUC
- TPR / TNR
- FPR / FNR
- Confusion Matrix

### Regression Metrics

- MAE
- MSE
- R²

### Exam Clue

> Ground truth / predictions vs actual labels / accuracy or F1 dropped / MAE increased

→ **Model Quality Monitor**

---

## 5. SageMaker Clarify

**SageMaker Clarify** focuses on:

- Bias detection
- Explainability
- Feature attribution
- SHAP values

### Bias Drift Monitoring

Bias monitoring checks whether the model remains fair across different groups.

A **facet** is the feature/column used to compare groups, such as age, gender, or location.

Typical bias metrics:

- Disparate impact
- Equal opportunity difference
- Average odds difference

**Exam clue:** fairness, demographic group, protected group, facet, different approval rates → **Bias Monitor + Clarify**

### Feature Attribution Drift

Feature attribution asks:

> Which features are influencing the model's predictions?

SageMaker Clarify uses **SHAP values**.

**Exam clue:** SHAP, feature importance, explainability, feature contribution changed → **Feature Attribution Monitoring + Clarify**

---

## 6. SageMaker Model Dashboard

**SageMaker Model Dashboard** provides a centralized view of model health.

Main features:

- Monitoring alerts
- Risk ratings
- Endpoint performance
- Batch Transform activity
- Model lineage
- Model details
- Model Cards

### Key Difference

- **Model Monitor** = detects model/data issues
- **Model Dashboard** = centralizes and displays model health

---

## 7. A/B Testing

A/B testing compares multiple model variants using real production traffic.

```text
SageMaker Endpoint
       ↓
Traffic Split
   ↙       ↘
Model A   Model B
```

### A/B vs Canary vs Monitoring

| Technique | Main Question |
|---|---|
| **A/B Testing** | Which model is better? |
| **Canary Deployment** | Can I safely roll out the new model? |
| **Model Monitor** | Is the model degrading over time? |

---

## 8. Tailoring Monitoring Strategies

Monitoring should depend on:

| Factor | Impact |
|---|---|
| **Model Complexity** | More complex models may need deeper monitoring |
| **Application Criticality** | High-risk systems require stricter monitoring |
| **Monitoring Requirements** | Real-time vs batch / continuous vs periodic |
| **Resource Availability** | Balance monitoring depth with cost and compute |

Additional monitoring areas:

- Input validation
- Output anomalies
- Latency
- Throughput
- User behavior
- Traffic patterns
- Errors and exceptions

---

## 9. Amazon CloudWatch

**Amazon CloudWatch** monitors operational health and performance.

Main features:

- Metrics
- Logs
- Dashboards
- Alarms
- Trend analysis
- Cross-account observability

Common ML metrics:

- CPU utilization
- Memory utilization
- Disk utilization
- GPU utilization
- Endpoint latency
- Invocation count
- Error rate
- Throughput

> **CloudWatch = What is happening?**

---

## 10. Monitoring vs Observability

| Concept | Main Question |
|---|---|
| **Monitoring** | What is wrong? |
| **Observability** | Why did it happen? |

Monitoring uses metrics, thresholds, and alarms. Observability adds logs, traces, model explanations, data lineage, and model version history for root-cause analysis.

---

## 11. AWS X-Ray

**AWS X-Ray** provides distributed tracing.

Use it to:

- Trace requests across services
- Find latency bottlenecks
- Trace failed requests
- Understand service dependencies

> **X-Ray = Where is the latency/error happening?**

---

## 12. CloudWatch Lambda Insights

Use **Lambda Insights** for deep Lambda performance monitoring.

It helps with:

- Cold starts
- Memory utilization
- Execution duration
- Invocation patterns
- Function performance

> **Lambda Insights = How is Lambda performing?**

---

## 13. CloudWatch Logs Insights

Use **CloudWatch Logs Insights** to query and analyze logs.

Main uses:

- Search errors
- Aggregate failures
- Analyze latency
- Compare time periods
- Detect patterns
- Investigate anomalies

> **Logs Insights = What do the logs reveal?**

---

## 14. AWS CloudTrail

**AWS CloudTrail** records API activity.

It answers:

- Who made the request?
- What action was performed?
- When?
- From which IP?
- On which AWS resource?

Use cases:

- Security auditing
- Compliance
- Forensics
- Troubleshooting
- Resource-change investigation

> **CloudTrail = Who did what?**

---

## 15. Amazon Athena

**Amazon Athena** runs SQL queries directly against data in Amazon S3.

Typical use:

- Query CloudTrail logs
- Query CUR billing data
- Analyze stored monitoring logs

> **Athena = Query S3 with SQL**

---

## 16. Amazon QuickSight

**Amazon QuickSight** is a business intelligence and visualization service.

Features:

- Dashboards
- Visual analytics
- Anomaly detection
- Forecasting
- Autonarratives

> **QuickSight = Visualize**

---

## 17. Amazon EventBridge

**Amazon EventBridge** is a serverless event bus.

It reacts to events such as:

- Training job state changes
- Endpoint state changes
- Pipeline execution changes
- Processing job changes
- Model deployment changes

Possible targets:

- Lambda
- SNS
- Step Functions
- Kinesis

> **EventBridge = React / Automate**

---

## 18. Amazon SNS

Use **Amazon SNS** for notifications such as email, SMS, and push notifications.

Typical flow:

```text
CloudWatch Alarm
      ↓
SNS
      ↓
Email / SMS
```

---

## 19. Main Monitoring Tool Comparison

| Service | Purpose |
|---|---|
| **CloudWatch** | Monitor |
| **CloudTrail** | Audit |
| **Athena** | Query |
| **X-Ray** | Trace |
| **QuickSight** | Visualize |
| **EventBridge** | React / Automate |
| **SNS** | Notify |

---

## 20. SageMaker Inference Recommender

**Inference Recommender** helps choose the best SageMaker inference deployment configuration.

It considers:

- Latency
- Throughput
- Cost
- Memory
- Instance type
- Traffic patterns

### Job Types

- **Default Recommendation** → quick starting point, standardized tests, minimal setup
- **Advanced Recommendation** → custom traffic, latency/throughput requirements, candidate instances

**Exam clue:** best instance type/configuration for SageMaker inference → **Inference Recommender**

---

## 21. AWS Compute Optimizer

**AWS Compute Optimizer** analyzes actual infrastructure utilization and recommends rightsizing.

Useful for:

- EC2
- Auto Scaling groups
- CPU utilization
- Memory utilization
- GPU utilization

| Inference Recommender | Compute Optimizer |
|---|---|
| SageMaker model serving | General infrastructure |
| Model load testing | Historical resource usage |
| Optimize inference | Rightsize infrastructure |

---

## 22. Auto Scaling

### Target Tracking
Maintain a metric around a target value.

### Step Scaling
Scale differently depending on alarm severity.

### Scheduled Scaling
Scale at known recurring times.

### Exam Memory

- Metric target → **Target Tracking**
- Different action by alarm severity → **Step Scaling**
- Predictable recurring time → **Scheduled Scaling**

---

## 23. SageMaker Serverless Inference

Use **Serverless Inference** when traffic is irregular, infrequent, unpredictable, or has long idle periods.

Benefits:

- No infrastructure management
- Automatically scales
- Can scale to zero
- Pay-per-use

---

## 24. Provisioned Concurrency

Provisioned Concurrency keeps serverless inference capacity warm.

Use it when:

- Cold starts are unacceptable
- Low latency is required
- Predictable bursts occur

> Serverless = irregular traffic  
> Provisioned Concurrency = avoid cold starts

---

## 25. Capacity Blocks for ML

**EC2 Capacity Blocks for ML** reserve GPU capacity for a future time window.

Use when:

- GPU availability must be guaranteed
- Training/fine-tuning is scheduled
- High-demand GPU instances are required

> **Capacity Block = Guaranteed future GPU capacity**

---

## 26. AWS Cost Explorer

**AWS Cost Explorer** analyzes AWS spending.

Use it to:

- View cost trends
- Find cost drivers
- Compare projects
- Forecast future cost
- Analyze RI usage

> **Cost Explorer = Analyze cost**

---

## 27. AWS Budgets

**AWS Budgets** creates financial guardrails.

Use it to:

- Set spending thresholds
- Alert on actual costs
- Alert on forecasted costs
- Trigger actions

> **Budgets = Limit / Alert**

---

## 28. AWS Cost and Usage Report (CUR)

**CUR** provides the most granular AWS billing and usage data.

Use it for:

- Detailed line items
- Hourly/daily billing detail
- Resource-level cost attribution
- External BI/analytics

> **CUR = Deep billing detail**

---

## 29. AWS Trusted Advisor

**Trusted Advisor** gives recommendations for:

- Cost optimization
- Performance
- Security
- Fault tolerance

Examples:

- Idle resources
- Oversized resources
- Savings opportunities

> **Trusted Advisor = Recommend**

---

## 30. Cost Allocation Tags

Tags assign AWS costs to:

- Project
- Team
- Department
- Environment
- Cost center

> **Tags = Allocate cost**

---

## 31. Amazon S3 Analytics

**S3 Analytics** helps optimize storage classes by identifying access patterns and data that can move to cheaper storage tiers.

> **S3 Analytics = Storage cost optimization**

---

## 32. AWS Purchasing Options

| Option | Best For |
|---|---|
| **On-Demand** | Short-term / unpredictable workloads |
| **Spot** | Interruptible ML training / batch jobs |
| **Reserved Instances** | Predictable long-term EC2 usage |
| **Savings Plans** | Consistent long-term usage with flexibility |
| **Capacity Blocks** | Guaranteed future GPU capacity |

### Fast Memory

- **On-Demand** = Flexible
- **Spot** = Cheapest
- **Reserved** = Predictable long-term
- **Savings Plan** = Flexible commitment
- **Capacity Block** = Guaranteed GPU

---

## 33. Shared Responsibility Model

### AWS
Responsible for **Security OF the Cloud**:

- Data centers
- Hardware
- Networking
- Underlying cloud infrastructure

### Customer
Responsible for **Security IN the Cloud**:

- Data
- Models
- IAM
- Encryption settings
- Applications
- Compliance configuration

> AWS = OF  
> Customer = IN

---

## 34. AWS IAM

**AWS IAM** controls access to AWS resources using users, groups, roles, and policies.

### Principle of Least Privilege

Give only the minimum permissions required.

---

## 35. IAM Policy Types

### Identity-Based Policies
Attached to users, groups, and roles.

### Resource-Based Policies
Attached to supported resources such as S3 buckets and VPC endpoints.

---

## 36. SageMaker Role Manager

Use **SageMaker Role Manager** to create and manage persona-based SageMaker IAM roles.

Examples:

- Data scientist
- ML engineer
- Data analyst

**Exam clue:** persona-based SageMaker permissions → **SageMaker Role Manager**

---

## 37. AWS Organizations and SCPs

**AWS Organizations** manages multiple AWS accounts.

### Organizational Units (OU)
Group accounts.

### Service Control Policies (SCP)
Set permission guardrails for accounts/OUs.

> IAM = identity permissions  
> SCP = account/OU guardrails

---

## 38. Amazon VPC

Use a **VPC** to isolate ML resources.

Sensitive workloads should often use private subnets with no direct public internet access.

> **VPC = Isolate**

---

## 39. VPC Endpoint / AWS PrivateLink

Provides private access from a VPC to supported AWS services without public internet access.

> **VPC Endpoint / PrivateLink = Private AWS access**

---

## 40. Endpoint Policies

An endpoint policy controls:

- Who can use the endpoint
- Which actions are allowed
- Which resources can be accessed

> Endpoint = private path  
> Endpoint Policy = rules for that path

---

## 41. NAT Gateway

A **NAT Gateway** allows resources in private subnets to initiate outbound internet connections.

> **NAT Gateway = Outbound internet**

---

## 42. Security Groups

Security Groups are resource/ENI-level virtual firewalls controlling inbound and outbound traffic.

> **Security Group = Resource firewall**

---

## 43. Network ACLs (NACLs)

NACLs control traffic at the **subnet** level and support allow/deny rules.

> **NACL = Subnet firewall**

---

## 44. AWS Network Firewall

Use **AWS Network Firewall** for deeper traffic inspection.

Capabilities include:

- IDS/IPS
- Web filtering
- Domain filtering
- Content inspection
- Advanced traffic rules

> **Network Firewall = Deep inspection**

---

## 45. VPC Flow Logs

**VPC Flow Logs** capture network traffic metadata.

Use them for:

- Suspicious IP investigation
- Unauthorized connections
- Network troubleshooting
- Traffic analysis

> **Flow Logs = Network traffic evidence**

---

## 46. AWS KMS

**AWS Key Management Service (KMS)** manages encryption keys.

Use it for:

- Encryption at rest
- Customer-managed keys
- Key policies
- Compliance

> **KMS = Data sitting**

---

## 47. TLS

**TLS** protects data in transit.

> **TLS = Data moving**

---

## 48. AWS Secrets Manager

Use **Secrets Manager** for:

- Passwords
- API keys
- Database credentials
- Secret rotation

> **Secrets Manager = Store and rotate secrets**

---

## 49. SageMaker Security Monitoring

### CloudWatch
Use for metrics, dashboards, alarms, and health monitoring.

### CloudTrail
Use for API auditing, compliance, and security investigations.

> IAM = Access  
> CloudWatch = Monitor  
> CloudTrail = Audit

---

## 50. SageMaker Security Troubleshooting

```text
Unauthorized Access
      ↓
CloudTrail
      ↓
IAM Policies
      ↓
VPC Flow Logs
      ↓
Data Access Events
      ↓
KMS / Encryption
```

| Investigation | Tool |
|---|---|
| Who made API request? | CloudTrail |
| Who accessed data? | CloudTrail Data Events / relevant access logging |
| Permissions too broad? | IAM |
| Suspicious network traffic? | VPC Flow Logs |
| Private network access? | PrivateLink / VPC Endpoint |
| Encryption issue? | KMS |

---

## 51. Compliance Frameworks

| Framework | Main Association |
|---|---|
| **ISO 27001** | Information security management |
| **SOC 2** | Security and operational controls |
| **PCI-DSS** | Payment card data |
| **HIPAA** | Healthcare / PHI |
| **FedRAMP** | U.S. federal government cloud |

### Memory

- ISO = Security
- SOC 2 = Controls
- PCI-DSS = Cards
- HIPAA = Health
- FedRAMP = Government

---

## 52. CI/CD Pipeline Security

| Stage | Security Technique |
|---|---|
| **Pre-Commit** | Pre-commit hooks / IDE security tools |
| **Commit** | SAST |
| **Build** | SCA |
| **Test** | DAST / IAST |
| **Deploy** | Penetration Testing |
| **Monitor** | Red / Blue / Purple Teams |

### SAST
Static source-code security testing.

> **SAST = Source Code**

### SCA
Scans dependencies, open-source packages, CVEs, licenses, and IaC manifests.

> **SCA = Components / Dependencies**

### DAST
Tests a running application externally using a black-box approach.

> **DAST = Running application**

### IAST
Combines runtime testing with internal code/application context.

> **IAST = Runtime + internal visibility**

### Security Teams

- **Red Team** = Attack
- **Blue Team** = Defend
- **Purple Team** = Red + Blue collaboration

---

## 53. CI/CD Infrastructure Security Services

| AWS Service | Purpose |
|---|---|
| **CloudFormation** | Infrastructure as Code |
| **AWS Config** | Configuration/compliance monitoring |
| **CloudTrail** | Audit changes |
| **Secrets Manager** | Store/rotate secrets |
| **IAM** | Access and least privilege |
| **CodeBuild** | Automate build/security tests |
| **Lambda** | Automate security checks/actions |

---

## 54. Important Service Comparisons

### CloudWatch vs CloudTrail vs Athena vs X-Ray

| Service | Main Question |
|---|---|
| **CloudWatch** | What is happening? |
| **CloudTrail** | Who did what? |
| **Athena** | How can I query stored data/logs? |
| **X-Ray** | Where is the latency/error? |

### Model Monitor vs Clarify

| Service | Focus |
|---|---|
| **Model Monitor** | Continuous production monitoring |
| **Clarify** | Bias + explainability / SHAP |

### Inference Recommender vs Compute Optimizer

| Service | Focus |
|---|---|
| **Inference Recommender** | SageMaker serving optimization |
| **Compute Optimizer** | General infrastructure rightsizing |

### Security Group vs NACL

| Security Group | NACL |
|---|---|
| Resource/ENI level | Subnet level |
| Resource firewall | Subnet firewall |

### VPC Endpoint vs NAT Gateway

| VPC Endpoint | NAT Gateway |
|---|---|
| Private AWS service access | Outbound internet access |
| Traffic stays private | Used when internet is needed |

### Cost Explorer vs Budgets vs CUR

| Tool | Purpose |
|---|---|
| **Cost Explorer** | Analyze and forecast spending |
| **Budgets** | Set threshold and alert |
| **CUR** | Detailed billing data |

---

## 55. Master Exam Cheat Sheet

| Keyword / Scenario | Answer |
|---|---|
| Input distribution changed | Data Quality Monitor |
| Ground truth / F1 dropped | Model Quality Monitor |
| Fairness / demographic group | Bias Monitor + Clarify |
| SHAP / feature importance | Feature Attribution + Clarify |
| Compare models with live traffic | A/B Testing |
| Gradual rollout | Canary |
| Central model health view | Model Dashboard |
| Metrics / alarms | CloudWatch |
| API audit | CloudTrail |
| SQL over S3 | Athena |
| Distributed tracing | X-Ray |
| Lambda cold start | Lambda Insights |
| Search logs | Logs Insights |
| BI visualization | QuickSight |
| React to state change | EventBridge |
| Notify by email/SMS | SNS |
| Best SageMaker inference instance | Inference Recommender |
| General EC2 rightsizing | Compute Optimizer |
| Analyze AWS spend | Cost Explorer |
| Cost threshold | AWS Budgets |
| Cost recommendation | Trusted Advisor |
| Most detailed billing | CUR |
| Allocate cost by project | Cost Allocation Tags |
| Cheap interruptible training | Spot |
| Guaranteed future GPU | Capacity Blocks |
| Irregular inference | Serverless Inference |
| Avoid cold starts | Provisioned Concurrency |
| Maintain target metric | Target Tracking |
| Alarm severity scaling | Step Scaling |
| Known recurring schedule | Scheduled Scaling |
| Minimum permissions | IAM / Least Privilege |
| Persona SageMaker roles | SageMaker Role Manager |
| Multi-account guardrails | Organizations + SCP |
| Private AWS access | VPC Endpoint / PrivateLink |
| Outbound private subnet internet | NAT Gateway |
| Resource firewall | Security Group |
| Subnet firewall | NACL |
| Advanced traffic inspection | Network Firewall |
| Network traffic logs | VPC Flow Logs |
| Encryption at rest | KMS |
| Encryption in transit | TLS |
| Store/rotate secrets | Secrets Manager |
| Resource configuration compliance | AWS Config |
| Payment cards | PCI-DSS |
| Healthcare | HIPAA |
| U.S. government | FedRAMP |
| Static source code scan | SAST |
| Dependency scan | SCA |
| Running app black-box test | DAST |
| Runtime + internal context | IAST |

---

## 56. Final Memory Map

```text
MODEL
├── Model Monitor = Drift
├── Clarify = Bias + SHAP
└── Model Dashboard = Central model health

OPERATIONS
├── CloudWatch = Monitor
├── CloudTrail = Audit
├── Athena = Query
├── X-Ray = Trace
├── QuickSight = Visualize
└── EventBridge = React

OPTIMIZATION
├── Inference Recommender = Model serving
├── Compute Optimizer = Infrastructure
├── Cost Explorer = Cost analysis
├── Budgets = Financial guardrail
├── CUR = Detailed billing
└── Trusted Advisor = Recommendations

SECURITY
├── IAM = Access
├── KMS = Encryption
├── Secrets Manager = Secrets
├── VPC = Isolation
├── PrivateLink = Private connectivity
├── NAT = Internet egress
├── Security Group = Resource firewall
├── NACL = Subnet firewall
└── Network Firewall = Deep inspection
```

---

# Final 20-Second Review

> **Model Monitor = Detect drift**  
> **Clarify = Bias + SHAP**  
> **CloudWatch = Monitor**  
> **CloudTrail = Audit**  
> **Athena = Query**  
> **X-Ray = Trace**  
> **EventBridge = React**  
> **Inference Recommender = Optimize SageMaker inference**  
> **Compute Optimizer = Rightsize infrastructure**  
> **Cost Explorer = Analyze spend**  
> **Budgets = Cost alerts**  
> **IAM = Access**  
> **KMS = Encryption**  
> **VPC Endpoint = Private AWS access**  
> **NAT Gateway = Outbound internet**  
> **Security Group = Resource firewall**  
> **NACL = Subnet firewall**
