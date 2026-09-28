---
title: "Stage 03: Publish the first guide"
description: Define an HTML deliverable, build the guide with DITA-OT, and inspect its generated pages.
type: tutorial
---

# Stage 03: Publish the first guide

Publish the map from stage 02. Keep this build throughout the course so you
can inspect every topic, link, and navigation change as you make it.

**You need:** stage 02 complete. **Time:** about 10 minutes.

## Define the deliverable

Create `project.json` in the project root:

```json
{
  "deliverables": [{
    "name": "full",
    "context": {"id": "full", "input": "audacity-guide.ditamap"},
    "output": "out/full",
    "publication": {"transtype": "html5", "params": [{"name": "nav-toc", "value": "full"}]}
  }]
}
```

The context identifies the root map. The publication selects the HTML5
transformation and requests navigation on each page. The output folder is
relative to the project root, so this deliverable builds into `out/full/`. Select `full` as the default deliverable
in the editor's project settings.

## Build and inspect

Check your work. From now on, the check also builds every deliverable and
checks the built output. You do not need to install DITA-OT: the editor and
the command line include DITA-OT 4.3.5.

In the editor, choose **Project** > **Check Project**. The result appears in
the **Project Validation** panel. From the project root on the command line,
run:

```bash
dogsbay-xml check .
```

The output looks like this example:

```
health   clean
build    full                 ok  /home/you/audacity-guide/out/full
output   clean (full)
Ready: the project is healthy, every deliverable built, and the output of full holds together.
```

The `build` line shows where the deliverable was built. Open
`out/full/index.html`, follow the topic link, and check the heading, short
description, list, and section.

Change the topic title, check again, and refresh the browser. Check both the
page heading and navigation label. Restore the title and check again when
you finish the exercise.

If the check fails, check the working directory and the map-relative paths.
The check stops at the first stage that fails. For details, see
[Check your work](/start-here/run-the-gate).

## Next lesson

Continue with [Stage 04: task and reference](/part-1-topics/stage-04-task-and-reference).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/02-first-map...tutorial/03-first-build).
