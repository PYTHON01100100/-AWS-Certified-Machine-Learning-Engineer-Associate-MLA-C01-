# MLA-C01 Domain 2 — Data Preparation for Machine Learning

## 1. Big Picture

Use this mental flow:

```text
Data Sources
   ↓
DMS / DataSync / Kinesis / Firehose
   ↓
Amazon S3
   ↓
Glue / SageMaker Data Wrangler / SageMaker Processing
   ↓
SageMaker Feature Store
   ↓
Training / Inference
```

The exam usually asks:

1. Where is the data?
2. How should it be moved?
3. How should it be transformed?
4. How should features be stored and reused?
5. Which option gives the **least operational overhead**?

---

# 2. Core AWS Services

## Amazon S3

**Purpose:** Main object storage for ML datasets.

Use for:
- CSV, JSON, Parquet
- Images, audio, logs
- Raw and processed datasets
- Model artifacts
- Data lake storage

**Exam clues:**
- Large training dataset
- Data lake
- Durable low-cost storage
- Raw + processed ML data

### Important features
- **Versioning** → keeps multiple object versions
- **Lifecycle policies** → move/delete objects automatically
- **SSE-S3** → S3-managed encryption
- **SSE-KMS** → KMS-based encryption
- **Event Notifications** → trigger workflows when data arrives

> **Memory:** S3 = default storage for ML datasets.

---

## AWS Glue

**Purpose:** Serverless ETL and data integration.

```text
Extract → Transform → Load
```

Use for:
- Cleaning data
- Joining datasets
- Format conversion
- Large-scale ETL
- Spark-based transformations
- Schema discovery

**Exam clues:**
- Serverless ETL
- Large-scale transformation
- Spark jobs
- General data engineering

> **Glue = general ETL**  
> **Data Wrangler = ML-specific preparation**

---

## AWS Glue Data Catalog

Stores **metadata**, not the actual data.

Contains information such as:
- Table names
- Columns
- Schemas
- Locations in S3

Used by:
- Athena
- Glue
- EMR
- Redshift Spectrum

**Exam clue:** Central metadata repository → **Glue Data Catalog**

---

## AWS Glue Crawler

Automatically scans data and detects its schema.

```text
S3 → Glue Crawler → Glue Data Catalog
```

**Exam clue:** Automatically discover schema → **Glue Crawler**

---

## Amazon SageMaker Data Wrangler

**Purpose:** Visual / low-code ML data preparation.

Use for:
- Categorical encoding
- Missing values
- Normalization
- Scaling
- Outlier handling
- Feature transformations
- Class imbalance
- Feature analysis
- Data visualization

**Exam clues:**
- Prepare data for ML
- Feature engineering
- Visual / low-code
- Least operational overhead
- Categorical → numerical

> If the question is specifically about **ML preprocessing** and asks for **least operational overhead**, Data Wrangler is often the best choice.

---

## SageMaker Processing

Runs managed preprocessing jobs using:
- Python
- Scikit-learn
- Spark
- Custom containers

Use for:
- Custom preprocessing scripts
- Feature engineering
- Model evaluation
- Data validation
- Post-processing

```text
S3 → Processing Job → preprocessing.py → S3
```

**Exam clue:** Custom preprocessing code → **SageMaker Processing**

---

## SageMaker Feature Store

Stores reusable ML features.

Examples:
- customer_age
- transaction_count
- average_spending
- fraud_score

### Online Store
Use for:
- Real-time inference
- Low-latency feature retrieval

### Offline Store
Use for:
- Historical training data
- Batch processing
- Analytics

> **Online = inference**  
> **Offline = training/history**

---

## Amazon Athena

Serverless SQL directly on S3.

```sql
SELECT * FROM transactions
WHERE fraud = true;
```

**Exam clues:**
- Query S3 with SQL
- Serverless
- Ad-hoc analysis

→ **Athena**

---

## Amazon Redshift

Cloud data warehouse for analytical SQL.

