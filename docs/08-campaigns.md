# Campaigns

## Definition

A Campaign is simply a group of levels executed together.

It does not create a new rules layer.

It answers:

```text
which target?
which levels?
in what order?
when?
with what final result?
```

## Example

```json
{
  "name": "merge-validation",
  "target": "kOA-Linux",
  "levels": ["N01", "N03", "N04", "N05"],
  "stop_on_config_error": true
}
```

## Useful types

### developer-fast

Fast feedback during development.

Runs only a few short levels.

### bundle-validation

Validates one area or work bundle.

### merge-validation

Runs validations required before merge.

### nightly

Runs a broader series when execution time is less critical.

### release-preparation

Groups validations relevant before preparing a delivery.

### delivery-check

Specifically verifies that the appendix is not present in the deliverable.

## Global result

A campaign should not reduce every situation to a boolean.

Recommended summary:

```json
{
  "campaign": "merge-validation",
  "status": "BLOCKED",
  "counts": {
    "PASS": 4,
    "WARN": 0,
    "FAIL": 0,
    "BLOCKED": 1
  }
}
```

## Completeness

The campaign must know the list of levels it was expected to run.

A required level without a final result makes the campaign incomplete.

## History

Each campaign receives a run identifier or timestamp so its logs remain separate from previous campaigns.
