---
title: "Stage 00: Set up the project"
description: An empty DITA project that the tools recognise, with the shared editor settings and the gate script that every later stage must pass.
type: tutorial
---

# Stage 00: Set up the project

In this stage you build an empty DITA project: no topics yet, only the files
that make a folder a project. The DogsBay XML editor reads `.dogsbay/config.xml`
to know that this is a DITA project on DITA-OT 4.3.5 and how to format files,
and the gate script `scripts/check-stage.sh` is the check every later stage
must pass.

There is no DITA to learn here, but there is one thing to understand: how a
DITA file finds its grammar. Every topic in this tutorial starts with a
DOCTYPE that names a public identifier such as
`-//OASIS//DTD DITA Concept//EN`. Neither the editor nor DITA-OT fetches
anything for it; they map the identifier to a DTD on disk through the DITA-OT
catalog, which is why the project only has to say which DITA-OT it targets.

**Time:** about 10 minutes.
**You need:** the tools from [Set up your tools](/start-here/set-up), and a
clone of the repository on `tutorial/00-setup` if you want to compare.

## Step 1: Create the project folder

::::steps
1. **Start an empty repository**
   The stage branch is an orphan, so start from nothing rather than from
   `main`.

   ```bash
   mkdir audacity-guide && cd audacity-guide
   git init
   mkdir -p .dogsbay scripts topics
   touch topics/.gitkeep
   ```

2. **Write the editor settings**
   `.dogsbay/config.xml` is shared by everyone who opens the project. It sets
   the project type, the framework the DOCTYPEs resolve against, and the
   format style that `dogsbay-xml format` and the editor apply, so diffs
   between stages stay about the feature and not about whitespace.

   ```xml title=".dogsbay/config.xml"
   <?xml version="1.0" encoding="UTF-8"?>

   <dogsbay-project>
     <project-type>DITA</project-type>
     <framework>DITA-OT 4.3.5</framework>
     <format-style indent="spaces" size="2" max-line-width="0" preserve-mixed="true" newline="lf" final-newline="true" preserve-text-breaks="true" preserve-blank-lines="true" text-continuation="block" trim-whitespace="true"/>
   </dogsbay-project>
   ```

3. **Keep personal settings out of git**
   The editor writes a `local.xml` beside `config.xml` for the active
   deliverable and window state. Ignore it inside the folder.

   ```gitignore title=".dogsbay/.gitignore"
   # Personal editor overrides (active deliverable, window state). Not shared.
   local.xml
   ```

4. **Ignore the build output**
   DITA-OT writes generated HTML and PDF. Keep it out of the repository.

   ```gitignore title=".gitignore"
   # Built deliverables (DITA-OT output): each deliverable's output in project.json
   # is under out/. Keep generated HTML/PDF out of git.
   out/
   output/
   *.tmp
   ```
::::

## Step 2: Add the gate

