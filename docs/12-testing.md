# Testing

## Purpose

LevelUpDiag-Koali tests should primarily verify orchestration.

The system under test is not kOA-Linux itself: Koali checks call kOA-Linux validators.

## Unit tests

Cover at least:

```text
load_config
path resolution
load_manifest
normalize_level_id
get_level
level ordering
dependency resolution
verdict normalization
result serialization
artifact path helpers
```

## Runner tests

Essential cases:

1. PASS level;
2. FAIL level;
3. missing executable;
4. missing level file;
5. timeout;
6. level exception;
7. BLOCKED dependency;
8. invalid config;
9. large output;
10. multiple levels executed in order.

## Log tests

Verify:

- directory creation;
- separation between two runs;
- writing `result.json`;
- writing process output;
- `latest` directory behavior.

## Manifest tests

Verify:

- unique identifiers;
- existing files;
- existing dependencies;
- no simple cycle;
- stable ordering.

## GUI tests

The GUI should mainly be tested for:

- manifest loading;
- level display;
- correct launch behavior;
- opening the correct directory;
- reporting the correct verdict.

Execution logic must remain testable without a graphical interface.

## kOA integration tests

Use a known checkout or fixture.

Test at least:

- missing target;
- missing command;
- PASS command;
- FAIL command;
- command BLOCKED by a missing tool.

## Removal test

Build a fake delivery containing a LevelUpDiag file and verify that the control level detects it.

Then build a clean delivery and verify that it passes.
