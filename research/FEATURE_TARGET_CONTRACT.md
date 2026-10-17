# Feature and target contract

## Unit of analysis

One row is an observed assignment episode, keyed by `(ticket_id, turn, agent_id, assignment_start)`. There are 59,916 episodes across 35,665 tickets and 472 agents. All episodes for one ticket must stay in the same train/validation/test partition.

## Feature rules

Features can include project, issue type, priority, ticket age at assignment, hour/day, prior turn count, prior agent assignment durations, prior agent/project/type/priority history, the agent's historically observed `Done` disposition rate before this event, and a historical interval-overlap workload proxy.

Performance summaries must use only events that ended or resolved strictly before the current assignment start. Full-period `agent_profiles.csv` aggregates are descriptive only unless recomputed as of every assignment time.

The workload proxy counts other assignment intervals active immediately before assignment start. It is not an estimate of work effort, staffing capacity, calendar availability, or live current workload.

## Candidate targets

| Target | Intended use | Limitations |
|---|---|---|
| `assignment_duration_sec` / log1p | Simple regression baseline | Elapsed assignment episode duration, not labor time. |
| `final_assignment_time_to_resolution_sec` | First main regression target candidate; 34,773 positive final-episode labels | Elapsed time after the final assignment started; long-tailed and observational. |
| `final_ticket_resolution_is_done` | Secondary ticket-disposition classifier | Not SLA success and not a counterfactual outcome for alternative agents. |
| `is_final_assignment` / reassignment | Process analysis | Historical workflow outcome, not direct agent quality. |
| Q1/Q2/Q3 manual ratings | Exploratory only | Rubric and zero-score semantics are not documented. |
| SLA success/violation | Not currently usable | No verified operational target/deadline and result label. |

## Prohibited post-assignment predictors

Do not use assignment end/duration, workflow duration, final-assignment marker, reassignment marker, final assignee, resolution timestamp/disposition/status, wall-clock resolution durations, end-of-ticket workflow aggregates, or full-period agent profiles as predictors for the same assignment outcome. Keep target columns separate from feature matrices.

Use only public comments timestamped at or before assignment time; exclude private comments by default. Text coverage is limited to a small fraction of tickets.

## Evaluation

1. Split chronologically and group by ticket ID; no ticket can occur in multiple partitions.
2. For a time-based training replay, a target may enter training only after its outcome was observable at that training cutoff.
3. Fit preprocessing and the model on training data only.
4. Compare with simple history-based duration baselines.
5. Report predictive accuracy separately from retrieval/assignment recommendation metrics and from claims about policy effect.
6. Logged outcomes are observed for historically assigned agents only. Offline prediction alone cannot prove better outcomes for agents who were not assigned.
