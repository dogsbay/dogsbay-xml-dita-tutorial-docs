---
title: "Stage 02: Create the first map"
description: Add your first topic to a DITA map and select the map as the project root.
type: tutorial
---

# Stage 02: Create the first map

A map selects the topics in a publication and their navigation order. Create
one now so that later lessons can add topics to a working guide.

**You need:** stage 01 complete. **Time:** about 10 minutes.

## Create the guide map

Save this file as `audacity-guide.ditamap` in the project root:

```xml title="audacity-guide.ditamap"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">

<map>
  <title>Audio user guide</title>
  <topicref href="topics/what-is-audacity.dita"/>
</map>
```

The `href` is relative to the map. The map title names the publication;
the topic title supplies its navigation label. Add a topicref when you add
a topic to the guide. Later lessons introduce hierarchy and generated links.

## Select and validate the map

Set the default root map to `audacity-guide.ditamap` in the project settings.
Validate the document in the editor, or run from the project root:

```bash
dogsbay-xml validate audacity-guide.ditamap
scripts/check-stage.sh
```

The gate checks the source. Stage 03 adds a deliverable so the gate can also
publish HTML. Try an incorrect topic filename, check the diagnostic, then
restore the filename and validate again.

## Next lesson

Continue with [Stage 03: first build](/part-1-topics/stage-03-first-build).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/01-concept...tutorial/02-first-map).
