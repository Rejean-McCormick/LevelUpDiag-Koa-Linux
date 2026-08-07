# Contributing

Contributions to LevelUpDiag-Koali must preserve the simplicity of the system.

## General rules

A change must have one clear responsibility:

- add or fix a level;
- evolve the runner;
- improve log collection;
- improve configuration;
- improve the result model;
- improve the GUI or CLI;
- adapt to a public kOA-Linux command.

Avoid introducing a new concept when a manifest field, shared helper, or new level is sufficient.

## Adding a level

A new level must:

1. have a stable identifier;
2. appear in `levelupdiag_manifest.json`;
3. have a real executable file;
4. use the shared configuration;
5. write a normalized result;
6. return a consistent exit code;
7. never silently modify the target.

See [`docs/03-levels-and-checks.md`](docs/03-levels-and-checks.md).

## Modifying the core

Shared helpers belong in `levelupdiag_core/`.

A shared helper must remain generic across multiple levels. Logic specific to a single level stays in that level.

## Documentation

Any visible change to the manifest, configuration, verdicts, execution ordering, or logs must update the corresponding documentation.

## Expected tests

Before merge:

```text
configuration parsing
manifest parsing
level lookup
runner behavior
result serialization
log placement
failure mapping
CLI behavior
```

Checks that touch kOA-Linux must also be tested against a test checkout or controlled fixtures.
