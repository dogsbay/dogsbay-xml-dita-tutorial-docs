---
title: "Stage 10: Reuse"
description: Write steps, notes and a list once in a shared warehouse, then pull them into tasks by conref, conkeyref and a conref range, and push a step in.
type: tutorial
---

# Stage 10: Reuse

In this stage two tasks stop repeating each other. "Save the project" was
written in *Trimming audio* and is needed in *Recording your first track*;
the noise task's warning and settings belong anywhere an effect changes the
audio. Content reference, `@conref`, lets an element in one topic *be* an
element from another: the processor replaces the referencing element with a
copy of the target at build time, so the words exist once.

The reusable elements live in two warehouse topics under `shared/`. They are
part of the project, they validate and they are in the map, but they are
never published as pages. The stage shows four ways to reach them: a plain
`@conref` by path, a `@conkeyref` through a key, a range of list items with
`@conrefend`, and a conref push that inserts a step into a task from the
warehouse's side.

**Time:** about 30 minutes.
**You need:** stage 09 complete.

## Step 1: Write the warehouses

::::steps
1. **Create `shared/common-steps.dita`**
   A task whose steps exist to be referenced. Every reusable element has an
   `@id`.

   ```xml title="shared/common-steps.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">

   <task id="common-steps">
     <title>Common steps</title>
     <shortdesc>Reusable steps that other tasks pull in by conref. This task is a warehouse and is not published on its own.</shortdesc>
     <taskbody>
       <steps>
         <step id="select-region">
           <cmd>Select the region of audio you want to work on.</cmd>
           <info>
             <p>Click and drag in the waveform to select a region; the selection is highlighted.
             To select the whole track, press <uicontrol>Ctrl+A</uicontrol> (Windows and Linux) or <uicontrol>Cmd+A</uicontrol> (macOS).</p>
           </info>
         </step>
         <step id="save-project">
           <cmd>Save the project with <menucascade><uicontrol>File</uicontrol><uicontrol>Save Project</uicontrol><uicontrol>Save Project</uicontrol></menucascade>.</cmd>
           <info>
             <p><keyword keyref="product-name"/> saves the project as a single <keyword keyref="project-extension"/> file in the folder you choose; the <wintitle>Save Project</wintitle> dialog suggests your Documents folder.</p>
           </info>
         </step>
         <!-- Conref push: this step is inserted before the marked step in the recording task. -->
         <step conaction="pushbefore">
           <cmd>Turn the playback volume down before you listen.</cmd>
           <info>
             <p>A recording can be much louder than you expect; start low and turn it up.</p>
           </info>
         </step>
         <step conaction="mark" conref="../topics/recording-your-first-track.dita#recording-your-first-track/play-back">
           <cmd/>
         </step>
       </steps>
     </taskbody>
   </task>
   ```

2. **Create `shared/common-notes.dita`**
   A generic `<topic>` for notes and a list. `<topic>` is the base type,
   with `<body>` instead of `<conbody>` or `<taskbody>`; a warehouse of
   notes has no reason to be a concept or a task.

   ```xml title="shared/common-notes.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE topic PUBLIC "-//OASIS//DTD DITA Topic//EN" "topic.dtd">

   <topic id="common-notes">
     <title>Common notes</title>
     <shortdesc>Reusable notes, tips and lists that other topics pull in by conref. This topic is a warehouse and is not published on its own.</shortdesc>
     <body>
       <note id="backup-warning" type="warning">This effect changes the audio data.
       Save the project first; you can undo with <uicontrol>Ctrl+Z</uicontrol> (Windows and Linux) or <uicontrol>Cmd+Z</uicontrol> (macOS).</note>
       <note id="quiet-room-tip" type="tip">For best results, record in a quiet room and position the microphone 15 to 30 cm from your mouth.</note>
       <p>Recommended starting settings for Noise Reduction:</p>
       <ul id="noise-reduction-settings">
         <li id="nr-first">Noise reduction: 12 dB</li>
         <li>Sensitivity: 6</li>
         <li id="nr-last">Frequency smoothing: 3</li>
       </ul>
     </body>
   </topic>
   ```

