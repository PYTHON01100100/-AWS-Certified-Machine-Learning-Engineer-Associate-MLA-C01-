# MLA-C01 — Domain 3: Machine Learning Model Development

## 1. Core ML Development Flow

```text
Prepare data
   ↓
Choose algorithm/model
   ↓
Train
   ↓
Tune hyperparameters
   ↓
Evaluate
   ↓
Register/version
   ↓
Deploy
   ↓
Monitor
```

---

# 2. Amazon SageMaker AI — Core Services

## SageMaker Studio
Integrated web-based ML development environment.

**Use for:**
- JupyterLab and code development
- Data preparation
- Training
- Experiment tracking
- Model analysis
- Deployment

**Exam clue:** integrated ML development environment → **SageMaker Studio**

---

## SageMaker Training Jobs
Managed training infrastructure.

You provide:
- Training data
- Algorithm/container
- Hyperparameters
- Instance type/count

SageMaker:
1. Provisions training instances
2. Runs the training job
3. Saves model artifacts to S3
4. Terminates training instances

**Exam clue:** managed model training → **SageMaker Training Job**

---

## SageMaker Managed Warm Pools
Keeps training infrastructure warm between consecutive training jobs.

**Choose when:** you need to minimize **startup/provisioning time** for repeated jobs.

```text
Consecutive training jobs + faster startup
→ Managed Warm Pools
```

---

## SageMaker Managed Spot Training
Uses spare AWS capacity to reduce training cost.

**Choose when:**
- Training can tolerate interruption
- Cost reduction is the priority

```text
Reduce training COST
→ Managed Spot Training
```

---

## SageMaker Automatic Model Tuning
Automates hyperparameter search.

You define:
- Search space
- Objective metric
- Max jobs/resources
- Tuning strategy

Examples:
- Maximize F1 / accuracy / AUC
- Minimize RMSE / loss

**Exam clue:** automatically optimize hyperparameters → **Automatic Model Tuning**

---

# 3. SageMaker Built-in Algorithms

| Algorithm | Best use | Real-world example | Exam clue |
|---|---|---|---|
| **XGBoost** | Tabular classification/regression | Customer churn, fraud, credit risk | Tabular + strong performance |
| **Linear Learner** | Linear classification/regression | Loan default, simple price prediction | Linear/simple/interpretable |
| **k-NN** | Similarity-based classification/regression | Classify customer based on nearest customers | Nearest / closest / similarity |
| **Factorization Machines** | Sparse data / recommendations | User-product recommendation, click prediction | Sparse matrix / recommendations |
| **K-Means** | Clustering | Customer segmentation | Groups / clusters / unlabeled |
| **PCA** | Dimensionality reduction | Reduce 500 sensor features to 30 components | Reduce dimensions |
| **Random Cut Forest** | Anomaly detection | Detect abnormal sensor values | Anomaly / outlier |
| **LDA** | Topic modeling | Discover topics in support tickets | Topics in documents |
| **BlazingText** | Word embeddings / text classification | Word2Vec, review classification | Embeddings / Word2Vec |
| **Sequence-to-Sequence** | Sequence mapping | English → French translation | Translation |
| **DeepAR** | Time-series forecasting | Forecast demand/sales | Forecast / time series |
| **IP Insights** | Entity-IP anomaly detection | Suspicious login IP | Unusual IP / compromise |

### Fast memory

```text
XGBoost        = TABULAR
Linear Learner = LINEAR
k-NN           = NEAREST
FM             = SPARSE / RECOMMENDATION
K-Means        = CLUSTERS
PCA            = REDUCE DIMENSIONS
RCF            = ANOMALIES
LDA            = TOPICS
BlazingText    = EMBEDDINGS / TEXT
Seq2Seq        = TRANSLATION
DeepAR         = FORECAST
IP Insights    = SUSPICIOUS IP
```

---

# 4. Built-in vs Script Mode vs BYOC vs BYOM

## Built-in Algorithm
AWS provides the algorithm and container.

**Choose when:** the standard SageMaker algorithm already matches your use case.

```text
Least custom code
→ Built-in algorithm
```

---

## Script Mode
You provide custom Python training code; AWS provides the framework container.

Typical frameworks:
- PyTorch
- TensorFlow
- scikit-learn

**Choose when:**
- Custom training logic
- Custom preprocessing
- Fine-tuning
- Custom architecture

```text
My code + AWS container
→ Script Mode
```

### Important parameters

```python
entry_point="train.py"
```

= main custom training script

```python
estimator.fit(...)
```

= start SageMaker training job

