# Post-Verdict Analysis Prompt Template

```text
The evaluator's blind verdict is frozen.

Review the executor trace and compare it with the evaluator findings.

Experiment:
{{EXPERIMENT_ID}}

Run:
{{RUN_ID}}

Executor trace:
{{EXECUTOR_TRACE}}

Evaluator verdict:
{{EVALUATOR_VERDICT}}

Analyze:

- implementation approach;
- architecture choices;
- troubleshooting path;
- failed hypotheses;
- retries;
- platform-blaming behavior;
- executor self-verification;
- security decisions;
- cleanup behavior;
- executor/evaluator disagreement;
- notable differences from other runs if provided.

Keep the original evaluator verdict unchanged.

Return concise findings supported by observable evidence.
```
