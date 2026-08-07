# LevelUpDiag-Koali

LevelUpDiag-Koali is a standalone diagnostic and validation appendix used alongside kOA-Linux during development.

Its role is simple:

1. load a local configuration;
2. read a level manifest;
3. run a series of `.py`, `.pyw`, or command-based checks;
4. respect their order and simple dependencies;
5. collect outputs and logs;
6. normalize verdicts;
7. provide a coherent view of the target state.

LevelUpDiag-Koali is not part of kOA-Linux. It is not required at runtime and must be removed before preparing a delivery.

## Principles

- the manifest describes the levels;
- each level remains independently executable;
- the runner orchestrates without reimplementing check logic;
- local paths and commands come from configuration;
- results clearly distinguish a target failure from an environment problem;
- logs are grouped by execution;
- the GUI is a convenience facade, not an authority;
- kOA-Linux remains the observed target and keeps its own rules.

## Documentation structure

Detailed documentation starts in [`docs/README.md`](docs/README.md).

The main documents are:

- [`docs/01-overview.md`](docs/01-overview.md)
- [`docs/02-architecture.md`](docs/02-architecture.md)
- [`docs/03-levels-and-checks.md`](docs/03-levels-and-checks.md)
- [`docs/04-configuration.md`](docs/04-configuration.md)
- [`docs/05-execution-and-ordering.md`](docs/05-execution-and-ordering.md)
- [`docs/06-results-and-logs.md`](docs/06-results-and-logs.md)
- [`docs/07-koa-linux-integration.md`](docs/07-koa-linux-integration.md)
- [`docs/08-campaigns.md`](docs/08-campaigns.md)
- [`docs/09-failure-and-blocking-model.md`](docs/09-failure-and-blocking-model.md)
- [`docs/10-security.md`](docs/10-security.md)
- [`docs/11-cli-and-gui.md`](docs/11-cli-and-gui.md)
- [`docs/12-testing.md`](docs/12-testing.md)
- [`docs/13-removal-before-delivery.md`](docs/13-removal-before-delivery.md)
- [`docs/14-development.md`](docs/14-development.md)
- [`docs/15-reference.md`](docs/15-reference.md)

## Mental model

```text
local config
     +
manifest
     ↓
runner
     ↓
N00 → N01 → N02 → ...
     ↓
results
     ↓
logs + global report
```

A level may itself call a public kOA-Linux script, `pytest`, `cargo`, a system tool, or another authorized command.

## Runtime directories

By default, locally produced files are stored under:

```text
.levelupdiag/
```

This directory contains execution state, logs, and local reports. It is not intended to be shipped with kOA-Linux.

## Project status

LevelUpDiag-Koali is a specialized adaptation of LevelUpDiag designed to support kOA-Linux development without merging the two repositories.
