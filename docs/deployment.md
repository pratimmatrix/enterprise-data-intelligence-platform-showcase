# Enterprise Data Intelligence Platform — Deployment

## Deployment Overview

The Enterprise Data Intelligence Platform uses a two-repository architecture.

```text
                    PRIVATE SOURCE REPOSITORY
                              |
                              | Git Push
                              v
                    Streamlit Community Cloud
                              |
                              v
                    LIVE STREAMLIT APPLICATION
                              |
                              |
                              v
                    PUBLIC SHOWCASE REPOSITORY
```

The private repository contains the complete implementation.

The public repository contains the project presentation, architecture, documentation, screenshots, and live application reference.

---

# Public Showcase Repository

Repository:

https://github.com/pratimmatrix/enterprise-data-intelligence-platform-showcase

### Purpose

The showcase repository contains:

- Project overview
- Architecture
- Technical documentation
- Screenshots
- Feature descriptions
- Deployment documentation
- Live application reference
- Portfolio presentation

The showcase repository does not contain the private implementation source code.

---

# Private Source Repository

Repository:

`enterprise-data-intelligence-platform-source`

### Purpose

The private source repository contains the actual application implementation.

It includes:

- `app.py`
- `src/`
- Machine learning pipelines
- Feature engineering
- Data validation
- Schema registry
- Model governance
- Model artifacts
- Dataset processing
- Logging
- Runtime resources
- Testing
- `requirements.txt`

The private repository is the authoritative source for the deployed application.

---

# Live Application

Production application:

https://enterprise-data-intelligence-platform.streamlit.app/

The live application is deployed through Streamlit Community Cloud.

The deployment is connected to the private source repository.

Therefore, updates pushed to the configured deployment branch can trigger a new application deployment.

---

# Deployment Architecture

```text
Developer
    |
    | Local Development
    v
Private Source Repository
    |
    | git add
    | git commit
    | git push
    v
GitHub
    |
    | Connected Repository
    v
Streamlit Community Cloud
    |
    | Install requirements
    | Load application
    | Initialize models/resources
    v
Live Application
    |
    +-----------------------------+
    |                             |
    v                             v
Baseline Dataset            Candidate Dataset
bank-full.csv               Uploaded CSV
    |                             |
    +-------------+---------------+
                  |
                  v
        Ingestion & Pre-Flight
                  |
                  v
          Deep Data Profiling
                  |
                  v
        Anomaly Intelligence
                  |
                  v
        Schema Drift Registry
                  |
                  v
       Feature Engineering
                  |
                  v
       Model Arena / Retraining
                  |
                  v
      Governance Quality Gate
                  |
                  v
        Real-Time Inference
```

---

# Local Development

From the private source directory:

```powershell
conda activate base

cd "C:\Users\Prati\Documents\enterprise-data-intelligence-platform-source-local"

pip install -r requirements.txt

streamlit run app.py
```

The application normally starts at:

```text
http://localhost:8501
```

If port `8501` is already occupied, Streamlit may automatically use another available port such as:

```text
http://localhost:8502
```

---

# Python Environment

The current deployment uses:

```text
Python 3.11
```

The local environment should use a compatible Python version.

Recommended:

```text
Python 3.11
```

---

# Requirements

The authoritative dependency list is maintained in:

```text
requirements.txt
```

Current project dependencies include:

```text
streamlit
pandas
numpy
scikit-learn
joblib
matplotlib
xgboost
lightgbm
catboost
```

The source repository's `requirements.txt` should always be treated as the authoritative dependency definition.

Do not maintain a second independent dependency list in the showcase repository.

---

# Streamlit Community Cloud

The Streamlit deployment should point to:

```text
Repository:
enterprise-data-intelligence-platform-source

Branch:
main

Main file:
app.py

Python:
3.11
```

The deployment must have permission to access the private GitHub repository.

---

# Private Repository Access

Because the source repository is private, Streamlit Community Cloud requires GitHub access to that repository.

If deployment fails with a repository-access error:

1. Open Streamlit Community Cloud.
2. Open the application settings.
3. Check the connected GitHub account.
4. Verify repository permissions.
5. Grant access to the private repository if necessary.
6. Reconnect GitHub if required.
7. Re-deploy the application.

---

# Secrets

Secrets must never be committed to Git.

