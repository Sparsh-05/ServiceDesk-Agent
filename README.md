# ServiceDesk-Agent

Research prototype for outcome-aware ticket assignment. The first phase is data audit and leakage-safe preparation; SLA-optimization claims are deliberately blocked until operational SLA labels and a counterfactual evaluation design are available.

## Status

- Operational data: 66,691 tickets in `issues.csv`; 59,916 assignment episodes across 35,665 tickets and 472 agents.
- Primary target candidate: observed elapsed time from the **first assignment start** to ticket resolution, with 34,950 valid labels. It reflects the historical assignment sequence, including later reassignment, and is not a counterfactual for alternative agents.
- Secondary target candidate: 34,773 valid rows measuring elapsed time from the final assignment start to resolution.
- Text: only 339 of 35,665 history tickets have public text recorded by the first assignment; semantic retrieval is currently a limited-coverage experiment.
- Missing: a verified operational SLA target/SLA-met label and a documented rubric for Q1/Q2/Q3.
- Model status: GO for a first structured predictive baseline; NO-GO for claims of SLA-compliance improvement or live availability.

## Data handling

Keep raw operational datasets and derived training tables local and git-ignored. Do not commit `data/raw/`, `data/processed/`, trained model files, private comments, or user-sensitive records. Do not combine the small `historical_tickets.csv` / `ticket_agent_features.csv` demonstration tables with the operational data: their IDs and labels do not match.

## Preparation package

The audited preparation scripts and derived tables are available in the accompanying local project bundle. Key ideas:
- Features are constructed as-of assignment start; target/outcome columns are kept in a separate table.
- Agent performance uses only outcomes strictly preceding the assignment timestamp.
- Workload is a historical overlapping-assignment count, not real-time availability.
- The primary model uses one first-assignment row per ticket; validation respects chronology and outcome-observation times.
- Do not equate `Done` with SLA success.

See `research/DATA_READINESS.md`, `research/FEATURE_TARGET_CONTRACT.md`, and `research/BUILD_PLAN.md` for data support, leakage rules, and the planned first baseline.
