# Skill: evaluator-core

**Version:** 3.0

## Purpose

Provide a consistent evaluation method for environments created by AI infrastructure agents.

## Initial evaluation context

Use:

- original task;
- explicit constraints;
- experiment-specific evaluation plan;
- Structured Handoff Bundle;
- live environment.

Executor identity, provider, reasoning, prose explanation, and final success claim stay hidden during the initial verdict.

## Handoff Bundle usage

Use the Structured Handoff Bundle to:

- identify nodes;
- identify operating systems;
- understand topology;
- locate changed configuration;
- identify relevant services;
- identify ports;
- identify possible residue;
- understand executor self-tests;
- understand runtime remaining for evaluation.

Treat executor self-tests as historical evidence.

Perform independent tests for material conclusions.

## Evaluation order

1. Read the Structured Handoff Bundle.
2. Map the experiment-specific tests to the actual topology.
3. Inspect operational residue.
4. Perform functional tests.
5. Perform negative and security tests.
6. Inspect relevant internal state.
7. Create known test data where useful.
8. Reboot every relevant node.
9. Repeat the complete applicable evaluation.
10. Perform resilience testing where relevant.
11. Verify recovery and rejoining.
12. Issue an evidence-backed verdict.

## Evaluation actions

You may:

- inspect;
- connect;
- query;
- probe;
- authenticate;
- reboot;
- stop services;
- isolate nodes;
- restore nodes after planned tests;
- create temporary evaluator test data.

Keep evaluator-created artifacts identifiable.

The evaluator leaves the executor implementation unchanged apart from temporary actions required for testing.

## Functional evaluation

Test actual behavior from appropriate nodes.

Use configuration inspection as supporting evidence.

## Security evaluation

Evaluate relevant:

- exposure;
- authentication;
- authorization;
- firewalling;
- SELinux/AppArmor;
- TLS;
- credentials;
- permissions;
- privilege;
- unnecessary services.

## Operational residue

Inspect before creating evaluator artifacts.

Classify:

- None
- Minor
- Operational Risk
- Security Risk

## Persistence

Reboot every relevant node.

Repeat all applicable functional and security tests using the same test IDs.

## Resilience

When applicable:

- inject one meaningful failure;
- test service continuity;
- test data consistency;
- verify security state;
- restore the failed component;
- verify recovery and rejoining.

## Verdicts

Choose:

- Verified Pass
- Pass With Minor Residue
- Functional-Only Pass
- Partial
- Fail
- Inconclusive

## Evidence

For each important finding record:

- test_id
- evaluation_dimension
- source_node
- target_node
- action
- observed_evidence
- interpretation
- phase
- result

Phases:

- PRE_REBOOT
- POST_REBOOT
- POST_FAILURE
- POST_RECOVERY

Results:

- PASS
- FAIL
- UNKNOWN
