
**Self-healing ML infrastructure**: detect data drift, trigger retraining, redeploy automatically. No human in the loop.

Part of the [AWS ML Engineering (MLA-C01)](https://aws.amazon.com/certification/certified-machine-learning-engineer-associate/) course by CTA.

---

## Architecture

```
Production Endpoint (XGBoost)
        │
        ▼
Data Capture ──► S3 (inference logs)
        │
        ▼
SageMaker Model Monitor
  ├── Baseline job (training data)
  └── Monitoring job (compares live vs. baseline)
        │
        ▼ drift detected
CloudWatch Alarm
        │
        ▼
SNS Topic (notification)
        │
        ▼
Lambda Function
        │
        ▼
Step Functions State Machine
  ├── Retrain model on new data
  └── Deploy new model to endpoint
```

---

## What this lab covers

| Task | Description |
|------|-------------|
| 2.1 | Set up environment, install dependencies |
| 2.2 | Upload pre-trained XGBoost model, create production endpoint with **Data Capture** enabled |
| 2.3 | Invoke endpoint with 200 new abalone records, verify captured data in S3 |
| 2.4 | Create **Model Monitor baseline** from training data (statistics + constraints) |
| 2.5 | Schedule **monitoring job** to compare inference distributions against baseline |
| 2.6 | Configure **CloudWatch alarm** on Model Monitor metric violations |
| 2.7 | Create **SNS topic** + Lambda function to receive alarm notifications |
| 2.8 | Build **Step Functions** state machine: retrain → evaluate → deploy |
| 3.x | Review automatic retraining workflow execution end to end |

---

## Key AWS services

- **Amazon SageMaker** — model training, hosting, Model Monitor, Pipelines
- **Amazon S3** — data capture storage, model artifacts, baseline data
- **Amazon CloudWatch** — monitoring metrics, alarms
- **Amazon SNS** — alarm notifications
- **AWS Lambda** — event-driven trigger on alarm
- **AWS Step Functions** — orchestration of retrain + redeploy workflow

---

## Dataset

**Abalone** — predict the age of abalone (shellfish) from physical measurements.  
Used as the training baseline and for simulating inference drift with new prediction data.

---

## Key concepts

**Data Capture**: SageMaker records every inference request and response to S3 automatically. This is the foundation for monitoring — you can't detect drift without the data.

**Baseline**: Model Monitor runs a one-time job on your training data to compute statistics (mean, std, quantiles) and constraints (expected data types, value ranges). This becomes the reference for what "normal" looks like.

**Drift detection**: The scheduled monitoring job compares live inference distributions against the baseline. Violations (e.g. feature distribution shift, missing values) are logged to CloudWatch.

**Self-healing loop**: CloudWatch alarm → SNS → Lambda → Step Functions. When drift is detected, the pipeline retrains on the new data and redeploys without manual intervention.

---

## Exam relevance (MLA-C01)

- Model Monitor is a managed solution for **data quality**, **model quality**, **bias drift**, and **feature attribution drift**
- CloudWatch integration is the standard pattern for alerting on SageMaker metrics
- Step Functions is the preferred AWS service for **ML workflow orchestration** (vs. SageMaker Pipelines for training pipelines specifically)
- SNS + Lambda = event-driven architecture pattern frequently tested

---

## Status

✅ Completed — June 2026  
🎯 MLA-C01 exam target: September 2026
