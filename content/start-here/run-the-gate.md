---
title: Check your work
description: Check a stage with Project > Check Project in the editor or dogsbay-xml check on the command line, read the result, and find the details when it fails.
type: how-to
---

# Check your work

Check your project at the end of every lesson. The check tells you whether
the project is ready to publish: whether its source is sound, whether every
deliverable builds, and whether the built output holds together.

- In the editor, choose **Project** > **Check Project**. The result appears
  in the **Project Validation** panel.
- From the command line, run `dogsbay-xml check .` from the project root.

Both run the same check. You do not need to install DITA-OT: the editor and
the command line include DITA-OT 4.3.5 and build with it.

## What it checks

The check runs three stages in order, and each runs only when the one before
it passed:

| Stage | Checks |
|---|---|
| Health | Every topic and map is valid against its DOCTYPE, and references, keys, reuse targets, metadata, and any configured house rules are sound |
| Build | Every deliverable in `project.json` builds with DITA-OT |
| Output | Every link, image, and fragment in the built output leads somewhere |

It stops at the first stage that fails, because later stages would only
report consequences of the same fault. A broken reference in the source, for
example, produces a broken link in the output.

Health uses the root map and house rules set in the project settings once
later stages add them.

## Before stage 03: no deliverables yet

Deliverables arrive in stage 03. Until then there is nothing to build, and
the check reports that it stopped at the build stage:

```
health   clean
build    nothing to build
Not ready: stopped at build — this project declares no deliverables, so there is nothing to build or check.
```

In stages 00 to 02, `health   clean` is the result to look for. From the
command line, add `--no-build` to check the source only:

```bash
dogsbay-xml check --no-build .
```

```
health   clean
Project health is clean. The build was not run, so nothing here speaks for the output.
```

## A ready project

From stage 03, a finished lesson checks as ready. For example, on stage 03:

```
health   clean
build    full                 ok  /home/you/audacity-guide/out/full
output   clean (full)
Ready: the project is healthy, every deliverable built, and the output of full holds together.
```

Later stages list one `build` line for each deliverable. Paths in recorded
output are examples.

## When it fails

When health fails, the check names the files with problems:

```
health   NOT CLEAN
  invalid: /home/you/audacity-guide/topics/what-is-audacity.dita
  (run project-health for the full report)
Not ready: stopped at health — the project itself has faults, so nothing was built and no output was read.
```

For the line, column, and message of each problem, open the
**Project Validation** panel in the editor, or run:

```bash
dogsbay-xml project-health .
```

When the output check fails, it lists each link that leads nowhere, with the
page and line that contains it.

Fix the problem and check again. After a lesson's deliberate-error
exercise, undo the change and check again before you continue.

## Find the output

Each deliverable builds into its own folder, as `project.json` declares it:
`out/full/` for the full guide, `out/beginner-mac/` for the macOS beginner
guide, and so on. The `out/` folder is ignored by Git.

To build and check one deliverable only:

```bash
dogsbay-xml check --deliverable=full .
```

## For maintainers

Each stage branch also carries `scripts/check-stage.sh`, the script used to
verify the branches. Readers do not need it. See
[The stage gate](/reference/the-gate).

## Where to go next

:::cards
- **[Stage 00: Set up the project](/part-1-topics/stage-00-setup)** {icon="play"}
  The empty project, file by file.

- **[The stage gate](/reference/the-gate)** {icon="book-open"}
  The maintainers' script, and why `xmllint` is not used.
:::