Use for:
- OLAP
- Repeated analytical queries
- Large structured analytics workloads

### Athena vs Redshift

| Athena | Redshift |
|---|---|
| Query S3 directly | Data warehouse |
| Serverless/ad-hoc | Persistent analytics platform |
| No data loading required | Data often loaded into warehouse |

---

## Amazon EMR

Managed big-data platform.

Supports:
- Spark
- Hadoop
- Hive
- Trino / Presto

**Exam clue:** Existing Spark/Hadoop workload → **EMR**

If the question emphasizes **least operational overhead**, Glue may be preferred over a managed cluster approach.

---

## AWS Database Migration Service (DMS)

Moves or replicates **databases**.

Example:

```text
On-prem MySQL → AWS DMS → RDS / S3 / Redshift
```

Supports:
- Full migration
- Continuous replication
- Change Data Capture (CDC)

**Exam clue:** Move on-prem database → **DMS**

---

## AWS DataSync

Moves **files**.

Example:

```text
On-prem NAS → DataSync → S3 / EFS / FSx
```

**Exam clue:** Transfer files from on-premises → **DataSync**

### DMS vs DataSync

```text
Database → DMS
Files    → DataSync
```

---

## Amazon Kinesis Data Streams

For real-time streaming where applications need direct access to records.

Use when:
- Multiple consumers
- Custom stream processing
- Replay is needed

---

## Amazon Data Firehose

Managed delivery of streaming data to destinations such as:
- S3
- Redshift
- OpenSearch

**Exam clue:** Stream data to S3 with minimal administration → **Firehose**

### Streams vs Firehose

```text
Kinesis Data Streams
→ You process the stream

Firehose
→ AWS delivers the stream
```

---

## Amazon EFS

Shared NFS filesystem.

Use when:
- Multiple instances need shared file storage
- Normal filesystem semantics are required

Important:
- EFS does **not** provide S3-style object versioning

---

## Amazon FSx for Lustre

High-performance filesystem for:
- ML training
- HPC
- High-throughput workloads

**Exam clue:** Extremely high file-system throughput for training → **FSx for Lustre**

### EFS vs FSx for Lustre

| EFS | FSx for Lustre |
|---|---|
| General shared filesystem | High-performance ML/HPC filesystem |
| NFS | Lustre |
| General workloads | Very high throughput |

---

## SageMaker Ground Truth

Used for data labeling.

Use for:
- Image labeling
- Text labeling
- Human labeling workflows
- Automated labeling

**Exam clue:** Label training data → **Ground Truth**

---

## AWS Lake Formation

Builds and governs a data lake.

Use for:
- Fine-grained data permissions
- Centralized data lake governance
- Access control across analytics services

> **Glue = ETL + metadata**  
> **Lake Formation = governance + permissions**

---

## IAM

Controls access to AWS resources.

Example:
- SageMaker execution role accessing S3

**Exam rule:** Prefer IAM roles over hardcoded access keys.

---

## AWS KMS

Encryption key management.

Used with:
- S3
- SageMaker
- EFS
- Databases
- Feature Store

### AWS-managed vs customer-managed keys

- **AWS-managed key** → easier, less control
- **Customer-managed KMS key** → more control, custom policies, rotation options

---

# 3. Data Preprocessing Techniques

## Categorical Encoding

Converts categories to numerical representations.

### One-Hot Encoding

Use for categories with **no natural order**.

```text
Country:
Saudi
USA
UK

↓

Saudi USA UK
1     0   0
0     1   0
0     0   1
```

Use for:
- Country
- City
- Color
- Product type

**Exam memory:** No order → **One-hot**

---

### Ordinal Encoding

Use when categories have a natural order.

```text
Low    → 1
Medium → 2
High   → 3
```

Use for:
- Low < Medium < High
- Small < Medium < Large

**Exam memory:** Ordered categories → **Ordinal**

---

### Target Encoding

Replace each category with a statistic based on the target.

