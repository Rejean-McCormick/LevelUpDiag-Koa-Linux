# kOA-Linux integration

## Principle

LevelUpDiag-Koali is external to kOA-Linux.

It knows the target checkout path through:

```text
target_repo_root
```

It then calls only public scripts or commands available in that checkout.

## One-way dependency

```text
LevelUpDiag-Koali
        ↓
public commands
        ↓
kOA-Linux
```

kOA-Linux does not depend on LevelUpDiag-Koali.

## Command examples

Depending on what exists in the targeted kOA-Linux version:

```text
python docs/tools/validate_docs.py
python ci/scripts/run-contracts.py
python ci/scripts/run-components.py
python ci/scripts/run-security.py
python ci/scripts/run-offline.py
python ci/scripts/run-system-tests.py
```

The manifest or configuration must reflect the commands actually available in the checkout being used.

## Read-only by default

Koali levels must treat the checkout as read-only.

Any operation that modifies the checkout must be separated from ordinary campaigns and clearly reported.

## Revision context

When a Git checkout is available, reports may record:

```text
branch
HEAD
working tree clean/dirty
```

This information is used for diagnosis and execution-context identification.

## Authority

If LevelUpDiag-Koali and kOA-Linux disagree on a rule, the kOA-Linux rule takes precedence.

LevelUpDiag-Koali is not responsible for automatically correcting that divergence.

## Missing public interface

When a level expects a command that does not exist in the target:

```text
BLOCKED
```

or:

```text
CONFIG_ERROR
```

depending on whether the problem comes from the target or configuration.

The issue must not be bypassed through an undocumented internal import.
