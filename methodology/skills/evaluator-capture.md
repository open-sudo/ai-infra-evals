# Skill: evaluator-capture

**Version:** 3.0

## Purpose

Capture the complete evaluator activity, evidence, timing, runtime usage, and verdict for each run.

## Metadata

Record:

- experiment_id
- run_id
- role: evaluator
- provider
- model_id
- model_version
- harness_version
- evaluator_skill_versions
- timestamp

## Preparation

Capture:

- preparation prompt;
- prepared plan;
- test IDs;
- expected outcomes;
- reboot plan;
- resilience plan.

## Handoff

Capture:

- handoff receipt time;
- Handoff Bundle version;
- platform runtime parameters;
- runtime remaining;
- actual node inventory;
- topology.

## Runtime

Capture every:

- evaluator message;
- MCP call;
- tool call;
- command;
- source node;
- target node;
- stdout;
- stderr;
- exit code;
- timestamp;
- duration;
- retry;
- error.

## Test results

For each test record:

- test_id
- phase
- expected_behavior
- observed_behavior
- evidence
- result
- interpretation

Reuse the same test ID across phases.

## Executor/evaluator agreement

Where an `EXECUTOR_SELF_TEST` corresponds to an evaluator test, record:

- executor self-test result;
- evaluator independent result;
- agreement: yes | no | partial.

## Reboots

Record:

- node;
- reboot_start;
- node_unreachable;
- node_return;
- recovery_duration;
- service recovery;
- connectivity recovery;
- runtime remaining;
- failures.

## Resilience

Record:

- injected failure;
- affected node/service;
- start time;
- expected behavior;
- observed behavior;
- failover duration;
- data state;
- security state;
- recovery time;
- rejoin result.

## Residue

For each finding record:

- node;
- artifact;
- evidence;
- severity.

Severity:

- None
- Minor
- Operational Risk
- Security Risk

## Cost

Record:

- evaluator start;
- evaluator end;
- platform runtime parameters;
- runtime consumed;
- runtime remaining at completion;
- input tokens;
- output tokens;
- cached tokens where available;
- model cost.

## Final record

Capture:

- overall verdict;
- residue classification;
- failed test IDs;
- unknown test IDs;
- executor/evaluator agreement summary;
- concise evidence summary.
