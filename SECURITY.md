# Security

LevelUpDiag-Koali runs external processes and handles local paths. Its main security surface is therefore command execution and output collection.

## Principles

- never place secrets in `levelupdiag.config.example.json`;
- limit commands to those declared in the manifest or configuration;
- prefer commands represented as argument lists;
- avoid shell interpretation when it is not required;
- validate paths before reading or writing;
- never write outside explicitly configured directories;
- treat kOA-Linux as read-only by default;
- filter sensitive values before writing logs;
- never convert an infrastructure error into PASS.

## Sensitive data

Logs may contain:

- local paths;
- process output;
- local user names;
- environment variables;
- information about installed tools.

Levels must not copy the full environment into their reports.

Values that look like secrets, tokens, passwords, or keys must be masked before persistence.

## Commands

Commands loaded from a local configuration file are treated as limited-trust inputs.

The runner must:

1. know the command that is actually executed;
2. record its working directory;
3. apply a timeout;
4. capture the exit code;
5. distinguish timeout, missing executable, and target failure.

## Reporting

A vulnerability involving command execution, path escape, secret leakage, or unexpected writes to the target must be treated as high priority.