::::steps
1. **Write the script**
   Save it as `scripts/check-stage.sh` and make it executable. It runs
   `validate-project`, then `project-health`, then, once the project has a
   `project.json`, a DITA-OT build of every deliverable.

   ```bash title="scripts/check-stage.sh"
   #!/usr/bin/env bash
   # check-stage.sh — the gate every tutorial stage branch must pass.
   #
   # Runs, from the project root:
   #   1. dogsbay-xml validate-project   every topic and map validates against its DOCTYPE
   #   2. dogsbay-xml project-health     links, keys, element ids, conref pushes, index,
   #                                     metadata policy and house rules are clean
   #   3. dita --project=project.json    every deliverable actually publishes
   #
   # Exit code is non-zero when any gate fails. Usage:
   #   scripts/check-stage.sh [project-root]           (default: the repo root)
   #   STRICT=0 scripts/check-stage.sh                 (project-health at severity=error only)
   #   SKIP_BUILD=1 scripts/check-stage.sh             (skip the DITA-OT build)
   #
   # Tooling is found from, in order: $DOGSBAY_XML / $DITA_HOME, the PATH, then the
   # developer defaults below.
   set -u

   ROOT="$(cd "${1:-$(dirname "$0")/..}" && pwd)"
   DOGSBAY_XML="${DOGSBAY_XML:-$(command -v dogsbay-xml || echo "$HOME/github/dogsbay-xml/bin/dogsbay-xml")}"
   DITA_HOME="${DITA_HOME:-$HOME/Downloads/dita-ot-4.3.5}"
   DITA="${DITA:-$(command -v dita || echo "$DITA_HOME/bin/dita")}"
   STRICT="${STRICT:-1}"
   SKIP_BUILD="${SKIP_BUILD:-0}"
   OUT="${OUT:-${TMPDIR:-/tmp}/check-stage-$$}"

   status=0
   say() { printf '\n== %s ==\n' "$*"; }
   fail() { status=1; printf 'FAIL: %s\n' "$*"; }

   [ -x "$DOGSBAY_XML" ] || { echo "dogsbay-xml CLI not found (set DOGSBAY_XML)"; exit 2; }

   say "validate-project  $ROOT"
   "$DOGSBAY_XML" validate-project "$ROOT" || fail "validation errors"

   say "project-health  $ROOT"
   if [ "$STRICT" = "1" ]; then
     "$DOGSBAY_XML" project-health "$ROOT" || fail "project-health found issues"
   else
     "$DOGSBAY_XML" project-health --severity=error "$ROOT" || fail "project-health found errors"
   fi

   if [ "$SKIP_BUILD" = "1" ]; then
     say "build  skipped (SKIP_BUILD=1)"
   elif [ -f "$ROOT/project.json" ]; then
     [ -x "$DITA" ] || { echo "dita command not found (set DITA_HOME or DITA)"; exit 2; }
     say "build  project.json -> $OUT"
     log="$OUT.log"; mkdir -p "$OUT"
     # DITA-OT does not always exit non-zero on content errors, so also grep the log.
     if ! (cd "$ROOT" && "$DITA" --project=project.json --output="$OUT" >"$log" 2>&1) \
        || grep -qE '^Error:|\[(DOT[A-Z]+[0-9]+[EF])\]' "$log"; then
       grep -E '^Error:|\[(DOT[A-Z]+[0-9]+[EF])\]' "$log" | sort -u | head -40
       fail "DITA-OT build reported errors (full log: $log)"
     else
       echo "all deliverables built"
     fi
   else
     say "build  skipped (no project.json yet)"
   fi

   echo
   [ "$status" = 0 ] && echo "STAGE OK" || echo "STAGE FAILED"
   exit "$status"
   ```

   ```bash
   chmod +x scripts/check-stage.sh
   ```

2. **Read the three steps**
   Each is a `say` header, a command and a `fail` on error. The build step
   greps the DITA-OT log for `Error:` and for `E` and `F` message codes
   because DITA-OT does not always exit non-zero when content is wrong. The
   details are on [Run the gate](/start-here/run-the-gate).
::::

## Step 3: Write the README and licence

