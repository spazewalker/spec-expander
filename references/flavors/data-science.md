# Flavor: Data science / ML

## Dimensions to cover
- **Decision:** what decision or action the output supports, and who consumes it (exec deck, product feature, internal analyst).
- **Target and framing:** exact label definition, prediction horizon, unit of analysis, classification vs regression vs ranking vs causal question.
- **Data:** sources, size, time span, known quality issues, leakage risks (features unavailable at prediction time), privacy constraints.
- **Evaluation:** primary metric tied to the decision (e.g. precision@k for outreach budgets, calibration for risk scores), baseline to beat, validation scheme (time-based split for temporal data).
- **Constraints:** interpretability needs, inference latency, retraining cadence, compute budget.
- **Deliverable:** notebook, report, dashboard, model artifact, API, or pipeline; reproducibility expectations.

## Common defaults ([pattern], override from profile/project)
- EDA first: data profile, missingness, target distribution, obvious leakage.
- A simple baseline (majority class, logistic regression, or last-value) before complex models; gradient-boosted trees as the strong tabular default.
- Time-based split when there's a time dimension; otherwise stratified k-fold.
- Report uncertainty (confidence intervals or variance across folds), not just point metrics.
- Fixed random seeds; pinned environment; data version noted.
- Interpretability for stakeholder-facing work (feature importance, SHAP, or simple rules).

## Typical checks
| Requirement | Check |
|---|---|
| Beats baseline | metric table: baseline vs model on a held-out set |
| No leakage | feature availability audit at prediction time; suspiciously high scores investigated |
| Robust | metric stability across folds, time periods, and key segments |
| Calibrated (if probabilities used) | calibration curve / Brier score |
| Reproducible | a clean run from raw data reproduces the reported numbers |
| Stakeholder-ready | one-page summary with the decision-relevant metric in plain language |

## Questions that usually matter
- What will be done with the predictions, and what does a false positive vs false negative cost?
- Exact definition of the target (e.g. "churn" = no purchase in 90 days? cancelled subscription?).
- Who is the audience, and how much interpretability do they need?
- One-off analysis or something that must run repeatedly?