If the application requires runtime secrets, configure them through the Streamlit Cloud Secrets interface.

Example:

```toml
[database]
username = "..."
password = "..."

[api]
key = "..."
```

Only configure secrets that are actually required by the application.

Never place passwords, API keys, tokens, or private credentials inside source files, documentation, or Git commits.

---

# Git Deployment Workflow

After modifying the private source code:

```powershell
cd "C:\Users\Prati\Documents\enterprise-data-intelligence-platform-source-local"
```

Check the working tree:

```powershell
git status
```

Stage the required files:

```powershell
git add .
```

Review:

```powershell
git status
```

Commit:

```powershell
git commit -m "Update adaptive data intelligence platform"
```

Push:

```powershell
git push origin main
```

Verify:

```powershell
git status
```

Expected result:

```text
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

---

# Deployment Update Lifecycle

```text
Modify Application
       |
       v
Test Locally
       |
       v
git status
       |
       v
git add
       |
       v
git commit
       |
       v
git push origin main
       |
       v
GitHub
       |
       v
Streamlit Cloud Detects Update
       |
       v
Application Rebuild
       |
       v
Application Restart
       |
       v
Updated Live Application
```

---

# Important Deployment Rule

The showcase repository does not need to be deployed as the application source.

The live Streamlit application uses the private source repository.

Therefore:

```text
Private Source Repository
        ↓
Streamlit Deployment
```

is the deployment relationship.

The showcase repository is:

```text
Public Documentation
        ↓
Project Presentation
        ↓
Live Demo Link
```

---

# Adaptive Dataset Deployment Behavior

The deployed application supports two dataset states.

## 1. Default Baseline

The application starts with the approved baseline dataset:

```text
bank-full.csv
```

The baseline represents the default production dataset.

---

## 2. Candidate Dataset

The user can upload an updated CSV.

For example:

```text
bank-full-updated.csv
```

The candidate dataset may contain:

- Additional rows
- Additional columns
- Removed columns
- Modified columns
- New categorical values

After validation, the candidate dataset becomes the active dataset for analysis.

---

# Active Dataset State

Once a valid candidate CSV is uploaded:

```text
Candidate CSV
      |
      v
Validation
      |
      v
Active Dataset
      |
      +----> Profiling
      |
      +----> Anomaly Analysis
      |
      +----> Schema Analysis
      |
      +----> Feature Engineering
      |
      +----> Model Evaluation
      |
      +----> Governance
      |
      +----> Inference
```

The platform should not silently continue displaying reports from the previous baseline dataset.

---

# Reset to Default

The application provides a reset mechanism.

When the user selects:

```text
Reset to Default Prediction
```

the system returns to:

```text
bank-full.csv
```

The application then recalculates the relevant reports from the default baseline.

This prevents an uploaded candidate dataset from permanently replacing the production baseline.

---

# Download Main Dataset

The UI provides an option to download the original baseline dataset.

```text
Download Main Dataset
        |
        v
bank-full.csv
```

This allows users to:

1. Obtain the approved baseline.
2. Modify the dataset.
3. Add rows.
4. Add columns.
5. Test schema drift.
6. Upload the modified dataset.
7. Observe the adaptive pipeline.

---

# Candidate Dataset Validation

Every uploaded CSV passes through validation before model processing.

```text
Uploaded CSV
     |
     v
File Validation
     |
     v
Schema Validation
     |
     v
Target Validation
     |
     v
Data Quality Validation
     |
     v
Semantic Compatibility
     |
     v
Decision
```

Possible outcomes:

```text
VALID
WARNING
INSUFFICIENT DATA
INVALID DATASET
UNRELATED DATASET
REJECTED
```

---

# Wrong Dataset Protection

The application must not generate a prediction for an unrelated CSV.

Examples of invalid input include:

```text
Completely unrelated columns
Missing target
Incompatible target
Insufficient required fields
Invalid data types
Corrupted CSV
Unsupported structure
```

Expected behavior:

```text
Prediction:
BLOCKED

Reason:
Insufficient / incompatible dataset

Action:
Upload a compatible bank-marketing dataset
```

This prevents the system from producing meaningless predictions.

---

# Schema Drift

The application detects changes between the approved baseline schema and the candidate schema.

Supported drift categories include:

```text
Added Columns
Removed Columns
Modified Columns
Preserved Columns
Unseen Categories
Type Changes
```

Example:

```text
BASELINE

