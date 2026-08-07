# CLI and GUI

LevelUpDiag-Koali has two surfaces: CLI scripts and a graphical wrapper.

## CLI

The main historical script is:

```text
scripts/run_level.py
```

It can list or launch a level.

Examples:

```text
py scripts/run_level.py --list
py scripts/run_level.py N04
py scripts/run_level.py 4 --wait
py scripts/run_level.py N04 --windowed
```

## Recommended Koali evolution

Keep these usages and progressively add:

```text
py scripts/run_level.py --all
py scripts/run_level.py --campaign merge-validation
py scripts/run_level.py --from N03 --to N06
```

There is no need to build a large CLI while these commands cover the need.

## print_manifest.py

This script provides a quick view of the manifest.

It should remain a simple diagnostic tool.

## verify_repo.py

This script verifies the LevelUpDiag-Koali repository structure:

- main files are present;
- manifest is readable;
- referenced levels are present;
- Python modules are compilable;
- example configuration is readable.

It verifies the functional consistency of the repository, not kOA-Linux content.

## GUI

The `.pyw` wrapper should allow users to:

- view the ordered list;
- view each level status;
- launch a level;
- launch a campaign;
- open logs;
- display the latest result.

## Authority

The GUI does not own verdict logic that differs from the CLI.

Both go through the same shared functions.

## Windowed mode

A `.pyw` file can be launched through windowed Python.

A level intended for automation should avoid depending on mandatory GUI interaction.
