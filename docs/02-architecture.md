# Architecture

## Overview

```text
levelupdiag_wrapper.pyw
        │
        ├──────────────┐
        │              │
        ▼              ▼
configuration      manifest
        │              │
        └──────┬───────┘
               ▼
             runner
               │
        ┌──────┼──────┐
        ▼      ▼      ▼
      N00    N01    N02 ...
        │      │      │
        └──────┼──────┘
               ▼
      results + logs + artifacts
```

## Components

### Configuration

Configuration contains machine-specific or target-checkout-specific values:

- target root;
- available tools;
- local commands;
- working paths;
- required environment variables;
- control directory;
- diagnostics directory.

### Manifest

The manifest is the level inventory.

It describes their identity, file, ordering, and the properties needed by the runner.

### Core

`levelupdiag_core/` contains shared functions:

```text
config
manifest
commands
runner
results
logs
artifacts
```

The core must remain small enough to understand without an additional framework.

### Levels

Levels are the actual units of work.

They may use `.pyw` when a graphical experience is useful or `.py` when console execution is preferable.

### Wrapper

The wrapper provides a visual interface to:

- display levels;
- launch a level;
- follow status;
- open reports or diagnostics directories.

The wrapper must not contain level business logic.

## Dependency direction

```text
wrapper
   ↓
core
   ↓
manifest/config
   ↓
levels
   ↓
public target commands
```

Levels may use the shared core.

The core must not depend on any specific level.

## Runtime data

Local outputs are grouped under `.levelupdiag/`.

Example:

```text
.levelupdiag/
├── diagnostics/
│   ├── N01-environment/
│   ├── N02-services/
│   └── ...
├── runs/
└── latest/
```

The exact organization may evolve, but the path must remain centralized and configurable.