age
job
balance
campaign
y


CANDIDATE

age
job
balance
campaign
credit_score
customer_segment
y
```

Result:

```text
Added:
credit_score
customer_segment

Removed:
None

Modified:
None
```

---

# Adaptive Reporting

When a valid candidate dataset becomes active, the following sections operate against that candidate:

```text
1. Ingestion & Pre-Flight
2. Deep Data Profiling
3. Anomaly Intelligence
4. Schema Drift & Registry
5. Feature Engineering Studio
6. Adaptive Retraining & Governance
7. Real-Time Inference & Explainability
```

The previous baseline report should not remain misleadingly active.

---

# Model Arena

The adaptive retraining layer evaluates multiple machine learning algorithms.

Current model candidates include:

```text
Logistic Regression
Random Forest
Gradient Boosting
HistGradient Boosting
XGBoost
LightGBM
CatBoost
```

Each candidate is evaluated using:

```text
Accuracy
Precision
Recall
F1
ROC-AUC
Balanced Accuracy
```

---

# Model Selection

The platform uses:

```text
F1
```

as the primary model-selection metric because the classification task is affected by class imbalance.

Supporting metrics include:

```text
ROC-AUC
Balanced Accuracy
Accuracy
Precision
Recall
```

The strongest validated candidate can become the champion model only after governance checks.

---

# Governance

The governance layer protects the currently approved production model.

```text
Candidate Model
       |
       v
Metric Evaluation
       |
       v
Quality Gate
       |
       +-------- FAIL --------> Reject
       |
       v
     PASS
       |
       v
Champion Candidate
       |
       v
Production Model
```

Typical governance controls include:

- Minimum F1
- Minimum ROC-AUC
- Maximum allowed performance degradation
- Candidate-versus-production comparison
- Promotion decision
- Rejection decision
- Model metadata

---

# Real-Time Inference

After the dataset and model pass the required validation gates, the application provides customer-level inference.

The workflow is:

```text
Customer Input
      |
      v
Input Validation
      |
      v
Feature Engineering
      |
      v
Production Pipeline
      |
      v
Prediction
      |
      v
Probability
      |
      v
Risk
      |
      v
Business Priority
```

---

# Customer Profile

The inference interface provides a customer profile containing relevant information such as:

```text
Age
Job
Education
Marital Status
Balance
Housing
Loan
Default
Contact
Campaign
Previous Outcome
```

The profile converts model input into a readable business representation.

---

# Model Information

The prediction interface also exposes information about the selected model.

Example:

```text
Model
HistGradient Boosting

Accuracy
...

F1
...

ROC-AUC
...
```

This makes the prediction traceable to the model that generated it.

---

# Prediction Safety

The application should never fabricate a prediction when the input dataset is invalid.

```text
Valid Dataset
      |
      v
Prediction Allowed
```

versus:

```text
Invalid Dataset
      |
      v
Prediction Blocked
      |
      v
