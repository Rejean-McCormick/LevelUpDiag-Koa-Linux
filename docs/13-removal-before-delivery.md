# Removal before delivery

## Rule

LevelUpDiag-Koali is a development and validation tool.

It must be removed before any kOA-Linux delivery.

## What must be absent

The final deliverable must not contain, in particular:

```text
LevelUpDiag-Koali/
levelupdiag_core/
.levelupdiag/
levelupdiag_manifest.json
levelupdiag.config.example.json
levelupdiag.config.local.json
levelupdiag_wrapper.pyw
levelupdiag_wrapper_common.py
START_LEVELUPDIAG.bat
launchers/
```

The actual check list must match the names used by the final repository.

## Dedicated level

The Koali manifest should contain a final level such as:

```text
delivery.appendix.absent
```

or an equivalent Nxx identifier.

Its only role is to verify that the appendix is absent from inspected artifacts.

## What the check inspects

Depending on delivery type:

- staging directory;
- archive;
- package contents;
- mounted or extracted image;
- deliverable file inventory.

## What may be retained outside delivery

LevelUpDiag reports may be kept separately as development diagnostics.

They must not be copied into the product merely because they were used to validate it.

## Recommended procedure

1. complete useful campaigns;
2. export required reports outside staging;
3. remove the appendix and `.levelupdiag/` from the build scope;
4. rebuild or clean staging;
5. run the absence check;
6. only then continue the delivery process.

## Failure

Any residual presence produces:

```text
FAIL
```

The remedy is to correct staging or packaging, then rerun the check.