```python
model.fit(...)
```

= train the TensorFlow/Keras model inside the script

### Important SageMaker environment variables
- `SM_MODEL_DIR` → save final model
- `SM_CHANNEL_TRAINING` → training-data channel path

---

## BYOC — Bring Your Own Container
You provide:
- Docker image
- OS/runtime
- Framework
- Dependencies
- Training/inference code

Store image in **Amazon ECR**.

**Choose when:**
- Unsupported framework
- Custom system libraries
- Special CUDA/runtime
- Full environment control needed

```text
My code + my container
→ BYOC
```

---

## BYOM — Bring Your Own Model
Model was trained outside SageMaker and you want to host it in SageMaker.

Choose:
- **Pre-built container** if framework is supported
- **BYOC** if runtime is custom

```text
Existing trained model
→ BYOM
```

---

# 5. Pre-built Containers / Deep Learning Containers

SageMaker provides ML-focused pre-built containers for frameworks such as:
- TensorFlow
- PyTorch
- scikit-learn
- Hugging Face environments

**Exam distinction:**

```text
SageMaker = ML-focused pre-built containers
ECS/EKS   = container orchestration
Fargate   = serverless container compute
```

---

# 6. Training Data Input Modes

## File Mode
Downloads the whole S3 dataset before training.

**Choose when:**
- Small/moderate dataset
- Need random/local file access

```text
File = DOWNLOAD ALL
```

## Pipe Mode
Streams data directly from S3.

**Choose when:**
- Very large dataset
- Sequential reading
- Want faster startup / less disk use

```text
Pipe = STREAM
```

## Fast File Mode
Streams from S3 while presenting file-like access.

**Choose when:**
- Large dataset
- Code expects normal file paths

```text
Fast File = STREAM + FILE INTERFACE
```

---

# 7. Distributed Training

## Data Parallelism
Each GPU/instance has a full copy of the model but processes different batches.

**Choose when:**
- Model fits on one GPU
- Dataset is huge
- Need faster training

```text
MODEL fits + DATA huge
→ Data Parallelism
```

## Model Parallelism
Split the model itself across multiple GPUs/instances.

**Choose when:**
- Model cannot fit in one GPU's memory
- Very large transformer/LLM

```text
MODEL too large
→ Model Parallelism
```

---

# 8. EC2 Instance Families for ML

| Family | Best use |
|---|---|
| **T** | Development / notebooks / bursty workloads |
| **M** | Balanced general-purpose ML |
| **R** | Memory-intensive workloads |
| **C** | CPU-intensive workloads |
| **G** | Cost-effective GPU workloads / inference |
| **P** | High-performance GPU training |
| **Trainium / Trn** | AWS ML training accelerator |

### Fast memory

```text
T = light/dev
M = balanced
R = RAM
C = CPU
G = economical GPU
P = powerful GPU
Trn = training accelerator
```

---

# 9. AWS Trainium and Inferentia

## Trainium
AWS-designed accelerator for large-scale ML training.

**Choose when:**
- LLM training
- Transformer training
- High-performance, cost-efficient training

```text
Trainium = TRAIN
```

## Inferentia
AWS-designed accelerator for ML inference.

```text
Inferentia = INFER
```

---

# 10. Optimization Techniques

## Batch Gradient Descent
Updates weights after processing the entire dataset.

**Use when:**
- Dataset is small
- Want stable/smooth updates

## Stochastic Gradient Descent (SGD)
Updates after every single example.

**Use when:**
- Very frequent updates
- Online/incremental learning

## Mini-batch Gradient Descent
Updates after a small batch.

**Use when:**
- Deep learning
- Large datasets
- GPU training

```text
Batch GD      = ALL → UPDATE
SGD           = ONE → UPDATE
Mini-batch GD = GROUP → UPDATE
```

---

# 11. Core Hyperparameters

## Learning Rate
Controls update step size.

```text
Too low  → slow convergence
Good     → stable convergence
Too high → overshooting / instability / divergence
```

## Batch Size
Examples processed before a weight update.

- Small batch → less memory, noisier gradients
- Large batch → better hardware utilization, more memory

## Epochs
Number of complete passes through training data.

- Too few → underfitting
- Too many → overfitting risk
- Use **early stopping** when validation stops improving

---

# 12. Neural Network Hyperparameters

## Activation Functions

| Activation | Best use |
|---|---|
| **ReLU** | Hidden layers, helps reduce vanishing-gradient problems |
| **Sigmoid** | Binary classification output |
| **Softmax** | Multiclass classification output |
| **Linear** | Regression output |
| **Tanh** | Zero-centered activation, some specialized architectures |

