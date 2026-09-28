---
title: "Stage 00: Set up the project"
description: An empty DITA project that the tools recognize, with the shared editor settings and the gate script that every later stage must pass.
type: tutorial
---

# Stage 00: Set up the project

Create the project settings, validation script, and README for the guide.
The DogsBay XML editor uses `.dogsbay/config.xml` to select the DITA
framework and formatting settings. The script `scripts/check-stage.sh`
runs the checks used throughout the tutorial.

A topic's DOCTYPE identifies its grammar. For example,
`-//OASIS//DTD DITA Concept//EN` identifies the concept DTD. An XML catalog
maps that identifier to a local DTD. The tools can then validate the topic
without downloading the grammar.

**Time:** about 10 minutes.
**You need:** the tools from [Set up your tools](/start-here/set-up), and a
clone of the repository on `tutorial/00-setup` if you want to compare.


Recorded diagnostic examples below come from earlier runs. File counts, paths, and stage numbers can differ. Run the gate for your current checkout.

## Step 1: Create the project folder

::::steps
1. **Create your authoring repository**
   Run these commands from the directory where you want to keep your work.
   If you are inspecting completed stages in the clone, read the examples
   and skip the file-creation steps.

   ```bash
   mkdir audacity-guide && cd audacity-guide
   git init
   git remote add origin https://github.com/dogsbay/dogsbay-xml-dita-tutorial.git
   git fetch origin
   mkdir -p .dogsbay scripts topics
   touch topics/.gitkeep
   ```

   Fetching adds the reference branches as `origin/tutorial/NN-slug`
   without adding their files to your working directory. Later lessons
   use these refs to retrieve images and compare your work.

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

3. **Keep personal settings out of Git**
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

## Step 3: Write the README and license

::::steps
1. **Write the README**
   The "You are on" line is the one line that changes at every stage. The
   stage table lists the whole ladder.

   ````md title="README.md"
   # DITA tutorial
   
   Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
   
   You are on stage 00: setup.
   
   ## Stages
   
   | Branch | Lesson |
   |---|---|
   | `tutorial/00-setup` | setup |
   | `tutorial/01-concept` | concept |
   | `tutorial/02-first-map` | first map |
   | `tutorial/03-first-build` | first build |
   | `tutorial/04-task-and-reference` | task and reference |
   | `tutorial/05-inline-and-block` | inline and block |
   | `tutorial/06-rich-tasks` | rich tasks |
   | `tutorial/07-links` | links |
   | `tutorial/08-figures` | figures |
   | `tutorial/09-map-structure` | map structure |
   | `tutorial/10-keys` | keys |
   | `tutorial/11-reuse` | reuse |
   | `tutorial/12-glossary` | glossary |
   | `tutorial/13-metadata-and-index` | metadata and index |
   | `tutorial/14-conditional-text` | conditional text |
   | `tutorial/15-subject-scheme` | subject scheme |
   | `tutorial/16-branch-filtering` | branch filtering |
   | `tutorial/17-key-scopes` | key scopes |
   | `tutorial/18-chunking-and-output` | chunking and output |
   | `tutorial/19-bookmap` | bookmap |
   | `tutorial/20-troubleshooting` | troubleshooting |
   | `tutorial/21-hazards-and-safety` | hazards and safety |
   | `tutorial/22-software-domains` | software domains |
   | `tutorial/23-learning` | learning |
   | `tutorial/24-drafts-and-localization` | drafts and localization |
   | `tutorial/25-house-rules` | house rules |
   | `tutorial/26-final` | final |
   
   ## Verification
   
   Run `scripts/check-stage.sh` from the project root. From stage 03, the gate builds the deliverables in `project.json`.
   Run `python3 scripts/check-output-links.py <output-directory>` after publishing HTML. Source health and generated output checks report separate results.
   
   The chunking experiment under `examples/chunking/` is available from stage 18. Build its maps separately; they are not release deliverables.
   
   Topic text is adapted from the Audacity Manual. See LICENSE and NOTICE for licensing and attribution.
   ````

2. **Add the license and the credit**
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
   International license; copy it from
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

## Next lesson

Continue with [Stage 01: concept](/part-1-topics/stage-01-concept).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.