3. **Read the files**
   - Both `<shortdesc>`s say what the file is for: a reader who opens it
     in the editor, or an agent that finds it in a search, learns at once
     that it is a warehouse and is not published.
   - The step `save-project` uses `<keyword keyref="project-extension"/>`
     for `.aup3`, the key defined in stage 09; a warehouse step should not
     hard-code product facts either.
   - `<ul id="noise-reduction-settings">` has ids on its first and last
     items, `nr-first` and `nr-last`, so a topic can pull the three items as
     a range.
   - The last two steps are the push. `<step conaction="pushbefore">` is
     the content to insert; the `<step conaction="mark" conref="…">` that
     follows names, by an ordinary conref, the step in the recording task
     to insert it before. The pushed step has content; the marker has an
     empty `<cmd/>` because a step must have one to validate.
::::

## Step 2: Put the warehouses in the map

::::steps
1. **Edit `audacity-guide.ditamap`**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -9,6 +9,9 @@
    
      <mapref href="keydefs-product.ditamap"/>
      <keydef keys="start-here" href="topics/what-is-audacity.dita"/>
   +  <!-- Warehouses: pulled in by conref and conkeyref, never pages of their own. -->
   +  <keydef keys="common-notes" href="shared/common-notes.dita"/>
   +  <topicref href="shared/common-steps.dita" processing-role="resource-only"/>
    
      <topichead navtitle="Getting started">
        <topicref href="topics/what-is-audacity.dita" collection-type="family">
   ```

2. **Read the two forms**
   - `<keydef keys="common-notes" href="shared/common-notes.dita"/>` gives
     the notes warehouse a key. A keydef is resource-only, so the topic is
     part of the map's key space and is processed, but is not a page.
   - `<topicref href="shared/common-steps.dita" processing-role="resource-only"/>`
     does the same by path. `@processing-role="resource-only"` is what a
     keydef sets by default: the topic is read, so its conref push can act,
     but it gets no entry in the table of contents and no output page.
   - The warehouses need to be in the map. A conref by path would resolve
     without it, but a conkeyref needs the key, and a conref push acts
     only when DITA-OT processes the pushing topic as part of the
     publication.
::::

## Step 3: Pull the content into the tasks

::::steps
1. **Edit `topics/trimming-audio.dita`**
   The save step becomes a reference to the warehouse.

   ```diff title="topics/trimming-audio.dita"
   --- a/topics/trimming-audio.dita
   +++ b/topics/trimming-audio.dita
   @@ -34,11 +34,8 @@
              <p>If you hear a click at the join, select a few milliseconds around it and apply <menucascade><uicontrol>Effect</uicontrol><uicontrol>Fade In</uicontrol></menucascade> or <uicontrol>Fade Out</uicontrol>.</p>
            </info>
          </step>
   -      <step>
   -        <cmd>Save the project with <menucascade><uicontrol>File</uicontrol><uicontrol>Save Project</uicontrol><uicontrol>Save Project</uicontrol></menucascade>.</cmd>
   -        <info>
   -          <p><keyword keyref="product-name"/> saves the project as a single <filepath>.aup3</filepath> file in the folder you choose; the <wintitle>Save Project</wintitle> dialog suggests your Documents folder.</p>
   -        </info>
   +      <step conref="../shared/common-steps.dita#common-steps/save-project">
   +        <cmd/>
          </step>
        </steps>
        <result>
   ```

2. **Edit `topics/recording-your-first-track.dita`**
   The tip comes in by key, the save step by path, and the play-back step
   gets the `@id` the push targets.

   ```diff title="topics/recording-your-first-track.dita"
   --- a/topics/recording-your-first-track.dita
   +++ b/topics/recording-your-first-track.dita
   @@ -9,7 +9,7 @@
          <p>This task walks you through making your first audio recording.
          You need a microphone connected to your computer.
          If you are new to audio terms such as sample rate and clipping, read <xref href="what-is-digital-audio.dita"/> first.</p>
   -      <note type="tip">For best results, record in a quiet room and position the microphone 15 to 30 cm from your mouth.</note>
   +      <note conkeyref="common-notes/quiet-room-tip"/>
        </context>
        <steps>
          <step>
   @@ -55,9 +55,12 @@
          <step>
            <cmd>Click <uicontrol>Stop</uicontrol> when you are finished.</cmd>
          </step>
   -      <step>
   +      <step id="play-back">
            <cmd>Click <uicontrol>Play</uicontrol> to listen to your recording.</cmd>
          </step>
   +      <step conref="../shared/common-steps.dita#common-steps/save-project">
   +        <cmd/>
   +      </step>
        </steps>
        <result>
          <p>You now have a recorded audio track in your project.
   ```

3. **Edit `topics/removing-background-noise.dita`**
   The warning by key, the select step by path, and the settings as a
   range.

   ```diff title="topics/removing-background-noise.dita"
   --- a/topics/removing-background-noise.dita
   +++ b/topics/removing-background-noise.dita
   @@ -8,8 +8,7 @@
        <context>
          <p>Background noise such as hum from electronics, hiss from a microphone or fan noise degrades a recording.
          The Noise Reduction effect learns what the noise sounds like from a section that contains nothing else, then removes it everywhere.</p>
   -      <note type="warning">This effect changes the audio data.
   -      Save the project first; you can undo with <uicontrol>Ctrl+Z</uicontrol> (Windows and Linux) or <uicontrol>Cmd+Z</uicontrol> (macOS).</note>
   +      <note conkeyref="common-notes/backup-warning"/>
        </context>
        <steps>
          <step>
   @@ -19,8 +18,8 @@
              You need at least half a second of noise-only audio.</p>
            </info>
          </step>
   -      <step>
   -        <cmd>Select that section by clicking and dragging in the waveform.</cmd>
   +      <step conref="../shared/common-steps.dita#common-steps/select-region">
   +        <cmd/>
          </step>
          <step>
            <cmd>Choose <menucascade><uicontrol>Effect</uicontrol><uicontrol>Noise Removal and Repair</uicontrol><uicontrol>Noise Reduction</uicontrol></menucascade>, then click <uicontrol>Get Noise Profile</uicontrol>.</cmd>
   @@ -36,9 +35,7 @@
            <info>
              <p>Recommended starting settings:</p>
              <ul>
   -            <li>Noise reduction: 12 dB</li>
   -            <li>Sensitivity: 6</li>
   -            <li>Frequency smoothing: 3</li>
   +            <li conref="../shared/common-notes.dita#common-notes/nr-first" conrefend="../shared/common-notes.dita#common-notes/nr-last"/>
              </ul>
              <note type="caution">Too much noise reduction makes voices sound metallic or underwater.
              Start moderate, increase gradually, and use <uicontrol>Preview</uicontrol> before applying.</note>
   ```

4. **Read the references**
   - `@conref="../shared/common-steps.dita#common-steps/save-project"` is
     the same `file#topic/element-id` form as an `<xref>`, relative to the
     referencing topic, hence `../shared/`. The whole element, `<cmd>`,
     `<info>` and all, comes from the target.
   - `@conkeyref="common-notes/backup-warning"` is `key/element-id`: the
     key from the map names the topic, so the path is not in the topic.
     Move the warehouse and only the keydef changes.
   - `@conrefend` on the `<li>` makes the reference a range: every element
     from the `@conref` target to the `@conrefend` target, in this case all
     three list items, replaces the one `<li>`. Both targets must be
     siblings of the same type.
   - **The `<cmd/>` placeholder.** A conref'd `<step>` still has to validate
     as a `<step>` before it is resolved, and the DTD says a step contains a
     `<cmd>`. So the referencing step carries an empty `<cmd/>` that the
     conref replaces. The same rule applies to any element whose content
     model requires a child. A `<note conkeyref="…"/>` needs nothing,
     because a note may be empty.
   - The referencing element and the target must be the same element type,
     and the target may not need anything the referencing context does not
     allow. A `<step>` into `<steps>`, a `<note>` into `<context>`, `<li>`s
     into a `<ul>`.