### Fast memory

```text
Hidden deep layers → ReLU
Binary output      → Sigmoid
Multiclass output  → Softmax
Regression output  → Linear
```

---

## Regularization

### Dropout
Randomly disables neurons during training.

**Use when:** neural network is overfitting.

### L1
Encourages sparse weights and can drive some weights to zero.

**Choose when:** feature selection / sparsity is useful.

### L2
Shrinks large weights without usually forcing them to zero.

**Choose when:** general regularization / overfitting reduction.

```text
L1 = zero some weights
L2 = shrink weights
```

---

# 13. Decision Tree Hyperparameters

## `max_depth`
Maximum tree depth.

```text
Decrease max_depth → simpler tree → less overfitting
Increase max_depth → more complex → more capacity
```

## `min_samples_split`
Minimum samples required before splitting a node.

```text
Increase min_samples_split → simpler tree
Decrease min_samples_split → more complex tree
```

## Split criteria
- Gini impurity
- Entropy / information gain

**Exam clue:** choosing best split → Gini/Entropy

---

# 14. Bias-Variance Tradeoff

## High Bias = Underfitting
Model too simple.

```text
Training performance = poor
Validation/test       = poor
```

Possible fixes:
- Increase complexity
- Add useful features
- Train longer
- Reduce excessive regularization

## High Variance = Overfitting
Model too complex / memorizes training data.

```text
Training performance = excellent
Validation/test       = poor
```

Possible fixes:
- More data
- Regularization
- Dropout
- Early stopping
- Reduce complexity

### Fast memory

```text
Train BAD + Test BAD
→ High Bias / Underfitting

Train GREAT + Test BAD
→ High Variance / Overfitting
```

---

# 15. Loss Functions

## Classification
Use:
- Binary cross-entropy / log-loss / log-likelihood for binary classification
- Categorical cross-entropy for multiclass classification

## Regression
Use:
- MSE
- RMSE
- MAE

**Important distinction:**

```text
Loss function = how wrong the model is
Optimizer     = how weights are updated
```

Gradient descent / SGD / Adam are **optimizers**, not loss functions.

---

# 16. Classification Metrics

## Accuracy
Overall percentage correct.

**Choose when:** classes are reasonably balanced.

## Precision
Of predicted positives, how many were correct?

**Use when:** false positives are costly.

```text
False Positive costly
→ Precision
```

## Recall
Of actual positives, how many were found?

**Use when:** false negatives are costly.

```text
False Negative costly
→ Recall
```

## F1
Balances precision and recall.

**Use when:**
- Classes are imbalanced
- Both FP and FN matter

## AUC-ROC
Measures class separation across many thresholds.

**Use when:** compare classifiers independent of one chosen threshold.

### Fast memory

```text
Accuracy  = overall correctness
Precision = when I say YES, am I right?
Recall    = did I find all YES cases?
F1        = precision + recall balance
AUC-ROC   = separation across thresholds
```

---

# 17. Regression Metrics

| Metric | Meaning | Better |
|---|---|---|
| **MSE** | Mean squared prediction error | Lower |
| **RMSE** | Error in original target units | Lower |
| **MAE** | Mean absolute error, more robust to outliers | Lower |
| **R²** | Variance explained | Higher |
| **Adjusted R²** | R² with complexity penalty | Higher |

### Exam clues

```text
Penalize large errors strongly → MSE / RMSE
Need original units           → RMSE
Outliers should matter less   → MAE
Variance explained            → R²
Account for extra features    → Adjusted R²
```

---

# 18. Train / Validation / Test Split

Common example:

```text
80% Training
10% Validation
10% Test
```

- **Training** → learn model parameters
- **Validation** → tune hyperparameters / early stopping / compare models
- **Test** → final unbiased evaluation

### Fast memory

```text
TRAIN      = LEARN
VALIDATION = TUNE
TEST       = FINAL EXAM
```

### Other split strategies
- **Stratified split** → imbalanced classes
- **Time-based split** → time series / forecasting
- **Cross-validation** → small datasets

---

# 19. Hyperparameter Tuning Techniques

## Manual
Human chooses values.

**Best for:** small projects / strong domain knowledge.

## Grid Search
Tests every predefined combination.

**Best for:** small search space.

```text
Grid = EVERYTHING
```

## Random Search
Randomly samples combinations.

**Best for:** large search space / limited compute.

```text
Random = RANDOM COMBINATIONS
```

Why it often beats grid:
- Not all hyperparameters matter equally
- Explores important dimensions more efficiently

