---
title: Check your work
description: Check a stage with Project > Check Project in the editor or dogsbay-xml check on the command line, read the result, and find the details when it fails.
type: how-to
---

# Check your work

Check your project at the end of every lesson. The check tells you whether
the project is ready to publish: whether its source is sound, whether every
deliverable builds, and whether every link in the built pages leads
somewhere.

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
| Build | Every deliverable in **Manage Deliverables** builds with DITA-OT |
| Output | Every link, image, and fragment in the built output leads somewhere |

It stops at the first stage that fails, because later stages would only
report consequences of the same fault. A broken reference in the source, for
example, produces a broken link in the output.

Health uses the root map and house rules set in the project settings once
later stages add them. Before stage 02, the project has no root map, so
every topic that no map refers to is reported as an orphan.

## Before stage 03: no deliverables yet

Deliverables arrive in stage 03. Until then there is nothing to build, so
the check reports on the source only. For example, on stage 02:

```
health   clean
Project health is clean — no deliverables yet, so nothing here speaks for the output.
```

In stages 00 to 02, this is the result to look for. A warning, such as the
orphan topic in stage 01, is listed under `health` and does not make the
check fail. For example, on stage 01:

```
health   clean, with warnings
  orphan topic: /home/you/my-audacity-guide/topics/what-is-audacity.dita — nothing refers to it, so it will not appear in the output
Project health is clean — no deliverables yet, so nothing here speaks for the output. 1 orphan topic above: worth knowing, and not treated as failures.
```

## A ready project

From stage 03, a finished lesson checks as ready. For example, on stage 03:

```
health   clean
build    full                 ok  /home/you/my-audacity-guide/out/full
output   clean (full)
Ready: the project is healthy, every deliverable built, and every link in the pages of full leads somewhere.
```

Later stages list one `build` line for each deliverable. Paths in recorded
output are examples.

A PDF deliverable, from stage 19, is one file with no pages to link
between, so the output stage reports that it wrote the file and checks the
links of the HTML deliverables. Warnings from the PDF renderer are
summarized on one line. For example, on stage 19, trimmed to the relevant lines:

```
health   clean
build    full                 ok  /home/you/my-audacity-guide/out/full
build    book-pdf             ok  /home/you/my-audacity-guide/out/book-pdf
  PDF rendering reported 11 warnings (7 The following feature isn't implemented by Apache FOP, yet: table-layout=… (on fo:table) (…, 1 The contents of fo:external-graphic line n exceed the available area in the inline-progres…, 1 The contents of fo:instream-foreign-object line n exceed the available area in the inline-…, and 2 other kinds)
output   wrote a file, no pages to check links in book-pdf
output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection)
Ready: the project is healthy, every deliverable built, and every link in the pages of 7 deliverables leads somewhere.
```

The PDF is built with the DITA-OT that the editor includes. You do not need
to install anything else.

## When it fails

When health fails, the check names the files with problems. For an invalid
file, it also shows the line, column, and message of the first three
errors. For example, on stage 03 with a `<section>` nested inside another
`<section>`:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/topics/what-is-audacity.dita
    27:15  The content of element type "section" does not match its content model.
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

Broken references, undefined keys, metadata policy errors, conref pushes,
and house rules findings are listed in the same way, with the file and
line. For the full report, open the **Project Validation** panel in the
editor, or run:

```bash
dogsbay-xml project-health .
```

When a build fails, the check names the deliverable and reports the
DITA-OT errors, with their message codes, such as `DOTJ046E`, when
DITA-OT gives one. When the output check fails, it lists each link that leads
nowhere, with the page and line that contains it, and the verdict counts
them, for example `Not ready: 1 link in the built output leads nowhere.`

Fix the problem and check again. After a lesson's deliberate-error
exercise, undo the change and check again before you continue.

## Find the output

Each deliverable builds into the folder in its **Output** field:
`out/full/` for the full guide, `out/beginner-mac/` for the macOS beginner
guide, and so on, inside `my-audacity-guide`. If you track your project
with Git, stage 03 adds `out/` to `.gitignore` so that Git ignores the
output.

To build and check one deliverable only:

```bash
dogsbay-xml check --deliverable=full .
```

## For maintainers

Each branch of the sample project also carries `scripts/check-stage.sh`,
the script used to verify the branches. Readers do not need it. See
[The stage gate](/reference/the-gate).

## Where to go next

:::cards
- **[Stage 00: Create your project folder](/part-1-topics/stage-00-setup)** {icon="play"}
  Start the guide in an empty folder.

- **[The stage gate](/reference/the-gate)** {icon="book-open"}
  The maintainers' script, and why `xmllint` is not used.
:::
