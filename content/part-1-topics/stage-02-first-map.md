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

The map has three elements:

- `<map>` is the root element. The `map` DOCTYPE declares it. Unlike a
  topic, a map does not need an `@id`.
- `<title>` is the title of the publication.
- `<topicref>` adds one topic to the publication. Its `@href` is relative
  to the map file. The navigation label comes from the title of the topic,
  so the map does not repeat it.

Add a `<topicref>` each time you add a topic to the guide. Later lessons
introduce hierarchy and generated links.

## Select and validate the map

Set the default root map for the project:

1. Click **Project** > **Manage Projects**.
2. Select the project.
3. In the **Default Root Map** field, enter `audacity-guide.ditamap`.

The project stores the setting in `.dogsbay/config.xml` as
`<default-root-map>audacity-guide.ditamap</default-root-map>`. The health
check uses this map to decide which topics belong to the publication.

Validate the document in the editor, or run these commands from the project
root:

```bash
dogsbay-xml validate audacity-guide.ditamap
scripts/check-stage.sh
```

The gate checks the source. Stage 03 adds a deliverable so the gate can also
publish HTML.

Validation checks the map against its grammar only. It does not check that
each `@href` points to a file. To see the difference, change the `@href` to
`topics/what-is-audacty.dita` and run `dogsbay-xml validate
audacity-guide.ditamap` again. It still prints `VALID`. Now run the gate.
The project health check reports the broken reference, and the topic that
the map no longer includes. The following output is an example:

```
== project-health  /home/you/audacity-guide ==
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Broken references (1):
  /home/you/audacity-guide/audacity-guide.ditamap:6  @href="topics/what-is-audacty.dita"
Orphan topics (1):
  /home/you/audacity-guide/topics/what-is-audacity.dita

Summary
  Broken references             1
  Orphan topics                 1
FAIL: project-health found issues
```

Restore the file name and rerun the gate.

## Next lesson

Continue with [Stage 03: first build](/part-1-topics/stage-03-first-build).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/01-concept...tutorial/02-first-map).