::::steps
1. **Write the README**
   The "You are on" line is the one line that changes at every stage. The
   stage table lists the whole ladder.

   ````md title="README.md"
   # DogsBay DITA tutorial — step by step

   A DITA 1.3 project built up one feature at a time. Each `tutorial/NN-slug`
   branch is the previous branch plus one lesson, from a single concept topic to a
   complete, publish-ready **Audacity User Guide**. The diff between two branches
   is the lesson.

   You are on **stage 00 — setup**: an empty project the tools recognise, and the
   gate every later stage must pass.

   ## Stages

   | Branch | Adds |
   |--------|------|
   | `tutorial/00-setup` | project settings, the gate script |
   | `tutorial/01-concept` | one concept topic |
   | `tutorial/02-task-and-reference` | a task and a reference topic |
   | `tutorial/03-inline-and-block` | inline semantics, notes, lists, code |
   | `tutorial/04-rich-tasks` | prereqs, substeps, choices, examples |
   | `tutorial/05-links` | cross-references and related links |
   | `tutorial/06-figures` | images, SVG and an equation |
   | … | maps, keys, reuse, glossary, metadata, conditions, subject schemes, branch filtering, key scopes, chunking, bookmap, troubleshooting, hazards, software domains, learning, drafts and localization, house rules |
   | `tutorial/25-final` | the complete guide (also tagged `tutorial/final`) |

   The walkthrough for readers is the docs site (see the `main` branch README).

   ## The gate

   ```bash
   scripts/check-stage.sh            # validate every file, check project health, build every deliverable
   SKIP_BUILD=1 scripts/check-stage.sh
   ```

   It needs the `dogsbay-xml` CLI and DITA-OT 4.3.5 (`DOGSBAY_XML`, `DITA_HOME`
   override the defaults). A stage is done when it prints `STAGE OK`.

   ## Layout

   ```
   .dogsbay/config.xml   shared editor project settings (project type, framework, format style)
   scripts/              the gate
   topics/               topics (empty at this stage)
   ```

   ## Licence and attribution

   CC BY 4.0, see [LICENSE](LICENSE). Topic text is adapted from the
   [Audacity Manual](https://manual.audacityteam.org/) (CC BY 3.0); see
   [NOTICE](NOTICE) for the credit the licence requires.
   ````

2. **Add the licence and the credit**
   The project is CC BY 4.0. The topic text in later stages is adapted from
   the Audacity Manual, which is CC BY 3.0 and requires a credit, so `NOTICE`
   carries it from the first stage.

   ```text title="NOTICE"
   DogsBay DITA tutorial — step by step
   Copyright (C) 2026 DogsBay Ltd.

   This work is licensed under the Creative Commons Attribution 4.0
   International licence. See LICENSE for the full text.

   -------------------------------------------------------------------------------
   Adapted material: the Audacity Manual
   -------------------------------------------------------------------------------
   Files:    topics/**, shared/**
   Source:   The Audacity Manual, https://manual.audacityteam.org/
   Credit:   Copyright the Audacity Team and the Manual's authors.
   Licence:  Creative Commons Attribution 3.0
             https://creativecommons.org/licenses/by/3.0/

   The Manual states: "Pages in this Manual are available under the terms of the
   Creative Commons Attribution 3.0 license. In essence, you are free to (1) copy,
   distribute and transmit the work (2) to adapt the work, under condition you
   must attribute the work to the authors (but not in any way that suggests that
   they endorse you or your use of the work). For any reuse or distribution, you
   may not remove our copyright notice and must make clear to others the license
   terms of this work."

   That notice is retained here, and the licence terms of the source work are
   stated above, as the licence requires.

   Changes made: the material was rewritten as DITA 1.3 topics, shortened, and
   restructured into maps to teach DITA. It is a teaching sample, not Audacity's
   documentation, and may be out of date or simplified. The Audacity Team does not
   endorse this work and is not affiliated with it.

   Audacity(R) is a registered trademark of Dominic Mazzoni.

   -------------------------------------------------------------------------------
   Original material
   -------------------------------------------------------------------------------
   README.md, the DITAVAL filters under filters/, house-style.sch, project.json,
   the scripts/ and the .dogsbay/ and .xagent/ configuration are original work by
   DogsBay Ltd., under the same CC BY 4.0 licence as this work.
   ```

   `LICENSE` is the full text of the Creative Commons Attribution 4.0
   International licence; copy it from
   [creativecommons.org](https://creativecommons.org/licenses/by/4.0/legalcode.txt)
   or from the branch.
::::

## Step 4: Run the gate

```bash
scripts/check-stage.sh
```

With no topics there is nothing to validate, no map to check and no
`project.json` to build, so the gate passes at once:

```
== validate-project  /home/you/audacity-guide ==
0 file(s): 0 valid, 0 invalid.

== project-health  /home/you/audacity-guide ==
Root map: none (none configured); house rules: none (none configured)
Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

== build  skipped (no project.json yet) ==

STAGE OK
```

If it prints `dogsbay-xml CLI not found` or `dita command not found`, go back
to [Set up your tools](/start-here/set-up) and set `DOGSBAY_XML` or
`DITA_HOME`.

## What you learned

- A DITA project is a folder of topics and maps; nothing declares it except
  the files themselves and, for the editor, `.dogsbay/config.xml`.
- DOCTYPEs are resolved through the DITA-OT catalog, so a project states which
  DITA-OT it targets and the DTDs come from there.
- The gate is three commands. `STAGE OK` means a stage is done.
- Format style lives in the project so every stage's diff is about the
  feature.

## Where to go next

:::cards
- **[Stage 01: A concept topic](/part-1-topics/stage-01-concept)** {icon="arrow-right"}
  Write *What is Audacity?* as a `<concept>` and validate one file.

- **[The branch on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/tree/tutorial/00-setup)** {icon="github"}
  `tutorial/00-setup`, the orphan root of the chain.
:::
