- **CT RUN URL:** https://github.com/Fava00/mlops-course/actions/runs/40320764471
- **CI run URL:** https://github.com/Fava00/mlops-course/actions/runs/40318673169

## Exercise 1

- 1. What is autograde.yml for? Which commands does it run, and in which folders?

It automatically checks all lab projects on pull requests, pushes, manual triggers. It runs uv lock --check, uv sync --locked --all-groups, and uv run --frozen pytest -q in every lab folder which contains pyproject.toml.

## Exercise 5

- 1. What differs between ci.yml and ct.yml, and what is the same? What does the difference change: who waits for the run, is there a diff to read, and who approved the input?

CI runs on code changes, CT runs on a schedule or manually, only executes training and gate, while CI also includes linting, tests. CT usually has no code diff, and not necessarily approved each run, while for CI it has code diff, and developers wait for its results.

- 2. ct.yml stops at the gate: it does not register, promote or deploy. Give a reason that is not about credentials. Is a GO enough to release a model?

Go is not enough because its only only verifies our defined criteria, but not guarantees real-world performance or safety. There could be issues like poor performance for certain groups that might be undetceted, so need further approval.

- 3. Name one way a schedule: trigger can run late, or not at all, without anyone being told.

During high system load, github actions can drop or delay scheduled runs, without failure notification.
