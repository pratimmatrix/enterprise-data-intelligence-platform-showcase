
# Enterprise Data Intelligence Platform — Deployment

## Deployment Overview

The platform uses a two-repository deployment strategy.

```text
Private Source Repository
        |
        | Streamlit Community Cloud
        v
Live Streamlit Application
        |
        v
Public Showcase Repository
```

The private repository contains the implementation while the public repository contains documentation and project presentation.

## Public Showcase Repository

https://github.com/pratimmatrix/enterprise-data-intelligence-platform-showcase

Purpose:

- Documentation
- Architecture
- Screenshots
- Project overview
- Live demo reference

## Private Source Repository

Repository:

`enterprise-data-intelligence-platform-source`

Purpose:

- Application source code
- Data processing
- Machine learning implementation
- Business logic
- Explainability
- Dashboard code
- Runtime resources

## Live Application

https://enterprise-data-intelligence-platform.streamlit.app/

The Streamlit deployment is connected to the private source repository.

## Local Development

From the local source directory:

```powershell
conda activate base

cd "C:\Users\Prati\Documents\enterprise-data-intelligence-platform-source-local"

pip install -r requirements.txt

streamlit run app.py
```

The local application is normally available at:

```text
http://localhost:8501
```

## Requirements

The application currently uses dependencies including:

```text
streamlit
pandas
numpy
scikit-learn
joblib
matplotlib
```

The source repository should contain the authoritative `requirements.txt`.

## Streamlit Community Cloud

Deployment should point to:

- Private GitHub repository
- Correct branch
- Correct main application file
- Required Python version
- Required secrets

The current deployment uses Python 3.11.

## Secrets

Secrets should never be committed to Git.

Streamlit Community Cloud provides a Secrets configuration area for runtime secrets.

Example:

```toml
[database]
username = "..."
password = "..."

[api]
key = "..."
```

Only use secrets that are actually required by the application.

## Private Repository Access

Streamlit Community Cloud must have permission to access the private source repository.

If Streamlit reports that it does not have access to private repositories, reconnect GitHub and grant the required repository access.

## Deployment Troubleshooting

### Main file does not exist

Verify the application entry point, for example:

```text
app.py
```

### Dependency error

Check `requirements.txt`.

For example, if Pandas styling requires Matplotlib, ensure:

```text
matplotlib
```

is included.

### Private repository access error

Reconnect GitHub through Streamlit Community Cloud and grant access to the private repository.

## Security

The source code remains private. The application can be public depending on Streamlit sharing settings.

The showcase repository remains public.

## Production Considerations

A production deployment would additionally benefit from:

- CI/CD
- Automated tests
- Containerization
- Monitoring
- Logging
- Authentication
- Authorization
- Database infrastructure
- Model registry
- Experiment tracking
- Data and model drift monitoring
- Secret rotation
- Audit logging
- Security reviews

## Deployment Lifecycle

```text
Developer
   ↓
Private Git Repository
   ↓
Dependency Validation
   ↓
Streamlit Deployment
   ↓
Application Startup
   ↓
Runtime Monitoring
   ↓
Bug Fix / Update
   ↓
Git Push
   ↓
Redeployment
```

