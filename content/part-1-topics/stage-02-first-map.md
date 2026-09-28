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

Create the map from a template:

1. In the **Explorer**, right-click the project root and choose
   **New File**.
2. Enter `audacity-guide.ditamap` and press Enter. The
   **New XML Document** dialog lists the templates for `.ditamap` files.
3. Choose **Map** and click **OK**.

The editor opens the new file with the template's content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">

<map>
  <title>Map Title</title>
  <topicref href="topic.dita"/>
</map>
```

Click inside the title, choose **XML** > **Select Element Content**
(Ctrl+Shift+E), and type `Audio user guide`. Then replace the placeholder
`topic.dita` in the `@href` with `topics/what-is-audacity.dita`. If you use
another editor, create the file in the project root and type the finished
listing.

The finished map:

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

Validate the map in the editor with **XML** > **Validate**, or run this
command from the project root:

```bash
dogsbay-xml validate audacity-guide.ditamap
```

Then check your work. In the editor, choose **Project** > **Check Project**.
The result appears in the **Project Validation** panel. From the command
line, run:

```bash
dogsbay-xml check .
```

The output looks like this example:

```
health   clean
Project health is clean — no deliverables yet, so nothing here speaks for the output.
```

The orphan warning from stage 01 is gone, because the map now refers to the
topic. The check reads the source only. Stage 03 adds a deliverable, so the
check can also build HTML. Until then, look for `health   clean`.

Validation checks the map against its grammar only. It does not check that
each `@href` points to a file. To see the difference, change the `@href` to
`topics/what-is-audacty.dita` and run `dogsbay-xml validate
audacity-guide.ditamap` again. It still prints `VALID`. Now check your work.
The check reports the broken reference, and the topic that the map no longer
includes. The following output is an example:

```
health   NOT CLEAN
  /home/you/audacity-guide/audacity-guide.ditamap:6  @href="topics/what-is-audacty.dita" — target not found
  (run project-health for the full report)
  orphan topic: /home/you/audacity-guide/topics/what-is-audacity.dita — nothing refers to it, so it will not appear in the output
Not ready: the project itself has faults. The build and the built output were not checked.
```

The **Project Validation** panel, or `dogsbay-xml project-health .`, gives
the full report. For example:

```
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Broken references (1):
  /home/you/audacity-guide/audacity-guide.ditamap:6  @href="topics/what-is-audacty.dita"
Orphan topics (1):
  /home/you/audacity-guide/topics/what-is-audacity.dita

Summary
  Broken references             1
  Orphan topics                 1
```

Restore the file name and check again. Confirm that the check reports
`health   clean` before you continue.

## Next lesson

Continue with [Stage 03: first build](/part-1-topics/stage-03-first-build).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/01-concept...tutorial/02-first-map).
