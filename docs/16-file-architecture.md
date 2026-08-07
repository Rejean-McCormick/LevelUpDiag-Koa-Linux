# File Architecture

## Status

This document defines the file architecture for the clean reconstruction of **LevelUpDiag-Koali**.

It is normative for the repository structure.

It complements the other documents without redefining their functional behavior. If there is any doubt about a file location or the creation of a new file, this document takes precedence over older examples.

The architecture is frozen **by documentation**: no lockfile, hash, or fingerprint-validation mechanism is used.

## Main rule

A versioned source file must be created only if it belongs to the tree defined here.

A new file, root directory, or unplanned subsystem requires an explicit decision and an update to this document before composition.

Runtime files under `.levelupdiag/` are generated locally and are not part of the versioned source architecture.

## Final tree

```text
LevelUpDiag-Koali/
├── .gitignore
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── SECURITY.md
├── pyproject.toml
│
├── levelupdiag.config.example.json
├── levelupdiag_manifest.json
│
├── levelupdiag_wrapper.pyw
├── levelupdiag_wrapper_common.py
├── START_LEVELUPDIAG.bat
│
├── levelupdiag_core/
│   ├── __init__.py
│   ├── config.py
│   ├── manifest.py
│   ├── models.py
│   ├── planner.py
│   ├── commands.py
│   ├── runner.py
│   ├── verdicts.py
│   ├── reports.py
│   ├── logs.py
│   └── artifacts.py
│
├── levels/
│   ├── N00_control_panel.pyw
│   ├── N01_environment.pyw
│   ├── N02_repository.pyw
│   ├── N03_documentation.pyw
│   ├── N04_contracts.pyw
│   ├── N05_components.pyw
│   ├── N06_integrations.pyw
│   ├── N07_profiles.pyw
│   ├── N08_security.pyw
│   ├── N09_offline.pyw
│   ├── N10_system.pyw
│   └── N11_delivery.pyw
│
├── launchers/
│   ├── N00-control-panel.bat
│   ├── N01-environment.bat
│   ├── N02-repository.bat
│   ├── N03-documentation.bat
│   ├── N04-contracts.bat
│   ├── N05-components.bat
│   ├── N06-integrations.bat
│   ├── N07-profiles.bat
│   ├── N08-security.bat
│   ├── N09-offline.bat
│   ├── N10-system.bat
│   ├── N11-delivery.bat
│   └── run_level.bat
│
├── scripts/
│   ├── print_manifest.py
│   ├── run_level.py
│   └── verify_repo.py
│
├── schemas/
│   └── levelupdiag.result.schema.json
│
├── tests/
│   ├── test_config.py
│   ├── test_manifest.py
│   ├── test_planner.py
│   ├── test_commands.py
│   ├── test_runner.py
│   ├── test_reports.py
│   ├── test_logs.py
│   └── test_delivery_level.py
│
└── docs/
    ├── README.md
    ├── 01-overview.md
    ├── 02-architecture.md
    ├── 03-levels-and-checks.md
    ├── 04-configuration.md
    ├── 05-execution-and-ordering.md
    ├── 06-results-and-logs.md
    ├── 07-koa-linux-integration.md
    ├── 08-campaigns.md
    ├── 09-failure-and-blocking-model.md
    ├── 10-security.md
    ├── 11-cli-and-gui.md
    ├── 12-testing.md
    ├── 13-removal-before-delivery.md
    ├── 14-development.md
    ├── 15-reference.md
    ├── 16-file-architecture.md
    └── AI_COMPOSER_CONTRACT.json
```

## Repository root

### `.gitignore`

Contains only exclusions required by the repository and local outputs.

It must exclude at least:

```text
__pycache__/
*.py[cod]
.venv/
.pytest_cache/
.levelupdiag/
*.log
*.tmp
```

### `README.md`

Human entry point.

It does not contain the detailed specification of the internal architecture.

### `CHANGELOG.md`

History of functional, structural, and documentation changes.

### `CONTRIBUTING.md`

Contribution rules and the minimal definition of an acceptable change.

### `SECURITY.md`

Security rules specific to the appendix.

### `pyproject.toml`

Declares the Python project, supported Python version, and any development dependencies.

Core runtime must remain based on the Python standard library unless an explicit decision says otherwise.

### `levelupdiag.config.example.json`

Versioned example configuration.

No machine-specific value or real secret.

### `levelupdiag_manifest.json`

Canonical level inventory.

It is the single versioned source for:

- level identifier;
- name;
- file;
- enabled state;
- required or optional status;
- dependencies;
- timeout;
- metadata required by the runner.

Levels must not be discovered implicitly by scanning the directory.

