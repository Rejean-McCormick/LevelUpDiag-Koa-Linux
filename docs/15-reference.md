# Reference

## Main files

| File | Role |
|---|---|
| `levelupdiag_manifest.json` | Level inventory and ordering |
| `levelupdiag.config.example.json` | Versioned configuration template |
| `levelupdiag.config.local.json` | Local configuration |
| `levelupdiag_wrapper.pyw` | Graphical interface |
| `levelupdiag_wrapper_common.py` | Wrapper helpers |
| `scripts/run_level.py` | CLI launcher |
| `scripts/print_manifest.py` | Manifest display |
| `scripts/verify_repo.py` | Repository structural verification |
| `levelupdiag_core/` | Shared helpers |
| `.levelupdiag/` | Local state and diagnostics |

## Verdicts

| Verdict | Meaning |
|---|---|
| `PASS` | condition satisfied |
| `WARN` | non-blocking anomaly |
| `FAIL` | target tested, condition not satisfied |
| `SKIP` | execution intentionally omitted |
| `BLOCKED` | missing prerequisite |
| `PARTIAL` | incomplete result |
| `ERROR` | level error |
| `INFRA_ERROR` | environment problem |
| `CONFIG_ERROR` | invalid configuration |

## Main configuration fields

| Field | Role |
|---|---|
| `schema` | format version |
| `app_name` | target name |
| `target_repo_root` | target checkout root |
| `control_dir` | LevelUpDiag runtime state |
| `artifacts_dir` | logs and diagnostics |
| `toolchain` | required or optional tools |
| `commands` | shared commands |
| `env` | explicitly added variables |

## Level fields

Recommended base:

```text
id
name
file
enabled
required
depends_on
timeout_seconds
```

## Minimal result

```text
schema
standard
standard_version
level
name
verdict
findings
artifacts
```

## Recommended exit codes

For the runner:

| Code | Meaning |
|---|---|
| `0` | successful launch or result |
| `1` | negative level result |
| `2` | unknown argument or level |
| `3` | missing file or main prerequisite |
| `4` | invalid configuration |
| `5` | internal runner error |

Individual levels may use different internal codes if their JSON report provides the canonical verdict.

## Diagnostics layout

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

## Naming conventions

Levels:

```text
N00
N01
N02
```

Files:

```text
N01_environment.pyw
N04_contracts.py
```

Diagnostics directories:

```text
N01-environment/
N04-contracts/
```
