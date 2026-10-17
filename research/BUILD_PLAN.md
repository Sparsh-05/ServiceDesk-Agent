# First model build plan

## Primary target

Start with `first_assignment_time_to_resolution_log1p_sec`, restricted to the first observed assignment episode per ticket and a strictly positive `ticket_resolution_date - assignment_start). The audited data have **34,950** such rows, with an untransformed median of about **8.8 days**.

This predicts observed elapsed time from first assignment start to resolution under the historic assignment sequence, including any later reassignments. It does not measure hands-on effort, and it is not the counterfactual time if a different agent had been assigned.

## Secondary target

`final_assignment_time_to_resolution_log1p_sec` has 34,773 positive final-episode labels, with median around 7.8 days. Keep this separate: it describes time from the final assignment start rather than the first/new-ticket decision.

## Baselines

1. Global historical median, fitted on training labels only.
2. Historical median by issue type and, when adequately supported, agent/project context.
3. A structured tabular model with categorical ticket/agent context and strictly prior history features.
4. Separate assignment-episode-duration and historical finality/reassignment analyses.
5. Text retrieval on eligible-text tickets only, reported separately with text coverage.

## Evaluation

- Use chronological cutoffs and one first-assignment row per ticket for the primary target.
- For retraining simulation, include a training label only if its resolution outcome was known before that simulation's cutoff.
- Report MAE and median absolute error in hours, plus log-duration metrics.
- Fit imputers/encoders only on training data.
- Do not claim offline assignment recommendations improve policy outcomes without counterfactual assumptions or a prospective shadow/live evaluation.

## Blockers for the full research objective

1. Source a verified operational SLA target and result, or revise the main claim away from SLA compliance.
2. Confirm meanings of Q1, Q2, Q3 and why 144 rows have all-zero scores.
3. Connect a current roster/availability/open-ticket feed before live workload-aware use.
4. Explain why only 53.48% of issue IDs have assignment history.
5. Define how unobserved alternative-agent outcomes will be evaluated.