### `levelupdiag_wrapper.pyw`

Main Tkinter application.

It provides the view and delegates logic to the core.

### `levelupdiag_wrapper_common.py`

Helpers exclusively related to the wrapper and graphical launching.

Any logic reusable by the CLI belongs in the core, not in this file.

### `START_LEVELUPDIAG.bat`

Convenient Windows entry point to the main wrapper.

It contains no validation logic.

## `levelupdiag_core/`

The core is closed to **11 modules** in the target version.

No additional subpackage is planned.

### `__init__.py`

Stable public core exports.

### `config.py`

Owns:

- configuration loading;
- example/local merge;
- path resolution;
- explicitly added environment values.

It runs no checks.

### `manifest.py`

Owns:

- manifest reading;
- `LevelInfo` or equivalent construction;
- identifier normalization;
- level lookup.

It does not plan execution.

### `models.py`

Contains only shared data structures:

- `Finding`;
- `Artifact`;
- `StepResult`;
- `LevelResult`;
- `CampaignResult`.

No I/O, subprocess, or Tkinter.

### `planner.py`

Owns:

- selection;
- ordering;
- dependencies;
- cycle detection;
- dependency blocking.

The plan remains deterministic and sequential by default.

### `commands.py`

Owns external process execution.

It captures:

- command;
- cwd;
- exit code;
- timestamps;
- duration;
- useful output;
- timeout or launch error.

The normal execution path does not use `shell=True`.

### `runner.py`

Main orchestrator.

It:

- uses `planner.py`;
- runs levels;
- passes configuration;
- applies dependencies;
- aggregates a campaign;
- delegates logs and reports to their owning modules.

It contains no logic specific to N03, N04, N05, and so on.

### `verdicts.py`

Single source of verdicts:

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

Contains only constants and aggregation/normalization helpers.

### `reports.py`

Reads and writes structured results.

It does not execute commands.

### `logs.py`

Organizes and writes local logs.

It writes only within configured runtime directories.

### `artifacts.py`

Helpers for artifact paths, safe names, and local opening of directories/files.

It does not interpret artifact content as code.

## `levels/`

The directory contains exactly the levels defined in the target taxonomy.

Each file is thin and specific to its responsibility.

A level:

- loads configuration;
- verifies its local prerequisites;
- calls core helpers;
- launches a public command when needed;
- produces a `LevelResult`;
- never directly launches another level.

### Frozen taxonomy

| ID | File | Responsibility |
|---|---|---|
| N00 | `N00_control_panel.pyw` | lightweight self-check / appendix control |
| N01 | `N01_environment.pyw` | environment, Python, paths, and tools |
| N02 | `N02_repository.pyw` | target checkout and observable repository state |
| N03 | `N03_documentation.pyw` | public documentation validation |
| N04 | `N04_contracts.pyw` | contract validation |
| N05 | `N05_components.pyw` | component validations |
| N06 | `N06_integrations.pyw` | available integration validations |
| N07 | `N07_profiles.pyw` | available profile validations |
| N08 | `N08_security.pyw` | public security validations |
| N09 | `N09_offline.pyw` | offline validations |
| N10 | `N10_system.pyw` | system validations |
| N11 | `N11_delivery.pyw` | appendix absence in delivery |

This taxonomy must not be expanded, reduced, or renumbered without an explicit architecture change.

## `launchers/`

Launchers are extremely thin Windows wrappers.

Each launcher calls `scripts/run_level.py` with the corresponding identifier.

Conceptual example:

```bat
@echo off
py "%~dp0..\scripts\run_level.py" N04 --windowed
```

They contain no check, configuration, or verdict logic.

`run_level.bat` provides generic access to the CLI launcher.

## `scripts/`

### `print_manifest.py`

Displays the manifest in readable form.

No validation logic.

### `run_level.py`

Canonical CLI entry point to:

- list levels;
- run one level;
- run all enabled levels;
- run a simple campaign.

The GUI and launchers must converge on the same core functions.

### `verify_repo.py`

Verifies the structural consistency of **LevelUpDiag-Koali itself**:

- required files;
- readable JSON;
- coherent manifest;
- level files present;
- Python imports/compilation;
- valid level dependencies.

It does not become a general kOA-Linux validator.

## `schemas/`

The target version contains a single schema:

```text
levelupdiag.result.schema.json
```

It defines a level report.

No registry or complex schema system is planned.

## `tests/`

The suite remains flat and directly mapped to the core.

| Test | Covers |
|---|---|
| `test_config.py` | configuration |
| `test_manifest.py` | manifest and LevelInfo |
| `test_planner.py` | ordering and dependencies |
| `test_commands.py` | subprocess, timeout, errors |
| `test_runner.py` | orchestration and campaigns |
| `test_reports.py` | serialization |
| `test_logs.py` | log layout and writes |
| `test_delivery_level.py` | N11 and residue detection |

