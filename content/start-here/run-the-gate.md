---
title: Run the gate
description: What scripts/check-stage.sh checks, what it prints when a stage passes or fails, and the SKIP_BUILD and STRICT switches.
type: how-to
---

# Run the gate

Each stage includes `scripts/check-stage.sh`, the gate. Run it from the
project root to check the source and, from stage 03, build the deliverables.
The script writes build output and logs to a temporary directory by default.

The gate runs three checks in order and prints `STAGE OK` when all of them
pass, or `STAGE FAILED` with failed checks marked `FAIL:`. A missing tool
causes an immediate exit with code `2`.

## What it checks

| Step | Command | Checks |
|---|---|---|
| 1 | `dogsbay-xml validate-project <root>` | Every `.dita` and `.ditamap` file validates against its DOCTYPE |
| 2 | `dogsbay-xml project-health <root>` | References, keys, reuse targets, metadata, and configured house rules for the default root map. Warnings alone can pass. |
| 3 | `dita --project=project.json --output=<tmp>` | Every deliverable in `project.json` builds. The log is scanned for `Error:` lines and DITA-OT `E` and `F` message codes, because the build does not always exit non-zero on a content error |

Step 2 uses the root map and Schematron configured in `.dogsbay/config.xml`
once later stages add them. Step 3 is skipped until the project has a
`project.json`, which arrives in stage 03.

Stage 15 adds controlled-value validation. Stage 23 adds a catalog fallback
for learning topics, and stage 25 enables a separate house-rules check.
See [The gate](/reference/the-gate) for those details and the limits of the
checks.

## Run it

```bash
scripts/check-stage.sh
```

On stage 01, before the map and `project.json` are created,
it prints:

```
== validate-project  /home/you/dogsbay-xml-dita-tutorial ==
1 file(s): 1 valid, 0 invalid.

== project-health  /home/you/dogsbay-xml-dita-tutorial ==
Root map: none (none configured); house rules: none (none configured)
Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

== build  skipped (no project.json yet) ==

STAGE OK
```

The file count grows with each stage. `Root map: none` is expected until
stage 02.

## When it fails

A validation error names the file, the line and column, and the element whose
content model was broken:

```
== validate-project  /home/you/dogsbay-xml-dita-tutorial ==
/home/you/dogsbay-xml-dita-tutorial/topics/trimming-audio.dita:
  22:170  error: The content of element type "cmd" does not match its content model.
5 file(s): 4 valid, 1 invalid.
FAIL: validation errors
```

The gate carries on to the health check so you see everything at once, then
ends with `STAGE FAILED`. Fix the file, run it again.

## The switches

```bash
SKIP_BUILD=1 scripts/check-stage.sh    # skip DITA-OT; a fast loop while authoring
STRICT=0 scripts/check-stage.sh        # project-health at --severity=error only
scripts/check-stage.sh /path/to/root   # check another project folder
```

`SKIP_BUILD=1` skips publishing from stage 03 onward. Build time depends on
the size of the publication and your system.
`STRICT=0` restricts the health report to errors. The default, `STRICT=1`,
also reports warnings; it does not make every warning a failure. Use the
default when checking a completed lesson and review the reported findings.

## Find the output

The build header identifies the temporary directory for that run. Paths
such as `/tmp/check-stage-357688` in recorded output are examples. Use the
directory printed by your own run.

When a lesson asks you to inspect `out/full/` in your project, first build
there with:

```bash
dita --project=project.json
```

Alternatively, inspect `out/full/` under the gate's temporary directory.
The same rule applies to the other deliverable directories. Command output
can vary with tool versions; compare the checks and results, not process IDs
or line numbers.

## Environment

| Variable | Default | Purpose |
|---|---|---|
| `DOGSBAY_XML` | `dogsbay-xml` on `PATH`, else `~/github/dogsbay-xml/bin/dogsbay-xml` | The DogsBay XML command line |
| `DITA_HOME` | `~/Downloads/dita-ot-4.3.5` | The DITA-OT installation; `DITA` overrides the `dita` executable directly |
| `OUT` | a temporary directory | Where step 3 writes the build |

Exit codes: `0` stage OK, `1` a gate failed, `2` a tool was not found.

## Where to go next

:::cards
- **[Stage 00: Set up the project](/part-1-topics/stage-00-setup)** {icon="play"}
  The empty project, file by file.

- **[The gate](/reference/the-gate)** {icon="book-open"}
  The full reference, including why `xmllint` is not used.
:::
