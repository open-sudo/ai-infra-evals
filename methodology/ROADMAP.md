# AI Infrastructure Evaluation Roadmap

## Purpose

This methodology measures how reliably AI agents perform real infrastructure work.

Each experiment starts with a realistic infrastructure task. An executor agent performs the task in a live multi-node environment. A separate evaluator agent then tests the resulting system.

The evaluator looks at:

- functionality;
- security;
- persistence after reboot;
- resilience and recovery where relevant;
- operational residue left behind during implementation and troubleshooting.

The same task is run several times.

> One experiment = one infrastructure task + multiple independent executor/evaluator runs.

The larger mission is:

> **Can AI be trusted to operate real infrastructure?**

## Platform context

Both agents receive platform instructions through MCP.

The platform supplies its current documentation, supported images, node limits, runtime constraints, and other operational details.

These values are runtime parameters. Each run records the values exposed by the platform.

## Repository model

Generic behavior belongs in skills.

Experiment-specific details belong in prompts.

```text
skills/
  executor-capture.md
  evaluator-core.md
  evaluator-capture.md

prompts/
  executor-template.md
  evaluator-prep-template.md
  evaluator-runtime-template.md
  post-verdict-analysis-template.md

experiments/
  EXP-YYYY-NNN-G/
    executor-prompt.md
    evaluator-prep-prompt.md
    evaluator-runtime-prompt.md
```

## Experimental flow

```text
Natural-language task
        |
        v
Executor Agent
+ executor-capture
        |
        v
Live multi-node environment
        |
        v
Structured Handoff Bundle
        |
        v
Evaluator Agent
+ evaluator-core
+ evaluator-capture
        |
        v
Functionality
Security
Operational residue
Reboot / persistence
Resilience / recovery
        |
        v
Evidence-backed verdict
```

## Roles

### Executor

The executor is the system under test.

It receives:

- the natural-language task;
- explicit constraints;
- platform access through MCP;
- the configured handoff point.

The experiment harness gives the executor no infrastructure coaching.

The `executor-capture` skill records observable activity and creates the Structured Handoff Bundle.

### Evaluator

The evaluator provides standardized measurement.

It receives:

- the original task;
- explicit constraints;
- a prepared experiment-specific test plan;
- the Structured Handoff Bundle;
- live access to the environment;
- `evaluator-core`;
- `evaluator-capture`.

During the initial verdict, executor identity, provider, prose explanation, reasoning, and final success claim stay hidden.

### Human experiment owner

The human defines the experiment, selects models, reviews disputed findings, interprets aggregate results, and publishes the analysis.

Data collection and run aggregation are automated.

## Experiment IDs

Experiment:

```text
PF-YYYY-NNN-G
```

Runs:

```text
PF-YYYY-NNN-G-R01
PF-YYYY-NNN-G-R02
PF-YYYY-NNN-G-R03
...
```

Every captured record includes the experiment ID, run ID, role, model, provider, model version, and timestamp.

## Defining an experiment

Freeze these values before the first run:

- experiment ID;
- exploration thesis;
- exact natural-language task;
- explicit constraints;
- executor model/provider/version;
- evaluator model/provider/version;
- skill versions;
- number of runs;
- handoff point;
- evaluation criteria.

Example thesis:

> Examine whether an AI can configure SSH across a heterogeneous four-node Linux environment.

Example task:

> Configure secure SSH access across four heterogeneous Linux nodes using key-based authentication and a nonstandard port.

The article title is chosen after the results are known.

## Evaluator preparation

The evaluator plan is prepared before execution starts.

The experiment-specific evaluator preparation prompt defines:

1. functional tests;
2. negative and security tests;
3. operational-residue checks;
4. internal-state checks;
5. known test state to create before reboot where useful;
6. reboot procedure;
7. post-reboot retesting;
8. resilience or failover tests where relevant;
9. evidence required for each verdict.

Tests receive stable IDs.

Example:

```text
SSH-FUNC-001
SSH-NEG-001
SSH-SEC-001
```

Phases use the same IDs:

```text
PRE_REBOOT
POST_REBOOT
POST_FAILURE
POST_RECOVERY
```

## Structured Handoff Bundle

