# Lab 7 — Monitor a Model for Data Drift

> Add this section to your `XDT-AI/aws-ml-prep` README under the Labs section.

---

## Lab 7 · SageMaker Model Monitor for Data Drift ✅

**Objective**: Build a self-healing ML pipeline that detects data drift and automatically retrains and redeploys the production model.

**Architecture overview**:
```
Endpoint (Data Capture) → S3 → Model Monitor → CloudWatch Alarm → SNS → Lambda → Step Functions → Retrain & Redeploy
```

**Services covered**: SageMaker Model Monitor · Data Capture · CloudWatch · SNS · Lambda · Step Functions

**Key takeaways**:
- Data Capture must be enabled at endpoint creation — it cannot be added retroactively
- Model Monitor baseline separates *statistics* (computed values) from *constraints* (rules to enforce)
- CloudWatch alarm threshold is set on the `feature_baseline_drift` metric
- Step Functions is the right tool for ML workflow orchestration (retrain → evaluate → deploy)
- The full loop runs without human intervention once configured

**Weak spots to revisit for MLA-C01**:
- `ModelQualityMonitor` vs `DataQualityMonitor` — which metric each tracks
- Step Functions `Choice` state for conditional branching in retraining workflows
- IAM role requirements for Model Monitor to write to CloudWatch

**Completed**: June 2026 | Lab access via CTA (expires July 29, 2026)
