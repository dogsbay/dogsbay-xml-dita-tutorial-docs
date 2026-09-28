---
title: Practice exercises
description: Apply topic design, maps, reuse, conditions, output review, and house rules without copying a complete solution.
type: tutorial
---

# Practice exercises

Complete each exercise in your authoring repository or a separate practice
worktree. Each checkpoint below is a fetched reference branch. Use the
[worktree procedure](/start-here/learning-path#continue-across-a-skipped-stage)
to keep practice changes separate from the supplied lessons.

Try the exercise before reading the [suggested solutions](/practice/solutions).
Passing XML validation is one check; also explain your design choices and
inspect the requested result.

## Topics

**Checkpoint:** `origin/tutorial/07-links`.

Add a task called *Checking a recording before export*. Include a short
description, prerequisites, three or four actions, and an observable result.
Use UI markup only for actual control labels. Link to the existing export
task. Decide whether background about export formats belongs in this task
or in the existing reference topic.

**Check:** Validate the new topic with `dogsbay-xml validate`. Read only its
`cmd` elements and confirm that the actions form a usable procedure. At this
checkpoint the guide already has a map and HTML deliverable. Add the topic
to the map, [check your work](/start-here/run-the-gate), and inspect the
new page in `out/full/`.

## Maps and reuse

**Checkpoint:** `origin/tutorial/11-reuse`.

Add a practice map that publishes the recording and export tasks. Include
the key definitions and shared resources those tasks require. Reference the
existing topics. Add a key for the recording task and link to it by key from
your new checking task.

Before adding another reusable step, compare its purpose with the shared
save step. Explain whether you can use the existing element unchanged.

**Check:** Add a deliverable named `practice` to `project.json`, with
`practice.ditamap` as its input and `out/practice` as its output. Then
check that deliverable. In the editor, **Project** > **Check Project**
checks every deliverable. From the command line, run:

```bash
dogsbay-xml check --deliverable=practice .
```

The check builds the practice map into `out/practice/` and checks the links
in the built pages. Inspect titles, links, resolved product names, and the
reused save instruction. The health stage uses the guide's root map, so it
does not check the key space of your new map.

## Conditions

**Checkpoint:** `origin/tutorial/15-subject-scheme`.

Add a short note for podcasters to an existing topic. Publish a beginner
variant and the Linux podcaster variant. Predict which output contains the
note before building. Then deliberately use `audience="podcatser"` and run
`dogsbay-xml validate-conditions -S subject-scheme.ditamap .`.

**Check:** The intended note appears only where the filter includes
podcasters. The misspelled value is reported. Restore `podcaster`, rerun the
check, and rebuild. Explain why a DITAVAL and a subject scheme serve
different purposes.

## Published output

**Checkpoint:** `origin/tutorial/19-bookmap`.

Build the HTML guide and PDF book. Select one figure, one table, and one
cross-reference. Check each in both formats. Write a short report separating
source content problems from publishing or layout problems.

**Check:** Record the actual output paths, destination of the link, image
alternative, table headers, and PDF reading order. Use the
[accessibility review](/reference/accessibility). An automated source check
alone is insufficient for this exercise.

## House rules

**Checkpoint:** `origin/tutorial/25-house-rules`.

Choose a concept and temporarily remove its short description. Run DTD
validation and the Schematron check separately:

```bash
dogsbay-xml validate topics/what-is-audacity.dita
dogsbay-xml project-health --include=schematron .
```

**Check:** Explain why the DTD can accept the file while the project rule
rejects it. Restore the short description and check your work. Confirm
that the deliberate error is absent before continuing to the
[capstone](/practice/capstone).