Do not create a framework-specific test tree while these eight files remain sufficient.

## `docs/`

Documentation is adjacent to code and must not create a second governance hierarchy.

`AI_COMPOSER_CONTRACT.json` is the machine-readable complement intended for AI composition tools.

`16-file-architecture.md` is the canonical human reference for paths.

## Unversioned local runtime

Local runtime is limited to:

```text
.levelupdiag/
├── diagnostics/
│   └── <level>/
│       └── <run>/
│           ├── result.json
│           └── output.log
├── runs/
│   └── <campaign>/
│       ├── summary.json
│       └── summary.txt
└── latest/
```

Internal temporary files may exist during a write, but they do not become public formats.

`.levelupdiag/`:

- is ignored by Git;
- is not a source of authority;
- may be deleted without affecting code;
- is never included in a kOA-Linux delivery.

## Directories forbidden without an explicit decision

Do not create spontaneously:

```text
src/
app/
api/
server/
services/
plugins/
workers/
agents/
database/
migrations/
state/
cache/
vendor/
generated/
dist/
build/
packages/
policies/
catalogs/
adapters/
workflows/
```

This prohibition concerns the LevelUpDiag-Koali source architecture. An external tool may of course temporarily produce `build/` or `dist/`, but those directories do not become design components without an explicit decision.

## Files and concepts explicitly outside the target architecture

The clean reconstruction does not carry forward these historical elements:

```text
.smartignore
GitSink.bat
levelupdiag_core/http.py
```

The old frontend/backend/Playwright/UX-oriented taxonomies are not carried forward.

No hash-validation file, fingerprint inventory, or architecture lockfile must be added.

## Responsibility ownership

Each responsibility has one owner:

| Responsibility | Owner |
|---|---|
| configuration | `levelupdiag_core/config.py` |
| level inventory | `levelupdiag_manifest.json` + `manifest.py` |
| models | `models.py` |
| ordering/dependencies | `planner.py` |
| subprocess | `commands.py` |
| orchestration | `runner.py` |
| verdicts | `verdicts.py` |
| serialization | `reports.py` |
| logs | `logs.py` |
| artifacts | `artifacts.py` |
| graphical interface | `levelupdiag_wrapper*.py*` |
| CLI | `scripts/run_level.py` |
| appendix repository verification | `scripts/verify_repo.py` |
| check-specific logic | `levels/Nxx_*.pyw` |

Duplicating a responsibility across two owners is architectural drift.

## Internal dependency rules

Allowed dependencies:

```text
wrapper ───────→ core
scripts ───────→ core
levels ────────→ core
runner ────────→ planner
runner ────────→ commands
runner ────────→ reports
runner ────────→ logs
core modules ──→ models/verdicts/config as needed
```

Forbidden dependencies:

```text
core → wrapper
core → specific level
level → another level
kOA-Linux → LevelUpDiag-Koali
reports → subprocess
models → I/O
verdicts → I/O
```

## File creation rule

An AI system or developer must not solve a problem by creating a new module by default.

Decision order:

1. does the responsibility belong to an existing file?
2. can it be added without breaking that file's cohesion?
3. is it logic specific to a level?
4. is it genuinely a new durable responsibility?

Only the fourth case may justify an architecture evolution.

Before creation, the change must update:

```text
docs/16-file-architecture.md
docs/AI_COMPOSER_CONTRACT.json
docs/README.md if documentation navigation changes
```

## Rename rule

The following paths are stable repository interfaces:

```text
levelupdiag_manifest.json
levelupdiag.config.example.json
levelupdiag_wrapper.pyw
levelupdiag_wrapper_common.py
START_LEVELUPDIAG.bat
levelupdiag_core/
levels/
launchers/
scripts/
schemas/
tests/
docs/
```

They must not be renamed during an ordinary reconstruction or refactor.

## Deletion rule

A file in the final tree may be deleted only if:

1. its responsibility genuinely disappears;
2. no other documentation requires it;
3. the manifest and imports are adapted;
4. this document is modified in the same change.

## Structural compliance criterion

A clean reconstruction is structurally compliant when:

- every required file in the final tree exists;
- no explicitly removed historical file is carried forward;
- no unplanned new subsystem has been added;
- every declared level has exactly its expected file;
- responsibilities remain in their owning modules;
- `.levelupdiag/` remains local and unversioned;
- LevelUpDiag-Koali remains independent of kOA-Linux runtime code.

Structural compliance relies on documented paths and contracts, not on file fingerprints.