Explain Failure
```

---

# Logging

The platform records important application events.

Logging areas include:

```text
Ingestion
Validation
Profiling
Anomaly Detection
Schema Drift
Feature Engineering
Training
Model Evaluation
Governance
Inference
Exceptions
```

Logs are intended to support:

- Debugging
- Reproducibility
- Operational diagnosis
- Model lifecycle auditing
- Deployment troubleshooting

---

# Generated Runtime Files

Generated runtime artifacts should be reviewed before committing.

Examples include:

```text
catboost_info/
logs/
temporary files
cache files
runtime output
```

These should normally be excluded from Git unless there is a deliberate reason to version them.

The `.gitignore` file controls which generated files are excluded.

---

# Model Artifacts

Model artifacts are treated separately from source code.

Examples include:

```text
models/artifacts/
models/registry/
```

Before deployment, verify that every required runtime artifact is either:

1. committed intentionally, or
2. generated safely during deployment/startup.

Do not accidentally remove required production artifacts while cleaning the repository.

---

# Deployment Troubleshooting

## 1. Application does not start

Check:

```text
app.py
requirements.txt
Python version
Streamlit logs
```

Run locally first:

```powershell
streamlit run app.py
```

---

## 2. ModuleNotFoundError

Example:

```text
ModuleNotFoundError: No module named 'xgboost'
```

Add the required package to:

```text
requirements.txt
```

Then:

```powershell
git add requirements.txt
git commit -m "Fix deployment dependencies"
git push origin main
```

---

## 3. Model File Not Found

Check the expected model path.

Verify:

```text
models/
models/artifacts/
models/registry/
```

Do not assume the current working directory is the same in Streamlit Cloud and local Windows execution.

Use project-relative paths where appropriate.

---

## 4. Private Repository Access Error

Verify:

```text
GitHub connection
Repository permissions
Correct repository
Correct branch
```

Reconnect GitHub to Streamlit Community Cloud if necessary.

---

## 5. Application Uses Old Version

Check:

```text
GitHub latest commit
Streamlit deployment branch
Streamlit build logs
Streamlit runtime logs
```

Then trigger a redeploy if necessary.

---

# Git Safety

Before pushing:

```powershell
git status
```

Review all modified files.

Avoid accidentally committing:

```text
.env
API keys
Passwords
Tokens
Private credentials
Large temporary files
Unnecessary runtime logs
```

---

# Production Security

The private source repository should remain private.

The live application may be publicly accessible depending on Streamlit sharing configuration.

This means:

```text
Private Source
+
Public Application
```

is possible.

Never assume that making the source repository private automatically makes the deployed application private.

---

# Production Hardening

A future enterprise production deployment could additionally implement:

- CI/CD
- Automated unit tests
- Integration tests
- Docker
- Authentication
- Authorization
- RBAC
- Centralized logging
- Monitoring
- Database infrastructure
- Dedicated model registry
- Experiment tracking
- Data drift monitoring
- Model drift monitoring
- Secret rotation
- Audit logging
- Security scanning
- Dependency scanning
- Automated rollback

---

# Deployment Lifecycle

```text
                +-------------------+
                | Developer Changes|
                +---------+---------+
                          |
                          v
                +-------------------+
                | Local Testing     |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Git Status        |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Git Add           |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Git Commit        |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Git Push          |
                +---------+---------+
                          |
                          v
                +-------------------+
                | GitHub            |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Streamlit Cloud   |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Build & Deploy    |
                +---------+---------+
                          |
                          v
                +-------------------+
                | Live Application  |
                +-------------------+
```

---

# Release Checklist

Before every production update:

```text
[ ] Application runs locally
[ ] app.py starts successfully
[ ] requirements.txt is current
[ ] Required models exist
[ ] Required data exists
[ ] Schema registry is valid
[ ] Candidate CSV workflow tested
[ ] Reset-to-default tested
[ ] Wrong CSV rejection tested
[ ] Prediction blocking tested
[ ] Model arena tested
[ ] Governance tested
[ ] Customer profile tested
[ ] Logging tested
[ ] No secrets committed
[ ] .gitignore reviewed
[ ] git status reviewed
[ ] Commit created
[ ] Push completed
[ ] Streamlit deployment updated
[ ] Live URL tested
[ ] Showcase README updated if functionality changed
```

---

# Final Deployment Model

```text
                    +--------------------------+
                    |  PRIVATE SOURCE REPO     |
                    |                          |
                    |  app.py                  |
                    |  src/                    |
                    |  models/                 |
                    |  data/                   |
                    |  requirements.txt        |
                    +------------+-------------+
                                 |
                              Git Push
                                 |
                                 v
                    +--------------------------+
                    | STREAMLIT CLOUD          |
                    |                          |
                    | Build                    |
                    | Dependencies             |
                    | Runtime                  |
                    +------------+-------------+
                                 |
                                 v
                    +--------------------------+
                    | LIVE APPLICATION         |
                    |                          |
                    | Ingestion                |
                    | Profiling                |
                    | Anomalies                |
                    | Schema Drift             |
                    | Features                 |
                    | Retraining               |
                    | Governance               |
                    | Inference                |
                    +------------+-------------+
                                 |
                                 v
                    +--------------------------+
                    | PUBLIC SHOWCASE          |
                    |                          |
                    | README                   |
                    | Architecture             |
                    | Screenshots              |
                    | Documentation            |
                    | Live Demo Link           |
                    +--------------------------+
```

The **private source repository is the implementation authority**, Streamlit Community Cloud is the deployment layer, the live URL is the production interface, and the showcase repository is the public presentation layer.
