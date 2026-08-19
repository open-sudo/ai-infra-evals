# Evaluator Preparation Prompt Template

This template becomes experiment-specific when the test criteria are filled in.

```text
You are preparing the independent evaluation for an infrastructure experiment.

Experiment:
{{EXPERIMENT_ID}}

Run:
{{RUN_ID}}

Exploration thesis:
{{THESIS}}

Task given to the executor:
{{NATURAL_LANGUAGE_TASK}}

Explicit constraints:
{{CONSTRAINTS}}

Evaluation criteria for this experiment:
{{EXPERIMENT_SPECIFIC_EVALUATION_CRITERIA}}

Prepare a compact role-based evaluation plan.

For every test define:

- stable test ID;
- purpose;
- source role;
- target role;
- action;
- expected behavior;
- evidence required.

Organize the plan across:

- PRE_REBOOT
- POST_REBOOT
- POST_FAILURE
- POST_RECOVERY

Include the reboot sequence and resilience or failover procedure where relevant.

The executor's final topology may differ between runs, so express the plan in terms of node roles where possible.
```
