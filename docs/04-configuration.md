# Configuration

## Files

LevelUpDiag uses two complementary files:

```text
levelupdiag.config.example.json
levelupdiag.config.local.json
```

The `example` file is versioned and serves as a template.

The `local` file contains machine-specific values and must not contain information intended for the repository.

## Principles

- local paths belong in local configuration;
- secrets must not be versioned;
- all levels go through the shared loader;
- paths are resolved consistently;
- an unknown or invalid required field must produce a clear error.

## Koali target configuration

Configuration can be simplified compared with the generic web model:

```json
{
  "schema": "levelupdiag.koali.config.v1",
  "app_name": "kOA-Linux",
  "target_repo_root": "C:/mycode/kOA-Linux/koa-linux",
  "control_dir": ".levelupdiag",
  "artifacts_dir": ".levelupdiag/diagnostics",
  "toolchain": {
    "required": ["python"],
    "optional": ["git", "cargo"]
  },
  "commands": {
    "docs": "python docs/tools/validate_docs.py",
    "contracts": "python ci/scripts/run-contracts.py",
    "components": "python ci/scripts/run-components.py",
    "security": "python ci/scripts/run-security.py",
    "offline": "python ci/scripts/run-offline.py",
    "system": "python ci/scripts/run-system-tests.py"
  },
  "env": {}
}
```

Commands that do not exist in the target checkout must not be invented by configuration.

## target_repo_root

The kOA-Linux root is a central value.

The runner must verify that it exists before launching target-related checks.

## control_dir

The local control directory contains state produced by LevelUpDiag-Koali.

Recommended value:

```text
.levelupdiag
```

## artifacts_dir

Directory for diagnostics and logs.

Recommended value:

```text
.levelupdiag/diagnostics
```

## Toolchain

Tools are split into:

- `required`: absence blocks levels that depend on them;
- `optional`: used when available.

A level must report exactly which tool is missing.

## Commands

Commands are convenient aliases.

A level may use a shared alias instead of duplicating the same command across several files.

## Environment

`env` contains only variables explicitly required for execution.

Avoid copying or displaying the full environment.
