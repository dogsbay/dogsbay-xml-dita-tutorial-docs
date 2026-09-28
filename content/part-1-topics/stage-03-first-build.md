---
title: "Stage 03: Publish the first guide"
description: Define an HTML deliverable, build the guide with DITA-OT, and inspect its generated pages.
type: tutorial
---

# Stage 03: Publish the first guide

Publish the map from stage 02. Keep this build throughout the course so you
can inspect every topic, link, and navigation change as you make it.

**You need:** stage 02 complete and DITA-OT installed. **Time:** about 10 minutes.

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
transformation and requests navigation on each page. The output is relative
to the build's output base directory. Select `full` as the default deliverable
in the editor's project settings.

## Build and inspect

From the project root, run:

```bash
scripts/check-stage.sh
dita --project=project.json --output=out
python3 scripts/check-output-links.py out
```

The gate builds into a temporary directory. The second command keeps a local
copy under `out/out/full/`. Open its `index.html`, follow the topic link, and
check the heading, short description, list, and section.

Change the topic title, rebuild, and refresh the browser. Check both the page
heading and navigation label. Restore the title when you finish the exercise.

If a command fails, check the working directory, the map-relative paths, and
the configured DITA-OT installation. A successful source check does not ensure
that generated links work; inspect the output-link check separately.

## Next lesson

Continue with [Stage 04: task and reference](/part-1-topics/stage-04-task-and-reference).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/02-first-map...tutorial/03-first-build).