Example:

```text
Merchant A → fraud rate 0.02
Merchant B → fraud rate 0.35
Merchant C → fraud rate 0.08
```

Useful for:
- High-cardinality categorical features

Risk:
- Data leakage
- Overfitting

**Exam memory:** Many unique categories → consider **Target Encoding**

---

# 4. Missing Value Handling

## Numerical Features
Common options:
- Mean
- Median

Use **median** when outliers are present.

## Categorical Features
Common options:
- Mode
- `"Unknown"`

---

# 5. Normalization and Scaling

## Min-Max Scaling

Rescales data to approximately:

```text
0 → 1
```

Good when:
- A fixed bounded range is useful
- Features have very different numeric ranges

Weakness:
- Sensitive to outliers

**Exam clue:** Need values between 0 and 1 → **Min-Max**

---

## Standardization

Transforms features so they have approximately:

```text
Mean = 0
Standard deviation = 1
```

Useful for:
- Logistic Regression
- SVM
- PCA
- Neural networks

**Exam clue:** Features on different scales / distance-based or coefficient-based models → **Standardization**

### Important tree-model note

Decision Trees, Random Forest, and XGBoost usually **do not require scaling**.

---

# 6. Outlier Handling

Outliers are unusually extreme values.

Example:

```text
Normal transaction amounts:
50, 60, 80, 100

Outlier:
1,000,000
```

Possible actions:
- Remove
- Cap / winsorize
- Log-transform
- Investigate whether the value is valid

Use box plots, percentiles, or statistical analysis to identify them.

---

# 7. Feature Transformations

Changes a feature so a model can learn patterns more effectively.

Examples:
- Log transformation
- Square root
- Polynomial features
- Interaction features

Example:

```text
Highly skewed income
→ log(income)
```

Useful when:
- Distribution is highly skewed
- Relationships are nonlinear
- Features interact with each other

---

# 8. Feature Analysis

Used to understand relationships between features and the target.

Common techniques:
- Correlation
- Feature importance
- Distribution analysis
- Mutual information
- PCA

Use it to identify:
- Redundant features
- Highly correlated features
- Important predictors
- Potential leakage

---

# 9. Data Visualization

Useful for understanding:
- Distribution
- Outliers
- Correlations
- Class imbalance
- Trends

Common charts:
- Histogram
- Box plot
- Scatter plot
- Bar chart
- Correlation heatmap

---

# 10. Class Imbalance

Example:

```text
Normal = 99%
Fraud  = 1%
```

Accuracy can become misleading.

Example:
- Model predicts "normal" for everything
- Accuracy = 99%
- Fraud detection = useless

Use:
- SMOTE
- Class weights
- Oversampling
- Undersampling

Evaluate with:
- Precision
- Recall
- F1
- PR-AUC

---

## SMOTE

**Synthetic Minority Over-sampling Technique**

Creates synthetic examples for the minority class.

```text
Before:
Normal = 990
Fraud  = 10

After SMOTE:
Fraud class receives synthetic samples
```

Important:
- Apply SMOTE only to the **training set**
- Do not apply it to validation/test data

**Exam clue:** Need more minority-class examples → **SMOTE**

---

## Class Weights

Give mistakes on the minority class a larger penalty.

Example:

```text
Normal mistake → weight 1
Fraud mistake  → weight 10
```

Does **not** create synthetic data.

### SMOTE vs Class Weights

| SMOTE | Class Weights |
|---|---|
| Creates synthetic samples | Changes loss importance |
| Changes training dataset | Does not add samples |
| Useful when minority data is too small | Useful when minority mistakes should matter more |

---

# 11. Model Evaluation Metrics

## Classification Metrics

### Accuracy

```text
Correct predictions / Total predictions
```

Use when:
- Classes are reasonably balanced

Avoid relying on it alone for heavily imbalanced problems.

---

### Precision

```text
TP / (TP + FP)
```

Question answered:

> Of everything predicted positive, how much was actually positive?