## Bayesian Optimization
Uses previous trial results to choose better future trials.

**Best for:** expensive training jobs / limited budget.

```text
Bayesian = LEARN FROM PAST
```

## Hyperband
Starts many configurations and stops weak ones early.

**Best for:** iterative training where bad configs can be detected quickly.

```text
Hyperband = KILL BAD TRIALS EARLY
```

---

# 20. Model Size Management

## Pruning
Removes low-impact weights/connections.

```text
Pruning = fewer parameters
```

## Quantization
Reduces numeric precision.

```text
FP32 → FP16 → INT8 → INT4
```

Benefits:
- Lower RAM/VRAM
- Faster inference
- Smaller model size

```text
Quantization = fewer bits per parameter
```

## Knowledge Distillation
Trains a smaller student model to imitate a larger teacher.

```text
Teacher → Student
```

### Fast memory

```text
Pruning      = REMOVE weights
Quantization = LOWER precision
Distillation = TEACHER → STUDENT
```

---

# 21. Fine-tuning

Adapts a pretrained model to a task/domain.

Benefits:
- Less training time
- Less data
- Lower compute than training from scratch

## Domain Adaptation
General model → finance/healthcare/legal/etc.

## Instruction Tuning
Teach model to follow task instructions and output formats.

### Fine-tuning vs RAG

```text
Need new behavior/style/task specialization
→ Fine-tuning

Need external/current knowledge
→ RAG
```

---

# 22. Catastrophic Forgetting

Occurs when fine-tuning causes the model to lose previously learned capabilities.

```text
New task good + old task bad
→ Catastrophic forgetting
```

### Prevention
- Rehearsal: mix old + new data
- Elastic Weight Consolidation (EWC): protect important old weights
- Modular/adaptor architectures

---

# 23. Model Registry and Versioning

## SageMaker Model Registry
Central model catalog for production models.

Tracks:
- Model versions
- Metrics
- Metadata
- Approval status
- Lineage

## Model Package Group
Organizes related model versions.

```text
FraudModel
├── v1
├── v2
├── v3
└── v4
```

## Approval statuses
- PendingManualApproval
- Approved
- Rejected

**Exam clue:** manual approval before production → **Model Registry approval workflow**

---

# 24. SageMaker Experiments vs Registry vs Monitor

```text
Experiments
= Which TRAINING RUN was best?

Model Registry
= Which MODEL VERSION is approved?

Model Monitor
= Is the DEPLOYED MODEL still healthy?
```

---

# 25. SageMaker Clarify

Use for:
- Bias detection
- Fairness
- Bias drift
- Explainability
- Feature attribution / SHAP

```text
Bias / fairness / SHAP
→ SageMaker Clarify
```

---

# 26. SageMaker Model Monitor

Use for deployed-model monitoring.

Common monitoring types:
- Data Quality
- Model Quality
- Bias Drift
- Feature Attribution Drift

```text
Production drift / quality
→ Model Monitor
```

---

# 27. SageMaker Pipelines

Managed ML workflow orchestration.

Example:

```text
Preprocess
   ↓
Train
   ↓
Evaluate
   ↓
Register
   ↓
Approve
   ↓
Deploy
```

**Exam clue:** automated/reproducible ML workflow → **SageMaker Pipelines**

---

# 28. SageMaker Feature Store

Central repository for reusable ML features.

Correct order:

```text
1. Create Feature Group
2. Ingest records
3. Access features for training/inference
```

## Online Store
Low-latency feature retrieval for real-time inference.

## Offline Store
Historical feature storage for training/analytics.

### Fast memory

```text
Online  = inference
Offline = training/history
```

---

# 29. Data Preparation Services

## Amazon S3
Primary object storage for:
- Training data
- Validation data
- Model artifacts
- Batch I/O

## AWS Glue Crawler
Discovers:
- Schema
- Columns
- Data types

```text
Unknown S3 schema
→ Glue Crawler
```

## AWS Glue DataBrew
Visual/no-code data cleaning and feature engineering.

Use for:
- Missing values
- Transformations
- Rename columns
- Feature creation

```text
Clean / transform visually
→ DataBrew
```

## Amazon Athena
SQL queries directly against S3.

```text
SQL on S3
→ Athena
```

## AWS Glue Data Quality
Checks:
- Nulls
- Invalid values
- Schema consistency
- Duplicates

```text
Data quality
→ Glue Data Quality

Bias/fairness
→ SageMaker Clarify
```

---

# 30. Storage for Training