At handoff, `executor-capture` produces a machine-readable summary.

The evaluator uses it to spend live-environment time on validation rather than rediscovery.

The bundle contains:

- run metadata;
- runtime information exposed by the platform;
- node inventory;
- topology;
- observed runtime state;
- observed changes;
- command history;
- executor self-tests;
- suspected temporary artifacts;
- executor final visible response.

Executor self-tests are historical evidence. The evaluator still performs independent tests for material conclusions.

## Evaluation sequence

1. Read the Structured Handoff Bundle.
2. Map the prepared tests to the actual topology.
3. Inspect operational residue before creating evaluator artifacts.
4. Run functional tests.
5. Run negative and security tests.
6. Inspect relevant internal state.
7. Create known test state where useful.
8. Reboot every relevant node.
9. Repeat the complete applicable evaluation.
10. Run resilience or failover tests where relevant.
11. Verify recovery and rejoining.
12. Issue an evidence-backed verdict.

## Evaluation dimensions

### Functionality

Test the requested behavior from another node whenever practical.

### Security

Evaluate relevant exposure, authentication, authorization, firewalling, mandatory access controls, TLS, credential handling, permissions, privilege, and unnecessary services.

### Persistence

Reboot every relevant node and repeat the applicable functional and security tests.

### Resilience

Where the task involves clustering, replication, redundancy, failover, or high availability, inject a relevant failure and verify recovery.

### Operational residue

Inspect for unnecessary state left behind by implementation or troubleshooting.

Examples:

- temporary files;
- abandoned scripts;
- unused users or keys;
- debug packages;
- temporary repositories;
- stale firewall rules;
- unused services or containers;
- exposed credentials;
- duplicate configuration fragments;
- backup files;
- unnecessary listening ports.

Residue classification:

- None
- Minor
- Operational Risk
- Security Risk

## Verdicts

Each run receives one overall verdict:

- Verified Pass
- Pass With Minor Residue
- Functional-Only Pass
- Partial
- Fail
- Inconclusive

Operational residue receives its own classification.

## Repeated runs

A published experiment should generally include at least five independent runs.

Keep constant:

- task;
- explicit constraints;
- executor model/version;
- evaluator model/version;
- skill versions;
- handoff methodology;
- evaluation methodology.

Each run starts fresh.

## Run-to-run delta

Aggregate differences across:

- outcome;
- architecture;
- image and distribution choices;
- troubleshooting;
- security;
- persistence;
- resilience;
- operational residue;
- runtime usage;
- cost.

For small samples, report counts such as `4 of 5`.

## Post-verdict trace analysis

After the evaluator freezes the blind verdict, reveal the executor trace.

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
- executor/evaluator disagreement.

The original blind verdict stays preserved.

## Publication

Each article should report:

- experiment ID;
- exploration thesis;
- exact task;
- executor model/version;
- evaluator model/version;
- skill versions;
- number of runs;
- platform runtime parameters;
- images and topologies;
- executor self-assessment;
- evaluator verdicts;
- reboot results;
- resilience results;
- security findings;
- operational residue;
- run-to-run delta;
- time and cost;
- limitations.

The headline comes from the strongest supported observation.

## Future brownfield work

A later phase can introduce existing users, workloads, data, integrations, configuration drift, and hidden dependencies. That phase adds preservation and regression analysis.

## Definition of done

- [ ] Natural-language task frozen.
- [ ] Executor and evaluator versions recorded.
- [ ] Skill versions recorded.
- [ ] Evaluator plan prepared before execution.
- [ ] Handoff point defined.
- [ ] Defined number of  runs completed.
- [ ] Executor activity captured automatically.
- [ ] Structured Handoff Bundle generated automatically.
- [ ] Evaluator used the bundle.
- [ ] Independent behavioral tests completed.
- [ ] Operational residue evaluated.
- [ ] Security impact evaluated.
- [ ] Relevant nodes rebooted.
- [ ] Applicable tests repeated after reboot.
- [ ] Resilience tested where relevant.
- [ ] Every run received an evidence-backed verdict.
- [ ] Executor/evaluator agreement calculated.
- [ ] Run-to-run deltas aggregated automatically.
- [ ] Costs and timing aggregated automatically.
