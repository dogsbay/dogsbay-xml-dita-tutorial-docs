---
title: Run the gate
description: What scripts/check-stage.sh checks, what it prints when a stage passes or fails, and the SKIP_BUILD and STRICT switches.
type: how-to
---

# Run the gate

Every stage branch carries `scripts/check-stage.sh`, the gate. It is the one
command that decides whether a stage is done, and every stage page ends by
running it. Run it from the project root at any time; it never changes your
files.

The gate runs three checks in order and prints `STAGE OK` when all of them
pass, or `STAGE FAILED` with the first failure marked `FAIL:`.

## What it checks

| Step | Command | Proves |
|---|---|---|
| 1 | `dogsbay-xml validate-project <root>` | Every `.dita` and `.ditamap` file validates against its DOCTYPE |
| 2 | `dogsbay-xml project-health <root>` | No broken links, undefined or unused keys, broken element ids, orphan conref pushes or dangling index redirects; no metadata-policy or house-rule violations; formatting and authoring leftovers are reported as warnings |
| 3 | `dita --project=project.json --output=<tmp>` | Every deliverable in `project.json` builds. The log is scanned for `Error:` lines and DITA-OT `E` and `F` message codes, because the build does not always exit non-zero on a content error |

Step 2 uses the root map and Schematron configured in `.dogsbay/config.xml`
once later stages add them. Step 3 is skipped until the project has a
`project.json`, which arrives in Part 2.

## Run it

```bash
scripts/check-stage.sh
```

On a passing stage in Part 1, where there is no map and no `project.json` yet,
it prints:

```
== validate-project  /home/you/dogsbay-xml-dita-tutorial ==
9 file(s): 9 valid, 0 invalid.

== project-health  /home/you/dogsbay-xml-dita-tutorial ==
Root map: none (none configured); house rules: none (none configured)
Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

== build  skipped (no project.json yet) ==

STAGE OK
```

The file count grows with each stage. `Root map: none` is expected until
stage 07.

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

`SKIP_BUILD=1` matters from Part 2 on, when the build takes about ten seconds.
`STRICT=0` lets warnings such as house-style formatting through; the branches
themselves are checked with `STRICT=1`, the default, so keep it on before you
compare your work with a branch.

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
