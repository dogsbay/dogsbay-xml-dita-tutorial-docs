---
title: "Stage 00: Set up the project"
description: An empty DITA project that the tools recognize, with a README, a license, and a first check of your work.
type: tutorial
---

# Stage 00: Set up the project

Create the project folder and README for the guide, then check the
project. At the end of every lesson, you check
your work with **Project** > **Check Project** in the editor or
`dogsbay-xml check` on the command line.

A topic's DOCTYPE identifies its grammar. For example,
`-//OASIS//DTD DITA Concept//EN` identifies the concept DTD. An XML catalog
maps that identifier to a local DTD. The tools can then validate the topic
without downloading the grammar.

**Time:** about 10 minutes.
**You need:** the tools from [Set up your tools](/start-here/set-up), and a
clone of the repository on `tutorial/00-setup` if you want to compare.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

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
   mkdir topics
   touch topics/.gitkeep
   ```

   Fetching adds the reference branches as `origin/tutorial/NN-slug`
   without adding their files to your working directory. Later lessons
   use these refs to retrieve images and compare your work.

   Each stage branch also has a `scripts/` folder. Its scripts are checks
   for the tutorial maintainers. You do not need them in your repository.

   The editor creates its project settings when you create the first topic
   in stage 01. You do not edit them.

2. **Ignore the build output**
   DITA-OT writes generated HTML and PDF. Keep it out of the repository.

   ```gitignore title=".gitignore"
   # Built deliverables (DITA-OT output): each deliverable's output in project.json
   # is under out/. Keep generated HTML/PDF out of git.
   out/
   output/
   *.tmp
   ```
::::

## Step 2: Write the README and license

::::steps
1. **Write the README**
   The "You are on" line is the one line that changes at every stage. The
   stage table lists the whole ladder. The "Verification" section names
   `scripts/`, which holds maintainer checks that readers do not need.

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

## Step 3: Check your work

Check the project. In the editor, choose **Project** > **Check Project**. The
result appears in the **Project Validation** panel. From the command line,
run this command from the project root:

```bash
dogsbay-xml check .
```

With no topics and no map, the source is clean. The project declares no
deliverables until stage 03, so there is nothing to build yet, and the
check reports on the source only. The output looks like this example:

```
health   clean
Project health is clean — no deliverables yet, so nothing here speaks for the output.
```

Before stage 03, `health   clean` means the stage is done. If the
command is not found, go back to [Set up your tools](/start-here/set-up) and
add the command line to your `PATH`. For more about the result, see
[Check your work](/start-here/run-the-gate).

## What you learned

- A DITA project is a folder of topics and maps; nothing declares it except
  the files themselves.
- DOCTYPEs are resolved through the catalog of the bundled DITA-OT, so a project states which
  DITA-OT it targets and the DTDs come from there.
- **Project** > **Check Project**, or `dogsbay-xml check`, checks your
  work. Before stage 03, `health   clean` means a stage is done.
- The editor keeps the project settings, including the format style, so
  every stage's diff is about the feature.

## Next lesson

Continue with [Stage 01: concept](/part-1-topics/stage-01-concept).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.
