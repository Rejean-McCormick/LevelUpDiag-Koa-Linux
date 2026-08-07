# Levels and checks

## Definition

A Level is an autonomous diagnostic or validation unit.

A good level answers one precise question, for example:

```text
Is the minimum environment available?
Do the documentation validation scripts pass?
Can components run their tests?
Are required integrations available?
Does the delivery still contain the appendix?
```

## Identity

Historical LevelUpDiag identifiers use the form:

```text
N00
N01
N02
...
```

This convention can be retained to stay simple and compatible with launchers.

The number represents the primary order, not an authority hierarchy.

## Minimal contract

Each manifest entry should contain at least:

```json
{
  "id": "N03",
  "name": "Static Integrity",
  "file": "levels/N03_static_integrity.pyw",
  "enabled": true,
  "required": true,
  "depends_on": ["N01"],
  "timeout_seconds": 180
}
```

Optional fields must remain justified by a real need.

## Level responsibilities

A level must:

1. load shared configuration;
2. validate its own prerequisites;
3. execute its diagnostic;
4. produce useful findings;
5. write a normalized result;
6. return a consistent exit code.

## Standalone level

A level should be directly executable when that helps diagnosis:

```text
python levels/N03_static_integrity.py
```

or, for a graphical level:

```text
pythonw levels/N03_static_integrity.pyw
```

The runner remains the normal path for aligned execution.

## Simple dependencies

Use `depends_on` when a level cannot provide a useful result without another level.

Example:

```text
N01 Environment
   ↓
N03 Static Integrity
   ↓
N04 Contracts
```

Do not create a dependency only to express a display preference.

## Required vs optional

`required: true` means the level is part of the expected campaign result.

`required: false` means it may be absent or non-applicable without preventing the campaign from completing.

## Placeholders

A manifest entry without a real file is incomplete.

The runner must report it clearly and never present it as a successful test.
