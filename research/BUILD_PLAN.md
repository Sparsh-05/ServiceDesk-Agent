# First model build plan

## Target

Start with `final_assignment_time_to_resolution_log1p_sec`, restricted to rows with a final assignment and a strictly positive `ticket_resolution_date - assignment_start`. The audited data have 34,773 such rows, with a median untransformed time around 7.8 days.

This target is elapsed time after the final assignment begins—not hands-on effort. It is not SLA compliance and does not by itself identify the causal effect of assigning an alternative agent.

## Baselines

1. Global historical median, fitted on training labels only.
2. Historical median by issue type and, when adequately supported, agent/project context.
3. A structured tabular model with categorical ticket/agent context and strictly prior history features.
4. Separate assignment-episode-duration and historical finality/reassignment analyses.
5. Text retrieval on eligible-text tickets only, reported separately with text coverage.

## Evaluation

- Chronological split by first assignment time; keep tickets grouped.
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
