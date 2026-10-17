# ServiceDesk-Agent data readiness — audited 18 October 2026

## Decision

**GO for the first structured, leakage-safe predictive baseline. NO-GO for an SLA-optimization claim.**

## Inventory

| Source | Rows | Columns |
|---|---:|---:|
| `issues.csv` | 66,691 | 58 |
| `issues_snapshot.csv` | 90,963 | 60 |
| `issues_change_history.csv` | 257,508 | 6 |
| `sample_utterances.csv` | 30,104 | 9 |
| `ticket_agent_history.csv` | 59,916 | 26 |
| `agent_profiles.csv` | 472 | 21 |
| `issues_snapshot_sample.xlsx` | 747 | 19 |

The assignment history covers 35,665 of 66,691 operational ticket IDs (53.48%), across 472 agents. The reason for missing history on the remaining tickets still needs investigation.

## Supported targets

- There are 59,916 observed assignment episodes with elapsed assignment-duration values. The median is about 231,440 seconds (64.3 hours); this is elapsed time assigned, not hands-on labor.
- **Primary initial target:** 34,950 first-assignment rows have a strictly positive `ticket_resolution_date - assignment_start`. The median is about 762,003 seconds (8.8 days). This predicts observed elapsed time from the first recorded assignment to resolution under the historic assignment sequence, including later reassignments. It is not active labor time and not an alternative-agent counterfactual.
- **Secondary target:** 34,773 final-assignment rows have a strictly positive `ticket_resolution_date - assignment_start`; median about 671,775 seconds (7.8 days). It addresses time from the final assignment start, a different question.
- 16,483 tickets are marked as reassigned.
- There is **no verified operational SLA target or SLA-met/SLA-violated label** in the audited operational schema. `Done` and `done_resolution_rate` must not be called SLA success.

## Text and human evaluation

Only 339 of 35,665 tickets in the assignment history have a public comment timestamped at or before the first assignment (0.95%). Public comments have a median length of 12 characters. Semantic retrieval is therefore a narrow-coverage exploratory experiment, not a representative full-corpus baseline.

The human-evaluation workbook has 747 assignment rows across 360 tickets. 603 rows have a nonzero score across Q1–Q3, spanning 293 tickets and 90 agents. 144 rows have all three scores equal to zero. The Q1/Q2/Q3 rubric and the meaning of zero are not documented; do not train on these scores as a definitive target until clarified.

## Workload and leakage

Assignment intervals can support a retrospective overlapping-assignment count. It is only a proxy for concurrent assigned tickets—not live availability, effort, or true capacity. The historical data end in March 2023, so this cannot represent current 2026 workload.

Features must be derived as-of assignment start. Exclude assignment end/duration, final-assignee fields, ticket resolution/status, end-of-ticket comment counts, and future agent-profile aggregates from predictors. Keep targets separate from features.

## Go/no-go

**GO:** first-assignment elapsed-time baseline, simple assignment-duration and reassignment/finality analysis, and a limited text-retrieval experiment.

**NO-GO:** validated SLA-success prediction, live availability-aware assignment, or a claim that the new policy causally improves SLA compliance. Those require an actual SLA label, a current workforce feed, and a credible counterfactual/prospective evaluation plan.
