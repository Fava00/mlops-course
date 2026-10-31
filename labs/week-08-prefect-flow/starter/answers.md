Scheduled run
- **name:**  vociferous-dachshund
- **final state:** Completed

## Exercise 2

1. Name three capabilities Prefect provides for this lab that DVC does not, and one capability DVC provides that Prefect does not. Explain which problem each capability solves.

Prefect provides:
    - retries for failed tasks, so a team can recover from temporary failures
    - a team can schedule automatic workflow runs at predefined tims
    - prefect recods durations, logs, execution history

DVC provides content-based dependency tracking.

2. run_pipeline calls dvc repro as a single task. What would you lose by replacing it with one Prefect task per DVC stage? When might splitting the stages into separate tasks be useful?

It could lose DVC's automatic dependency tracking. Splitting them, might be useful for retries, or running stages on different resources.

## Exercise 5

1. Is passing the quality gate enough to release a model trained by a scheduled job? What other checks or approvals would you require before it reaches users?

No its not enough, because the model needs performance check, fairness check, data validation, etc.

2. What should be notified, and to whom?

ML team should be notified of failed runs, successful training runs, quality-gate results, with metrics, errors.

3. What should happen on the second failure in a row that should not happen on the first?

It should notify the engineer who's responsible.

4. Which scheduled-job failure would these checks and notifications still miss?

Silent failures, like model producing unreliable predictons, data drift.