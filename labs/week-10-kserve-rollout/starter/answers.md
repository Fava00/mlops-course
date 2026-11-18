# Exercise 2

**1. Pick one item from your "promote.when" list and one from your "rollback.when" list. For each, name the signal it watches and how soon after the canary starts it can decide. Why can neither of them use the true diagnosis?**

    For promote, I would check if the candidate's error rate is maximum 1%, its p95 latency is below 250 ms, with at least 100 requests. It will take about 3 and 1/3 days on average with 300 requests/day and 10% canary.
    For roll back, I would roll back if the candidate's smoke test fails. I can make this decision after the test finished.


**2. At 300 requests a day, how many patients a day reach the candidate at your canary.percent? Is that enough to show that v2 is better than v1? What can it show?**

    300 * 0.1 = 30 requests per day on average. At this volume, I can spot obvious failures, like frequent HTTP errors or large latency increases. Establishing whether v2 is more accurate than v1 requires enough labeled outcomes from both versions.

**3. Which min_probability_spread did you choose? make smoke-local prints the spread of each version. Which value would make the spread check catch v3 as well, and what would a value close to v2's spread risk?**

    I chose 0.10. The measured spreads were 0.9193 for v1, 0.7100 for v2, and 0.1978 for v3. A threshold of 0.25 would catch v3 while passing v1 and v2. A threshold close to 0.71 could fail v2 after even a small decrease in its spread.

# Exercise 3

**4. Your measured percentage is not exactly your canary.percent. Why not? How many requests would you need before a difference of one percentage point meant something?**

    v2 served 15 of 200 requests (7.5%), and v1 served 185 (92.5%). With a configured 10% share, the sampling standard error is about 2.1 percentage points, so the 2.5-point difference is consistent with sampling variation. A 95% margin of error of ±1 percentage point requires roughly 3,500 requests (`1.96² × 0.10 × 0.90 / 0.01²`).

# Exercise 4

**5. The v3 pod is Ready and would answer every request with HTTP 200. Which checks failed, and which passed? If you could keep only one check, which would it be, and why?**

    v3 was Ready, but `identity.gate` failed because it reported `NO-GO`, and `behaviour.positive_is_positive` failed because it predicted `0` for the positive patient (`p=0.4751`). Health, version, input validation, negative-patient, probability-spread, and protocol-agreement checks passed. I would keep the positive-patient check because it catches the missed positive in the deployed model even when the pod is Ready and both routes agree.

# Exercise 5

**6. What did the rollback change, and what did it not put back? Look at the registry alias, metrics/baseline.json, the DVC files and the answers v3 gave while it had traffic.**

    `make rollback` set `spec.predictor.canaryTrafficPercent` from `10` to `0`, returning all main-endpoint traffic to v1 in a 5.790-second command run. Restoring the MLflow registry alias, `metrics/baseline.json`, or DVC state would require separate actions. Responses already returned by v3 would need to be identified and reviewed separately.

**7. Write the rollback steps for this model as a short list, as you would add them to the rollout plan. Which step did you just test, and which one have you not tested yet?**

    1. Run `make rollback` to set canary traffic to 0%.
    2. Verify v1 at 100% and v3 at 0% with `make isvc-status`. Run `make isvc-health` and confirm v1 on `diabetes-predictor-00001`.
    3. If KServe reports `RevisionMissing`, run `make rollback-reapply`, then repeat both checks.

    I tested the traffic rollback and both verification checks. The fallback in step 3 is untested.


# Exercise 6

**8. At which concurrency does the service stop meeting your rollout plan? What would you tell the platform team, and which numbers would you give with it?**

    Concurrency 16 was the first level to exceed the 250 ms p95 limit, at 613.64 ms and 73.6 requests/s. Concurrency 4 met both limits at 99.24 ms p95 and 66.5 requests/s. At concurrency 64, p95 rose to 2,124.93 ms and throughput dropped to 62.0 requests/s. Error rates stayed within the 1% limit at all levels.
