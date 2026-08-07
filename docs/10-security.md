# Security

This document complements the root [`SECURITY.md`](../SECURITY.md) with implementation rules.

## Command execution

The runner should prefer:

```python
subprocess.run(
    ["python", "script.py"],
    shell=False,
)
```

over executing a string through a shell.

A string-form command should be reserved for cases where a shell is genuinely required and the command source is controlled.

## Working directory

Every Run must explicitly know its `cwd`.

A kOA-Linux check normally uses:

```text
cwd = target_repo_root
```

## Timeout

Every external command must have a reasonable timeout.

A timeout must never produce PASS.

## Paths

Write functions must restrict destinations to:

- `control_dir`;
- `artifacts_dir`;
- another explicitly authorized directory.

Values containing `..` or resolving outside the expected root must be reviewed before writing.

## Sensitive logs

Before persistence, levels must avoid or mask values such as:

```text
password=
secret=
api_key=
token=
authorization:
```

This list may be adapted through configuration.

## Environment variables

Do not write the full environment to a log.

Add only required variables to the subprocess environment in addition to the inherited environment.

## kOA-Linux

The kOA-Linux checkout is treated as read-only during a normal campaign.

Levels must not:

- apply patches;
- modify manifests;
- create commits;
- automatically clean the worktree.

## External artifacts

When a level inspects an archive or delivery directory, it must avoid executing its contents.
