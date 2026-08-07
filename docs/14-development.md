# Development

## Purpose

Evolve LevelUpDiag-Koali without turning it into a general-purpose framework.

## Core organization

Compact target structure:

```text
levelupdiag_core/
├── __init__.py
├── config.py
├── manifest.py
├── models.py
├── planner.py
├── runner.py
├── commands.py
├── results.py
├── logs.py
└── artifacts.py
```

These files should be created only when they provide a real separation of responsibility.

## Adding a new level

1. choose its identifier;
2. define its responsibility;
3. create its file;
4. add the manifest entry;
5. use `load_config()`;
6. use shared helpers;
7. write a result;
8. test PASS, FAIL, and missing prerequisite;
9. document the level if its behavior is not obvious.

## Adding a dependency

Add `depends_on` to the manifest.

Avoid encoding dependencies through direct calls between level files.

## Adding a kOA command

The command must:

- exist in the target checkout;
- be usable from the target root;
- have non-interactive behavior for automated campaigns;
- produce a usable exit code.

## Modifying a verdict

Verdicts are a shared interface.

A new value must have a genuinely distinct meaning and be understood by:

```text
runner
reports
wrapper
tests
documentation
```

## Adding result fields

Prefer simple, useful fields.

Reasonable examples:

```text
started_at
ended_at
duration_seconds
exit_code
command
cwd
output_tail
```

Avoid deep structures when levels do not need them.

## Compatibility

A manifest or report change must either remain compatible with older entries or explicitly change the schema identifier.

## Definition of done

A change is complete when:

- it works through the CLI;
- it works through the wrapper when applicable;
- errors are classified correctly;
- logs are readable;
- associated tests pass;
- documentation matches actual behavior.
