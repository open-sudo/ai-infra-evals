# Skill: executor-capture

**Version:** 3.0

## Purpose

Capture the complete observable executor activity for an infrastructure experiment and generate the Structured Handoff Bundle automatically.

This skill provides instrumentation.

## Metadata

Attach to every record:

- experiment_id
- run_id
- role: executor
- provider
- model_id
- model_version
- harness_version
- timestamp

## Agent activity

Capture:

- system prompt;
- user/task prompt;
- visible model responses;
- MCP calls and responses;
- tool calls and responses;
- errors;
- retries;
- completion message.

Capture observable agent behavior.

## Platform activity

Record every infrastructure-platform MCP operation, including:

- node creation;
- node destruction;
- image selection;
- image version;
- network creation;
- interfaces;
- addresses;
- reboot operations;
- platform errors;
- runtime information exposed by the platform.

## Commands

For every command record:

- node;
- timestamp;
- command;
- exit_code;
- stdout;
- stderr;
- duration.

## System changes

Where observable, record:

- files created, modified, or deleted;
- ownership and permission changes;
- packages installed or removed;
- services started, stopped, enabled, or disabled;
- listening-port changes;
- firewall changes;
- route changes;
- SELinux/AppArmor changes;
- certificates and keys;
- user/group changes.

## Timing and cost

Record:

- executor start;
- first node creation;
- platform runtime parameters;
- each node creation time;
- each tool-call time;
- handoff time;
- runtime remaining at handoff;
- input tokens;
- output tokens;
- cached tokens where available;
- model cost.

## Executor self-tests

Identify observable commands or tool calls used by the executor to validate its own implementation.

Record:

- test action;
- source node;
- target node;
- output;
- result;
- time.

Label these records:

```text
EXECUTOR_SELF_TEST
```

Keep them separate from evaluator test results.

## Structured Handoff Bundle

At handoff, generate:

### Run

- experiment_id
- run_id
- first_node_created
- platform_runtime_parameters
- handoff_time
- runtime_remaining

### Nodes

For every node:

- node_id
- hostname
- image
- image_version
- addresses
- interfaces
- created_at
- status

### Topology

Record:

- networks
- membership
- interfaces
- routing information where available

### Observed runtime state

Where available:

- running services
- enabled services
- listening ports
- relevant processes

### Observed changes

Record:

- files changed
- packages changed
- services changed
- firewall changes
- route changes
- users/groups
- keys/certificates
- security-control changes

### Self-tests

Include the structured `EXECUTOR_SELF_TEST` records.

### Suspected residue

Identify observable artifacts that may be temporary:

- temporary files
- scripts
- test users
- temporary keys
- backup config files
- temporary firewall rules
- debug services
- temporary repositories

Label these as candidates. The evaluator determines their significance.

### Executor final response

Capture the final visible executor response.

Store executor identity separately so the handoff bundle can be supplied to the evaluator without revealing model or provider.

## Output

Produce:

1. event stream;
2. command log;
3. change record;
4. cost/timing record;
5. Structured Handoff Bundle.

Use stable machine-readable schemas across runs.