Use when **false positives are costly**.

Example:
- Spam filter blocking important email

**Exam memory:** False positives costly → **Precision**

---

### Recall

```text
TP / (TP + FN)
```

Question answered:

> Of all real positives, how many did the model find?

Use when **false negatives are costly**.

Example:
- Fraud
- Cancer detection

**Exam memory:** False negatives costly → **Recall**

---

### F1 Score

Balances Precision and Recall.

Use when:
- Dataset is imbalanced
- Both FP and FN matter

---

### ROC-AUC

Measures a classifier's ranking/separation ability across thresholds.

Use for:
- General classifier comparison

---

### PR-AUC

Precision-Recall Area Under Curve.

Especially useful for:
- Highly imbalanced classification
- Rare positive classes

Example:
- Fraud detection

---

## Regression Metrics

### MAE

Mean Absolute Error.

Use when:
- You want interpretable average error
- You want less sensitivity to large outliers than MSE

---

### MSE

Mean Squared Error.

Use when:
- Large errors should be punished strongly

---

### RMSE

Square root of MSE.

Useful because:
- Error is in the same unit as the target

---

### R²

Measures how much variance in the target is explained by the model.

Higher is generally better.

---

# 12. Overfitting vs Underfitting

## Overfitting

Model performs very well on training data but poorly on validation/test data.

```text
Training accuracy   = 99%
Validation accuracy = 75%
```

Causes:
- Model too complex
- Too little training data
- Too many irrelevant features

Fixes:
- L1 / L2 regularization
- More data
- Dropout
- Early stopping
- Reduce complexity
- Cross-validation
- Data augmentation

**Exam memory:**
```text
Train good + Validation bad
→ Overfitting
```

---

## Underfitting

Model performs poorly on both training and validation data.

```text
Training accuracy   = 65%
Validation accuracy = 64%
```

Fixes:
- More useful features
- More complex model
- Train longer
- Reduce regularization

**Exam memory:**
```text
Train bad + Validation bad
→ Underfitting
```

---

# 13. L1 vs L2 Regularization

Both help reduce overfitting.

## L1 — Lasso

Effect:
- Can shrink some weights exactly to zero
- Performs feature selection

Use when:
- Many irrelevant features
- Sparse model is useful

**Exam memory:** Need feature selection → **L1**

---

## L2 — Ridge

Effect:
- Shrinks large weights
- Usually keeps all features

Use when:
- Many useful or correlated features
- General overfitting reduction is needed

**Exam memory:** Shrink weights / many correlated useful features → **L2**

---

# 14. Bias and Explainability Metrics

## DPL — Difference in Positive Proportions in Labels

Measures bias in **original labels** before training.

Example:

```text
Positive labels:
Group A = 70%
Group B = 40%
```

**Memory:** DPL = **Labels / pre-training bias**

---

## DPPL — Difference in Positive Proportions in Predicted Labels

Measures bias in **model predictions** after training.

Example:

```text
Predicted positive:
Group A = 75%
Group B = 45%
```

**Memory:** DPPL = **Predicted labels / post-training bias**

---

## PDP — Partial Dependence Plot

Not a fairness metric.

Shows how changing one feature affects the model prediction on average.

Example:

```text
Age ↑
→ How does predicted churn change?
```

**Memory:** PDP = **Feature effect on prediction**

---

# 15. Distribution and Drift Metrics

## KS Test — Kolmogorov-Smirnov Test

Compares two numerical distributions.

Example:

```text
Training customer_age
vs
Production customer_age
```

Useful for:
- Data drift detection

---

## TVD — Total Variation Distance

Measures overall difference between two probability distributions.

Use when:
- You want a general measure of distribution difference

---

## KL Divergence

Measures how one probability distribution differs from another.

Important:

```text
KL(P || Q) ≠ KL(Q || P)
```

KL is **asymmetric**.

---

# 16. PCA

Principal Component Analysis.

