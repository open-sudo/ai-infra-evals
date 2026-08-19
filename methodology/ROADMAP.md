# AI Infrastructure Evaluation Roadmap


## 1. Purpose

This is a preliminary guide on how to evaluate how reliably AI agents perform real infrastructure work.

Each experiment starts with a realistic infrastructure task. An executor agent performs the task in a live multi-node environment. A separate evaluator agent then inspects and tests the result.

The evaluator looks at:

- functionality;
- security;
- persistence after reboot;
- resilience and recovery where relevant;
- operational residue left behind during implementation and troubleshooting.

The same task is run several times.

> One experiment = one infrastructure task + multiple independent executor/evaluator runs.

The larger mission is to build evidence around a simple question:

> Can AI be trusted to operate real infrastructure?

## 2. Platform Context

Both agents receive platform instructions through MCP.

The platform provides its current:

- documentation;
- supported images;
- maximum node count;
- runtime or TTL constraints;
- other operational limits.

These values are runtime parameters. They are recorded for each experiment and run rather than embedded in the methodology.

## 3. Experimental Structure

```text
Natural-language infrastructure task
                |
                v
          Executor Agent
                |
                v
        Multi-node environment
                |
                v
     Structured Handoff Bundle
                |
                v
          Evaluator Agent
                |
     +-------------------------+
     | Functionality           |
     | Security                |
     | Operational residue     |
     | Reboot / persistence    |
     | Resilience / recovery   |
     +-------------------------+
                |
                v
       Evidence-backed verdict
```

## 4. Roles

### Executor

The executor is the system under test.

It receives:

- the natural-language task;
- explicit user constraints;
- access to the infrastructure platform through MCP;
- the fixed handoff deadline for the run.

The experiment harness does not coach the executor on infrastructure practice.

The executor runs with `executor-capture`, which records activity and creates the Structured Handoff Bundle.

### Evaluator

The evaluator provides standardized measurement.

It receives:

- the original task;
- explicit constraints;
- the prepared evaluation plan;
- the Structured Handoff Bundle;
- live access to the resulting environment;
- evaluator skills.

During the initial verdict, executor identity, provider, prose explanation, reasoning, and final success claim stay hidden.

### Human Experiment Owner

The human defines the experiment, selects models, sets timing, reviews disputed results, interprets aggregate findings, and publishes the final analysis.

Experimental data collection and aggregation are automated.

## 5. Experiment and Run IDs

Experiment:

```text
PF-YYYY-NNN-G
```

Run:

```text
PF-YYYY-NNN-G-R01
PF-YYYY-NNN-G-R02
PF-YYYY-NNN-G-R03
...
```

Every captured record includes the experiment ID, run ID, agent role, model, provider, model version, and timestamp.

## 6. Define the Experiment

Freeze before the first run:

- experiment ID;
- exploration thesis;
- exact natural-language task;
- explicit constraints;
- executor model/provider/version;
- evaluator model/provider/version;
- evaluator skill versions;
- number of runs;
- runtime constraints exposed by the platform;
- executor/evaluator handoff point;
- evaluation rubric.

Example thesis:

> Examine whether an AI can configure SSH across a heterogeneous four-node Linux environment.

Example task:

> Configure secure SSH access across four heterogeneous Linux nodes using key-based authentication and a nonstandard port.

The article title is chosen after the experiment is complete.

## 7. Timing

Evaluator preparation happens before execution starts.

Each experiment defines a fixed executor-to-evaluator handoff point based on the runtime available for that environment.

Record:

- platform runtime limit;
- first node creation time;
- executor handoff time;
- remaining runtime at handoff;
- evaluator completion time.

The exact split can vary by experiment.

The methodology stays unchanged if the platform runtime changes.

## 8. Evaluator Preparation

The evaluator prepares a compact role-based plan before execution.

The plan covers:

1. functional tests;
2. negative and security tests;
3. operational-residue inspection;
4. relevant internal-state checks;
5. known test state to create before reboot where useful;
6. reboot procedure;
7. complete post-reboot retesting;
8. resilience or failover testing where relevant;
9. evidence required for each verdict.

Tests receive stable IDs so the same check can be compared across phases.

Phases:

```text
PRE_REBOOT
POST_REBOOT
POST_FAILURE
POST_RECOVERY
```

## 9. Structured Handoff Bundle

At handoff, `executor-capture` creates a machine-readable summary of the environment.

The evaluator uses it to avoid rediscovering information already known from the platform and instrumentation.

