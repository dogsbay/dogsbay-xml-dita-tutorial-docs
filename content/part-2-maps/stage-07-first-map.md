---
title: "Stage 07: Your first map"
description: Put the nine topics in a DITA map, declare an html5 deliverable in project.json, and build the guide with DITA-OT for the first time.
type: tutorial
---

# Stage 07: Your first map

In this stage the nine topics become a guide. A DITA map is the file that
says which topics belong to a publication and in what order; nothing in a
topic says that. `audacity-guide.ditamap` groups the topics under three
headings, `project.json` tells DITA-OT how to publish the map, and the gate
runs that build from now on, so "the stage is done" now also means "the
guide publishes".

No topic changes. The three new files are shown whole, and the edit to
`.dogsbay/config.xml` tells the editor and `project-health` which map is the
root of the project.

**Time:** about 20 minutes.
**You need:** stage 06 complete, and DITA-OT 4.3.5 on `DITA_HOME` or `PATH`
(see [Set up your tools](/start-here/set-up)).

## Step 1: Write the map

::::steps
1. **Create `audacity-guide.ditamap`** in the project root, beside `topics/`.

   ```xml title="audacity-guide.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">

   <map>
     <title>Audacity User Guide</title>
     <topicmeta>
       <shortdesc>Record, edit and export audio with Audacity, from your first track to a finished file.</shortdesc>
     </topicmeta>

     <topichead navtitle="Getting started">
       <topicref href="topics/what-is-audacity.dita"/>
       <topicref href="topics/what-is-digital-audio.dita"/>
       <topicref href="topics/installing-audacity.dita"/>
     </topichead>

     <topichead navtitle="Recording and editing">
       <topicref href="topics/preparing-to-record.dita"/>
       <topicref href="topics/recording-your-first-track.dita"/>
       <topicref href="topics/trimming-audio.dita"/>
       <topicref href="topics/removing-background-noise.dita"/>
       <topicref href="topics/exporting-audio.dita"/>
     </topichead>

     <topichead navtitle="Reference">
       <topicref href="topics/supported-audio-formats.dita"/>
     </topichead>
   </map>
   ```

2. **Read the elements**
   - `<map>` is the root, with its own DOCTYPE, `-//OASIS//DTD DITA Map//EN`.
     Its `<title>` is the title of the publication.
   - `<topicmeta>` holds metadata about the map or about a reference. Here
     it carries the guide's `<shortdesc>`; stage 12 adds author, publisher
     and dates to it.
   - `<topicref href="…">` puts a topic in the publication. The path is
     relative to the map, so from the project root it starts with `topics/`.
     Order in the map is order in the output.
   - `<topichead>` is a heading with no topic behind it. `@navtitle` gives
     it the text shown in the table of contents. It groups the `<topicref>`s
     inside it; the output nests them under the heading.
::::

## Step 2: Declare the deliverable

::::steps
1. **Create `project.json`**
   A DITA-OT project file lists the deliverables a project ships: which map,
   which transformation type, which parameters, and where the output goes.

   ```text title="project.json"
   {
     "deliverables": [
       {
         "name": "full",
         "context": {
           "id": "full",
           "input": "audacity-guide.ditamap"
         },
         "output": "out/full",
         "publication": {
           "transtype": "html5",
           "params": [
             { "name": "nav-toc", "value": "full" }
           ]
         }
       }
     ]
   }
   ```

2. **Read the file**
   - `deliverables` is a list; this stage has one, named `full`. Later
     stages add filtered variants and a PDF beside it.
   - `context.input` is the map to publish. `output` is the folder, relative
     to the project root, or to `--output` when the gate passes one.
   - `publication.transtype` is the DITA-OT transformation: `html5` is one
     HTML page per topic with a table of contents. `params` sets
     transformation parameters; `nav-toc=full` puts the whole table of
     contents on every page.

3. **Build it by hand**
   The project file and the map can both be given to `dita` directly.
   Either command below writes `out/full/index.html` and one page per topic
   under `out/full/topics/`.

   ```bash
   dita --project=project.json                                # every deliverable in the file
   dita -i audacity-guide.ditamap -f html5 -o out/full        # this map, this transtype
   ```

   `out/` is ignored by git since stage 00. Open `out/full/index.html` in a
   browser: the three headings are the `<topichead>`s, and the decibel
   formula from stage 06 shows as its text alternative, because the html5
   transform drops MathML without a plugin.