| Service | Best use |
|---|---|
| **S3** | General ML data/object storage |
| **EFS** | Shared file system |
| **FSx for Lustre** | High-performance ML/HPC file access |

---

# 31. Amazon ECR

Stores Docker images.

```text
Custom container
→ ECR
→ SageMaker
```

Do not confuse:

```text
ECR
= container versions

Model Registry
= ML model versions
```

---

# 32. SageMaker Batch Transform

Runs offline/batch predictions using a trained model.

```text
Trained model + large dataset
→ Batch Transform
→ Predictions in S3
```

Not for:
- Data cleaning
- Feature engineering

---

# 33. AWS Marketplace for ML

Third-party commercial models/algorithms.

**Choose when:**
- Vendor already provides the needed model
- You want to subscribe instead of build

```text
Marketplace = BUY / SUBSCRIBE
```

---

# 34. SageMaker JumpStart

Pretrained models and solution templates.

**Choose when:**
- Want a pretrained starting point
- Want quick deployment/fine-tuning

```text
JumpStart = START FROM PRETRAINED
```

---

# 35. AI Services Relevant to Model Development

## Amazon Comprehend
NLP:
- Sentiment
- Entities
- Key phrases
- Text classification

```text
General text NLP
→ Comprehend
```

## Amazon Lex V2
Conversational AI:
- Chatbots
- Intents
- Slots
- Multi-turn conversations

```text
Chatbot
→ Lex
```

## Amazon Connect Contact Lens
Contact-center analytics:
- Call sentiment
- Categorization
- Agent/customer interaction insights

```text
Call center analytics
→ Contact Lens
```

## Amazon Textract
Extracts text/tables/forms from documents.

```text
Forms / PDFs / OCR
→ Textract
```

## Amazon Rekognition
Image/video analysis.

```text
Vision
→ Rekognition
```

## Amazon Transcribe
Speech → text.

## Amazon Polly
Text → speech.

## Amazon Translate
Text translation.

## Amazon Personalize
Recommendations.

## Amazon Kendra
Enterprise intelligent search.

---

# 36. Final Exam Decision Map

```text
Need tabular classification/regression?
→ XGBoost

Need linear/simple/interpretable?
→ Linear Learner

Need nearest/similar?
→ k-NN

Need recommendations on sparse data?
→ Factorization Machines

Need clusters?
→ K-Means

Need dimensionality reduction?
→ PCA

Need anomaly detection?
→ Random Cut Forest

Need document topics?
→ LDA

Need embeddings/text classification?
→ BlazingText

Need translation?
→ Sequence-to-Sequence

Need time-series forecast?
→ DeepAR

Need suspicious IP detection?
→ IP Insights
```

---

# 37. Highest-Value MLA-C01 Rules

```text
Train bad + test bad
→ UNDERFITTING / HIGH BIAS

Train great + test bad
→ OVERFITTING / HIGH VARIANCE
```

```text
False Positive costly
→ PRECISION

False Negative costly
→ RECALL

Need both
→ F1
```

```text
Large errors matter
→ RMSE

Outliers should matter less
→ MAE

Variance explained
→ R²
```

```text
Learning rate too high
→ overshoot / unstable

Learning rate too low
→ slow convergence
```

```text
Grid
→ exhaustive

Random
→ efficient random sampling

Bayesian
→ learns from previous trials

Hyperband
→ stops bad trials early
```

```text
Pruning
→ remove weights

Quantization
→ reduce precision

Distillation
→ teacher → student
```

```text
File Mode
→ download

Pipe Mode
→ stream

Fast File
→ stream + file interface
```

```text
Data Parallelism
→ model fits, dataset huge

Model Parallelism
→ model too large for one GPU
```

```text
Feature Store
→ features

Experiments
→ training runs

Model Registry
→ model versions/approval

Clarify
→ bias/explainability

Model Monitor
→ production monitoring

Pipelines
→ orchestration
```

---

# 38. One-Page Memory Map

```text
DATA
S3 / Glue Crawler / DataBrew / Athena
        ↓
FEATURES
SageMaker Feature Store
        ↓
TRAIN
SageMaker Training Jobs
Built-in / Script Mode / BYOC
        ↓
TUNE
Automatic Model Tuning
Grid / Random / Bayesian / Hyperband
        ↓
EVALUATE
Accuracy / Precision / Recall / F1 / AUC
MSE / RMSE / MAE / R²
        ↓
REGISTER
SageMaker Model Registry
        ↓
APPROVE
Pending → Approved / Rejected
        ↓
DEPLOY
Endpoint / Batch Transform
        ↓
MONITOR
Model Monitor / Clarify
```