::::

## Step 4: Update the README, run the gate and check what was published

::::steps
1. **Change the "You are on" line and the layout**

   ````diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 09 — keys**: product variables and indirect links through a key space.
   +You are on **stage 10 — reuse**: conref, conkeyref, conref ranges and conref push from a shared warehouse.
    
    ## Stages
    
   @@ -42,6 +42,7 @@ project.json          DITA-OT project file: the deliverables this guide ships
    .dogsbay/config.xml   shared editor project settings (project type, framework, default map and deliverable, format style)
    scripts/              the gate
    topics/               topics
   +shared/               warehouses: content pulled in by conref (common-steps, common-notes)
    images/               illustrations referenced by <image>
    ```
    
   ````

2. **Format and check**
   The format command covers `shared/` too.

   ```bash
   dogsbay-xml format -i topics/*.dita shared/*.dita
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   13 file(s): 13 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == build  project.json -> /tmp/check-stage-357688 ==
   all deliverables built

   STAGE OK
   ```

   `project-health` resolves every `@conref` and `@conkeyref` to an element
   that exists and checks that each `conaction="mark"` finds its target.
   Rename the play-back step's id and it reports both a broken element id
   and, under *Conref pushes*, `push target id 'playback' not found`.

3. **Check the output**
   The build folder has one page per topic in the table of contents and
   nothing for the warehouses:

   ```bash
   find /tmp/check-stage-357688 -name '*.html' | sort
   ```

   ```
   /tmp/check-stage-357688/out/full/index.html
   /tmp/check-stage-357688/out/full/topics/exporting-audio.html
   /tmp/check-stage-357688/out/full/topics/installing-audacity.html
   /tmp/check-stage-357688/out/full/topics/preparing-to-record.html
   /tmp/check-stage-357688/out/full/topics/recording-your-first-track.html
   /tmp/check-stage-357688/out/full/topics/removing-background-noise.html
   /tmp/check-stage-357688/out/full/topics/supported-audio-formats.html
   /tmp/check-stage-357688/out/full/topics/trimming-audio.html
   /tmp/check-stage-357688/out/full/topics/what-is-audacity.html
   /tmp/check-stage-357688/out/full/topics/what-is-digital-audio.html
   ```

   No `common-steps.html`, no `common-notes.html`. Open
   `recording-your-first-track.html`: the step "Turn the playback volume
   down before you listen" sits before "Click Play", pushed there from the
   warehouse, and the task's own file never mentions it.
::::

Two mistakes are worth making here. First, a `@conkeyref` through a key that
does not exist. Change `common-notes/backup-warning` in the noise task to
`common-note/backup-warning` and run the gate:

```
== project-health  /home/you/audacity-guide ==
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Undefined keys (1):
  /home/you/audacity-guide/topics/removing-background-noise.dita:11  key 'common-note' not defined
Unused keys (2):
  start-here  [/home/you/audacity-guide/audacity-guide.ditamap:11]
  digital-audio  [/home/you/audacity-guide/audacity-guide.ditamap:18]

Summary
  Undefined keys                1
  Unused keys                   2
FAIL: project-health found issues

== build  project.json -> /tmp/check-stage-357688 ==
Error: file:/home/you/audacity-guide/topics/removing-background-noise.dita:11:53: [DOTJ046E] The @conkeyref attribute value 'common-note/backup-warning' cannot be resolved because it does not contain a key or the key is not defined. Using the @conref attribute as fallback if it exists.
FAIL: DITA-OT build reported errors (full log: /tmp/check-stage-357688.log)

STAGE FAILED
```

DITA-OT resolves `@conkeyref` only through the root map's key space, and an
undefined key is a build error, `DOTJ046E`, not a warning: the note would
be missing from the page. The health check reports the same key a step
earlier.

Second, drop the placeholder. Replace the trimming task's
`<step conref="…"><cmd/></step>` with a self-closing
`<step conref="…"/>`, and validation fails before anything is resolved:

```
== validate-project  /home/you/audacity-guide ==
/home/you/audacity-guide/topics/trimming-audio.dita:
  37:77  error: The content of element type "step" is incomplete, it must match "((note|hazardstatement)*,cmd,(choices|choicetable|info|itemgroup|stepxmp|substeps|tutorialinfo)*,stepresult?,steptroubleshooting?)".
13 file(s): 12 valid, 1 invalid.
FAIL: validation errors
```

## What you learned

- `@conref="file#topic/id"` replaces an element with a copy of another;
  `@conkeyref="key/id"` does the same through the key space.
- `@conrefend` turns a reference into a range of sibling elements.
- Conref push: `conaction="pushbefore"` (or `pushafter`, `pushreplace`) on
  the content, then `conaction="mark"` with a `@conref` to the target, from
  the warehouse's side.
- A referencing element must still validate, so a conref'd `<step>` keeps
  an empty `<cmd/>`.
- Warehouses go in the map as a `<keydef>` or with
  `@processing-role="resource-only"`, and are not published.
- An undefined key behind a `@conkeyref` is a DITA-OT error, `DOTJ046E`.

## Where to go next

:::cards
- **[Stage 11: Glossary](/part-2-maps/stage-11-glossary)** {icon="arrow-right"}
  Glossary entries, a group of units with abbreviations, and terms in the
  text bound to their definitions by key.

- **[Compare 09 to 10 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/09-keys...tutorial/10-reuse)** {icon="github"}
  Exactly what this stage added.
:::
