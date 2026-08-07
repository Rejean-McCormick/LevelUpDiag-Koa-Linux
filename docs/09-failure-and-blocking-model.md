# Failure and blocking model

Distinguishing between failure types is a central LevelUpDiag function.

## PASS

The level executed correctly and its expected condition is satisfied.

## WARN

The level found a non-blocking anomaly or risk.

## FAIL

The level was able to execute its diagnostic and the target does not satisfy the expected condition.

Example:

```text
tests start correctly
but 3 tests fail
→ FAIL
```

## SKIP

The level was intentionally not executed.

It must have an explicit reason.

## BLOCKED

The level should be executed, but a prerequisite prevents obtaining a useful result.

Examples:

```text
required tool missing
required service unavailable
dependent level blocked
target checkout inaccessible
```

## PARTIAL

The level obtained some information, but not enough to produce a complete PASS or FAIL result.

## ERROR

The level or its internal code encountered an error.

Example:

```text
Python exception inside the level
→ ERROR
```

## INFRA_ERROR

The execution environment prevents the diagnostic from running.

Examples:

```text
process cannot be launched
infrastructure timeout
interpreter missing
system error
```

## CONFIG_ERROR

The diagnostic configuration is inconsistent or invalid.

Examples:

```text
invalid target_repo_root
unknown level
required command missing from config
malformed manifest
```

## Summary

```text
FAIL
= the target was tested and did not satisfy the test

BLOCKED
= the target could not be tested correctly

ERROR
= the check itself is broken

INFRA_ERROR
= the execution environment is broken

CONFIG_ERROR
= LevelUpDiag configuration is broken
```

## Aggregation

By default:

- a required `FAIL` makes the campaign fail;
- a required `CONFIG_ERROR` makes the campaign fail;
- a required `BLOCKED`, `PARTIAL`, or `INFRA_ERROR` makes the campaign blocked;
- a required `SKIP` makes the campaign incomplete unless an explicit rule says otherwise;
- `WARN` is not equivalent to `FAIL`.