::::

## Step 3: Tell the editor which map is the root

::::steps
1. **Edit `.dogsbay/config.xml`**

   ```diff title=".dogsbay/config.xml"
   --- a/.dogsbay/config.xml
   +++ b/.dogsbay/config.xml
   @@ -2,6 +2,8 @@
    
    <dogsbay-project>
      <project-type>DITA</project-type>
   +  <default-root-map>audacity-guide.ditamap</default-root-map>
      <framework>DITA-OT 4.3.5</framework>
      <format-style indent="spaces" size="2" max-line-width="0" preserve-mixed="true" newline="lf" final-newline="true" preserve-text-breaks="true" preserve-blank-lines="true" text-continuation="block" trim-whitespace="true"/>
   +  <default-deliverable file="project.json" name="full"/>
    </dogsbay-project>
   ```

2. **Read the settings**
   - `<default-root-map>` names the map that `project-health` analyses. With
     a root map, the health check resolves keys (from stage 09) and knows
     which topics are in the publication. The gate output changes from
     `Root map: none (none configured)` to
     `Root map: audacity-guide.ditamap (project config)`.
   - `<default-deliverable>` names the deliverable the editor previews and
     builds by default: the `full` entry in `project.json`.
::::

## Step 4: Update the README and run the gate

::::steps
1. **Change the "You are on" line and the layout**

   ````diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 06 — figures**: images with alt text, an inline SVG, an image map and a MathML equation.
   +You are on **stage 07 — first map**: a map, a deliverable in `project.json`, and the first DITA-OT build.
    
    ## Stages
    
   @@ -36,7 +36,9 @@ override the defaults). A stage is done when it prints `STAGE OK`.
    ## Layout
    
    ```
   -.dogsbay/config.xml   shared editor project settings (project type, framework, format style)
   +audacity-guide.ditamap  the map (start here)
   +project.json          DITA-OT project file: the deliverables this guide ships
   +.dogsbay/config.xml   shared editor project settings (project type, framework, default map and deliverable, format style)
    scripts/              the gate
    topics/               topics
    images/               illustrations referenced by <image>
   ````

2. **Check**
   There is nothing new to format. The gate now finds `project.json` and
   runs the build as its third step, into a temporary folder it names in
   the header line.

   ```bash
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   10 file(s): 10 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == build  project.json -> /tmp/check-stage-357688 ==
   all deliverables built

   STAGE OK
   ```

   The map counts as a file, so ten validate. The build takes a few
   seconds; `SKIP_BUILD=1 scripts/check-stage.sh` skips it while you are
   editing.
::::

The map is the first file whose references `project-health` checks against
the whole project. Misspell the trimming task in the map as
`topics/trimming-audo.dita` and run the gate:

```
== project-health  /home/you/audacity-guide ==
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Broken references (1):
  /home/you/audacity-guide/audacity-guide.ditamap:19  @href="topics/trimming-audo.dita"

Summary
  Broken references             1
FAIL: project-health found issues
```

The map still validates, because the DTD does not know whether a file
exists. DITA-OT would report `[DOTX008E] The resource … cannot be loaded`
and the gate's build step would catch that too, but the health check
catches it first, in a second, without a build.

## What you learned

- A map lists the topics of a publication; a topic knows nothing about the
  maps that use it.
- `<map>`, `<title>`, `<topicmeta>` with `<shortdesc>`, `<topichead>` with
  `@navtitle`, `<topicref>` with `@href`.
- `project.json` declares deliverables: map, transtype, parameters and
  output folder. `dita --project=project.json` builds them all.
- `.dogsbay/config.xml` names the default root map and deliverable, and
  `project-health` reports the root map it used.
- The gate now has three steps, and a broken reference in the map fails
  the stage.

## Where to go next

:::cards
- **[Stage 08: Map structure](/part-2-maps/stage-08-map-structure)** {icon="arrow-right"}
  Hierarchy, sequences, link control and a relationship table, and the
  hand-written links come out of the topics.

- **[Compare 06 to 07 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/06-figures...tutorial/07-first-map)** {icon="github"}
  Exactly what this stage added.
:::
