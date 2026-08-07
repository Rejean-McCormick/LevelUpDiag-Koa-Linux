# Overview

## Purpose

LevelUpDiag-Koali provides a consistent way to run a series of diagnostics and validations around a kOA-Linux checkout.

It mainly answers four questions:

1. which levels exist;
2. in what order they must run;
3. what the result of each one is;
4. where to find the produced logs and artifacts.

## Nature of the system

LevelUpDiag-Koali is a development appendix.

It can live in a workspace such as:

```text
workspace/
├── koa-linux/
└── LevelUpDiag-Koali/
```

It remains autonomous and can be deleted without preventing kOA-Linux from working.

## What it does

- loads configuration;
- discovers levels through a manifest;
- runs one level or a series of levels;
- handles simple prerequisites;
- applies timeouts;
- captures output;
- assigns a verdict;
- produces reports;
- displays state through the CLI or GUI.

## What it does not do

- it does not replace kOA-Linux tests;
- it does not define kOA-Linux rules;
- it does not create a new authority layer;
- it must not become a product dependency;
- it must not be required at runtime.

## Main concepts

### Level

An executable diagnostic or validation unit.

### Run

A concrete execution of a Level at a given point in time.

### Result

The normalized outcome of a Run.

### Finding

A useful observation produced by a level.

### Artifact

An auxiliary file produced by a level: log, report, capture, or command output.

### Campaign

A set of Runs executed together against the same target and context.

## Philosophy

The system favors:

- small levels;
- readable responsibilities;
- explicit results;
- easy-to-find logs;
- understandable orchestration;
- minimal magic.