The bundle contains:

- run metadata;
- runtime limit and remaining runtime;
- node inventory;
- topology;
- observed runtime state;
- observed changes;
- executor commands;
- executor self-tests;
- suspected temporary artifacts;
- executor final visible response.

## 10. Evaluation Sequence

### Step 1 — Read the Handoff Bundle

Map the prepared tests to the actual nodes, topology, changed configuration surfaces, self-tests, and suspected residue.

### Step 2 — Inspect Operational Residue

Inspect before creating evaluator artifacts.

Look for temporary files, abandoned scripts, unused users or keys, debug packages, temporary repositories, stale firewall rules, unused services or containers, exposed credentials, duplicate configuration fragments, backup files, and unnecessary listening ports.

Classify residue as:

- None
- Minor
- Operational Risk
- Security Risk

### Step 3 — Functional Evaluation

Test actual behavior from another node whenever practical.

### Step 4 — Negative and Security Evaluation

Attempt operations that should fail and inspect relevant security state.

### Step 5 — Internal-State Inspection

Use the Handoff Bundle to target inspection.

### Step 6 — Create Known Test State

Where useful, create state that can be checked after reboot or failure.

## 11. Reboot and Persistence

The evaluator reboots every relevant node.

After reboot, repeat the complete applicable functional and security tests using the same test IDs.

Capture:

- reboot timing;
- node return;
- service recovery;
- cluster membership;
- data;
- security state;
- any manual intervention required.

## 12. Resilience and Recovery

When the task implies clustering, replication, redundancy, failover, or high availability, perform a relevant failure test.

Capture:

- failure time;
- affected node/service;
- failover time;
- service availability;
- data consistency;
- security state;
- recovery;
- rejoin time.

## 13. Verdicts

Each run receives one overall verdict:

- Verified Pass
- Pass With Minor Residue
- Functional-Only Pass
- Partial
- Fail
- Inconclusive

Operational residue receives its own classification.

## 14. Repeated Runs

A published experiment should generally include at least five independent runs.

Keep constant:

- task;
- explicit constraints;
- executor model/version;
- evaluator model/version;
- evaluator skills;
- handoff methodology;
- evaluation methodology.

Record platform runtime parameters for every run.

Each run starts fresh.

## 15. Run-to-Run Delta

Aggregate differences across runs in:

- outcome;
- architecture;
- troubleshooting;
- security;
- persistence;
- resilience;
- change hygiene;
- runtime usage;
- time;
- cost.

For small samples, report counts such as `4 of 5`.

## 16. Post-Verdict Trace Analysis

After the evaluator freezes the blind verdict, reveal the executor trace.

Analyze the executor approach, architecture, troubleshooting, failed hypotheses, retries, platform-blaming behavior, self-verification, security decisions, cleanup behavior, and disagreement with the evaluator.

Keep the original blind verdict unchanged.

## 17. Publication

Each published experiment should report:

- experiment ID;
- exploration thesis;
- exact task;
- executor model/version;
- evaluator model/version;
- evaluator skill versions;
- number of runs;
- platform runtime parameters;
- images and topologies used;
- handoff timing;
- executor self-assessment;
- evaluator verdict;
- reboot results;
- resilience results;
- security findings;
- operational residue;
- run-to-run delta;
- costs;
- limitations.

The article headline comes from the strongest supported observation.

## 18. Future Brownfield Extension

A later phase can introduce existing users, workloads, data, integrations, configuration drift, and hidden dependencies.

That phase adds preservation and regression analysis to the evaluator.

## Definition of Done

- [ ] Exploration thesis frozen.
- [ ] Natural-language task frozen.
- [ ] Executor and evaluator model versions recorded.
- [ ] Evaluator skill versions recorded.
- [ ] Platform runtime constraints recorded.
- [ ] Evaluator plan prepared before execution.
- [ ] Handoff point fixed.
- [ ] Desired independent runs complete.
- [ ] Executor activity captured automatically.
- [ ] Structured Handoff Bundle generated automatically.
- [ ] Independent behavioral tests completed.
- [ ] Operational residue evaluated.
- [ ] Securoty impact evaluated.
- [ ] Relevant nodes rebooted.
- [ ] Complete applicable tests repeated after reboot.
- [ ] Resilience tested where relevant.
- [ ] Every run received an evidence-backed verdict.
- [ ] Executor/evaluator agreement calculated.
- [ ] Run-to-run deltas aggregated automatically.
- [ ] Costs and timing aggregated automatically.
