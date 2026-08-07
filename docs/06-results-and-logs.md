# Results and logs

## Purpose

Each level must produce a result that can be understood without rereading its entire console output.

The system still keeps detailed logs for diagnosis.

## Verdicts

LevelUpDiag verdicts are:

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

Their detailed meaning is described in [09-failure-and-blocking-model.md](09-failure-and-blocking-model.md).

## Minimal level report

```json
{
  "schema": "levelupdiag.report.v1",
  "standard": "LevelUpDiag",
  "standard_version": "1.0",
  "level": "N04",
  "name": "Contracts",
  "verdict": "PASS",
  "findings": [],
  "artifacts": []
}
```

## Recommended Koali extension

Without making the schema heavy, a result may add:

```json
{
  "started_at": "2026-08-07T10:00:00-04:00",
  "ended_at": "2026-08-07T10:00:12-04:00",
  "duration_seconds": 12.0,
  "exit_code": 0,
  "command": "python ci/scripts/run-contracts.py",
  "cwd": "C:/mycode/kOA-Linux/koa-linux",
  "output_tail": "..."
}
```

## Findings

A finding is a structured observation.

Example:

```json
{
  "severity": "FAIL",
  "code": "COMMAND_FAILED",
  "message": "The contracts runner returned a non-zero exit code.",
  "path": "ci/scripts/run-contracts.py"
}
```

A finding should explain:

- what happened;
- where;
- why it matters;
- what to check next when relevant.

## Logs

Logs must be grouped by level and by run.

Example:

```text
.levelupdiag/
└── diagnostics/
    └── N04-contracts/
        └── 20260807_100000/
            ├── result.json
            ├── output.log
            └── notes.txt
```

## output.log

`output.log` contains the complete, or sufficiently complete, output of the external process.

The main report may keep only an `output_tail` to remain readable.

## latest

A convenience projection may point to the latest result of each level:

```text
.levelupdiag/latest/N04/result.json
```

It does not replace run history.

## Campaign report

A campaign may write:

```text
.levelupdiag/runs/<run-id>/summary.json
.levelupdiag/runs/<run-id>/summary.txt
```

The summary contains:

- target;
- start and end time;
- expected levels;
- verdict for each level;
- counts of PASS/WARN/FAIL/BLOCKED/etc.;
- paths to logs.

## Principle

Logs tell the details.

The result tells the conclusion.
