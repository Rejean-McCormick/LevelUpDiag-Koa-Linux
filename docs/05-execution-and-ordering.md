# Execution and ordering

This document describes the core behavior of LevelUpDiag-Koali: align a series of levels and produce a readable execution flow.

## Primary order

Levels remain ordered by their identifier or by an explicit `order` field.

Example:

```text
N00 Control Panel
N01 Environment
N02 Repository
N03 Documentation
N04 Contracts
N05 Components
N06 Integrations
N07 Profiles
N08 Security
N09 Offline
N10 System
N11 Delivery
```

The exact list belongs to the manifest.

## Dependencies

Numeric order indicates the general progression.

`depends_on` expresses mandatory dependencies.

Example:

```text
N01 Environment
 ├─→ N03 Documentation
 ├─→ N04 Contracts
 └─→ N05 Components

N04 Contracts
 └─→ N06 Integrations
```

## Execution plan

Before execution, the runner builds a level list:

1. filter disabled levels;
2. verify files;
3. verify dependencies;
4. order levels;
5. determine which levels are immediately executable;
6. run levels;
7. record each result;
8. continue or block according to campaign policy.

## Sequential execution

This is the default mode.

```text
N01 → N02 → N03 → N04
```

It is the easiest mode to diagnose.

## Parallel execution

Parallelism may be used only for independent levels.

```text
       ┌→ N03
N01 ───┼→ N04
       └→ N05
```

The runner must never parallelize two levels when an explicit dependency exists.

## Failed precondition

If N04 depends on N03 and N03 is `BLOCKED`, N04 normally becomes `BLOCKED` with a dependency-related reason.

A functional failure (`FAIL`) may either stop dependents or allow them to run if their result remains useful. This behavior must be declared at the campaign or level scope, not left implicit.

## Timeout

Each level may define a timeout.

When it is exceeded:

- the process is stopped;
- the result records the timeout;
- available output is preserved;
- the runner continues according to campaign policy.

## `.pyw` windows

`.pyw` levels may be launched with windowed Python.

For an automated campaign, prefer a non-interactive variant when the level provides one.

## Campaign completion

A campaign is complete when every expected level has reached a final state:

```text
PASS
WARN
FAIL
SKIP
BLOCKED
PARTIAL
ERROR
INFRA_ERROR
CONFIG_ERROR
```

A missing result is not a final result.