Use for:
- Dimensionality reduction
- Highly correlated numerical features
- Reducing redundant information

Example:

```text
100 correlated numerical features
→ PCA
→ 20 principal components
```

---

# 17. Most Important Exam Comparisons

| Requirement | Best choice |
|---|---|
| Store ML datasets | Amazon S3 |
| General ETL | AWS Glue |
| Automatically detect schema | Glue Crawler |
| Metadata catalog | Glue Data Catalog |
| Visual ML preprocessing | SageMaker Data Wrangler |
| Custom preprocessing script | SageMaker Processing |
| Reusable ML features | SageMaker Feature Store |
| Real-time features | Feature Store Online |
| Historical features | Feature Store Offline |
| Query S3 with SQL | Athena |
| Data warehouse | Redshift |
| Spark/Hadoop | EMR |
| Move database | DMS |
| Move files | DataSync |
| Custom streaming consumers | Kinesis Data Streams |
| Stream directly to S3 | Firehose |
| Shared NFS | EFS |
| High-performance ML filesystem | FSx for Lustre |
| Label datasets | Ground Truth |
| Govern a data lake | Lake Formation |
| Encryption keys | KMS |
| Permissions | IAM |

---

# 18. Fast Exam Cheat Sheet

```text
ML-specific preprocessing + least overhead
→ SageMaker Data Wrangler

Custom preprocessing.py
→ SageMaker Processing

General ETL
→ AWS Glue

Automatically discover schema
→ Glue Crawler

Query S3 using SQL
→ Athena

On-prem database → AWS
→ DMS

On-prem files → AWS
→ DataSync

Real-time streaming with custom consumers
→ Kinesis Data Streams

Managed delivery of stream to S3
→ Firehose

Reusable features
→ SageMaker Feature Store

High-performance training filesystem
→ FSx for Lustre

No-order categories
→ One-Hot Encoding

Ordered categories
→ Ordinal Encoding

Many unique categories
→ Target Encoding

Need values between 0 and 1
→ Min-Max Scaling

Need mean 0 and std 1
→ Standardization

99% normal / 1% fraud
→ SMOTE / Class Weights

Synthetic minority examples
→ SMOTE

Increase importance of minority mistakes
→ Class Weights

False positives expensive
→ Precision

False negatives expensive
→ Recall

Imbalanced classification
→ F1 / PR-AUC

Feature selection
→ L1

Shrink large weights
→ L2

Train good + validation bad
→ Overfitting

Train bad + validation bad
→ Underfitting

Bias in original labels
→ DPL

Bias in model predictions
→ DPPL

Effect of feature on prediction
→ PDP

Compare numerical distributions / drift
→ KS test

Overall distribution difference
→ TVD / KL

Correlated numerical features / dimensionality reduction
→ PCA
```

---

# 19. Final Memory Map

```text
Sources
├── Database → DMS
├── Files    → DataSync
└── Stream   → Kinesis / Firehose
                 ↓
                S3
                 ↓
       ┌─────────┼──────────┐
       ↓         ↓          ↓
     Glue   Data Wrangler  Processing
  General ETL   Visual ML   Custom code
       └─────────┼──────────┘
                 ↓
          Feature Store
          /           \
      Online         Offline
        ↓               ↓
   Inference         Training
```

---

## Final Priority List for MLA-C01 Domain 2

Focus most on:

1. **Amazon S3**
2. **AWS Glue**
3. **SageMaker Data Wrangler**
4. **SageMaker Processing**
5. **SageMaker Feature Store**
6. **Athena**
7. **DMS vs DataSync**
8. **Kinesis Data Streams vs Firehose**
9. **EFS vs FSx for Lustre**
10. **Encoding / Scaling / Missing Values**
11. **SMOTE / Class Weights**
12. **Precision / Recall / F1 / PR-AUC**
13. **L1 / L2**
14. **Overfitting / Underfitting**
15. **DPL / DPPL / PDP**
16. **KS / TVD / KL / PCA**
