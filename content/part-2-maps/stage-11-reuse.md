---
title: "Stage 11: Reuse"
description: Write steps, notes and a list once in a shared topic, then pull them into tasks by conref, conkeyref and a conref range, and push a step in.
type: tutorial
---

# Stage 11: Reuse

Store common steps, notes, and list items in two topics under `shared/`.
Reuse those elements with `@conref`, `@conkeyref`, and `@conrefend`.
Use conref push to insert a step at a target in another topic.

Include the shared topics in the map as resources. DITA-OT processes their
content without publishing separate pages for them.

**Time:** about 30 minutes.
**You need:** stage 10 complete.

For the core course, focus on pulling steps with `conref` and notes with
`conkeyref`. The range and push examples are optional. See
[Choose a reuse method](/reference/choosing-reuse) before adding shared content.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: Write the shared topics

::::steps
1. **Create `shared/common-steps.dita`**
   A task whose steps exist to be referenced. Every reusable element has an
   `@id`.

   In the **Explorer**, right-click `my-audacity-guide`, choose
   **New Folder**, and enter `shared`. Then right-click the `shared`
   folder, choose **New File**, enter `common-steps.dita`, and choose the
   **Task** template. Replace the title placeholder, type the short
   description, and build the steps from the template's first `<step>`.
   If you use another editor, create the folder and the file, and type the
   finished listing.

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
             <p>Click and drag in the waveform to select a region; the selection is highlighted.</p>
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
   with `<body>` instead of `<conbody>` or `<taskbody>`; a shared topic of
   notes has no reason to be a concept or a task.

   Create `common-notes.dita` in the `shared` folder, and choose the
   **Topic** template:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE topic PUBLIC "-//OASIS//DTD DITA Topic//EN" "topic.dtd">

   <topic id="common-notes">
     <title>Topic Title</title>
     <shortdesc></shortdesc>
     <body>
       <p></p>
     </body>
   </topic>
   ```

   Replace the title placeholder and type the short description. The body
   starts with the two notes, so add them before the empty `<p></p>`, type
   the paragraph in it, and add the list after it. If you use another
   editor, create the file and type the finished listing. The finished
   topic:

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
     that it is a shared topic and is not published.
   - The step `select-region` is generic: it says how to select a region
     and nothing about which region. That is why it fits the noise task,
     which uses it to select a noise-only section. The noise task selects
     the whole track in a later step of its own.
   - The step `save-project` uses `<keyword keyref="project-extension"/>`
     for `.aup3`, the key defined in stage 10; a shared topic step should not
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

## Step 2: Put the shared topics in the map

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
     the notes shared topic a key. A keydef is resource-only, so the topic is
     part of the map's key space and is processed, but is not a page.
   - `<topicref href="shared/common-steps.dita" processing-role="resource-only"/>`
     does the same by path. `@processing-role="resource-only"` is what a
     keydef sets by default: the topic is read, so its conref push can act,
     but it gets no entry in the table of contents and no output page.
   - The shared topics need to be in the map. A conref by path would resolve
     without it, but a conkeyref needs the key, and a conref push acts
     only when DITA-OT processes the pushing topic as part of the
     publication.
::::

## Step 3: Pull the content into the tasks

::::steps
1. **Edit `topics/trimming-audio.dita`**
   The save step becomes a reference to the shared topic.

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
   @@ -55,8 +55,11 @@
          <step>
            <cmd>Click <uicontrol>Stop</uicontrol> when you are finished.</cmd>
          </step>
   -      <step>
   +      <step id="play-back">
            <cmd>Click <uicontrol>Play</uicontrol> to listen to your recording.</cmd>
   +      </step>
   +      <step conref="../shared/common-steps.dita#common-steps/save-project">
   +        <cmd/>
          </step>
        </steps>
        <result>
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
     Move the shared topic and only the keydef changes.
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

## Step 4: Check your work and check what was published

::::steps
1. **Format and check your work**
   Format the changed topics: choose **XML** > **Format** in each one and
   save it, or choose **Project** > **Project Tools** > **Format Project**
   to format every file at once. On the command line, the format command
   covers `shared/` too:

   ```bash
   dogsbay-xml format -i topics/*.dita shared/*.dita
   ```

   Then check the project. In the editor, choose **Project** >
   **Check Project** and read the result in the **Project Validation**
   panel. From the command line, run:

   ```bash
   dogsbay-xml check .
   ```

   Example output:

   ```
   health   clean, with warnings
     unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:11] — nothing references it
     unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:18] — nothing references it
   build    full                 ok  /home/you/my-audacity-guide/out/full
   output   clean (full)
   Ready: the project is healthy, every deliverable built, and the output of full holds together. 2 unused keys above: worth knowing, and not treated as failures.
   ```

   `start-here` and `digital-audio` are defined for a later stage: stage 18
   uses them in a new topic.

   The health stage resolves every `@conref` and `@conkeyref` to an element
   that exists and checks that each `conaction="mark"` finds its target.
   Rename the play-back step's id, for example to `playback`, and check
   your work. The check stops at health and names the reference that no
   longer resolves and the push that no longer finds its target. Example
   output:

   ```
   health   NOT CLEAN
     /home/you/my-audacity-guide/shared/common-steps.dita:28  @conref="../topics/recording-your-first-track.dita#recording-your-first-track/play-back" — element id 'play-back' not found in recording-your-first-track.dita
     /home/you/my-audacity-guide/shared/common-steps.dita:28  <step> — push target id 'play-back' not found in recording-your-first-track.dita
     (run project-health for the full report)
     unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:11] — nothing references it
     unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:18] — nothing references it
   Not ready: the project itself has faults. The build and the built output were not checked.
   ```

   The **Project Validation** panel, or `dogsbay-xml project-health .`,
   lists the two problems under `Conref pushes` and `Broken element ids`:

   ```
   Conref pushes (1):
     /home/you/my-audacity-guide/shared/common-steps.dita:28  <step> push target id 'play-back' not found in recording-your-first-track.dita
   Broken element ids (1):
     /home/you/my-audacity-guide/shared/common-steps.dita:28  @conref="../topics/recording-your-first-track.dita#recording-your-first-track/play-back" — element id 'play-back' not found in recording-your-first-track.dita
   ```

   The diagnostics name the id that the push looks for, `play-back`, not the
   new id. Restore the id and check again.

2. **Check the output**
   The check builds the guide into `out/full/`. The folder has one page
   per topic in the table of contents and nothing for the shared topics:

   ```bash
   find out/full -name '*.html' | sort
   ```

   ```
   out/full/index.html
   out/full/topics/exporting-audio.html
   out/full/topics/installing-audacity.html
   out/full/topics/preparing-to-record.html
   out/full/topics/recording-your-first-track.html
   out/full/topics/removing-background-noise.html
   out/full/topics/supported-audio-formats.html
   out/full/topics/trimming-audio.html
   out/full/topics/what-is-audacity.html
   out/full/topics/what-is-digital-audio.html
   ```

   The folder does not contain `common-steps.html` or `common-notes.html`. Open
   `recording-your-first-track.html`: the step "Turn the playback volume
   down before you listen" sits before "Click Play", pushed there from the
   shared topic, and the task's own file never mentions it.
::::

Test two errors. First, use a `@conkeyref` with an undefined key. Change `common-notes/backup-warning` in the noise task to
`common-note/backup-warning` and check your work. The check stops at
health. Example output:

```
health   NOT CLEAN
  /home/you/my-audacity-guide/topics/removing-background-noise.dita:11  @conkeyref="common-note/backup-warning" — key 'common-note' not defined
  (run project-health for the full report)
  unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:11] — nothing references it
  unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:18] — nothing references it
Not ready: the project itself has faults. The build and the built output were not checked.
```

The **Project Validation** panel, or `dogsbay-xml project-health .`, gives
the full report:

```
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Undefined keys (1):
  /home/you/my-audacity-guide/topics/removing-background-noise.dita:11  key 'common-note' not defined
Unused keys (2):
  start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:11]
  digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:18]

Summary
  Undefined keys                1
  Unused keys                   2
```

Because health fails, the check does not build. To see what DITA-OT makes
of the same fault, build the deliverable directly:

```bash
dogsbay-xml build . full
```

The first lines of the example output:

```
full (html5) → /home/you/my-audacity-guide/out/full: FAILED — 1 error(s)
    [DOTJ046E] file:/home/you/my-audacity-guide/topics/removing-background-noise.dita:11 The @conkeyref attribute value 'common-note/backup-warning' cannot be resolved because it does not contain a key or the key is not defined. Using the @conref attribute as fallback if it exists.
```

DITA-OT resolves `@conkeyref` only through the root map's key space, and an
undefined key produces build error `DOTJ046E`: the note would
be missing from the page. The health check reports the same key before
anything is built.

Undo the key change. Next, remove the placeholder. Replace the trimming task's
`<step conref="…"><cmd/></step>` with a self-closing
`<step conref="…"/>`, and check your work. The file is no longer valid,
and the check names it with the line, column, and message. Example output:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/topics/trimming-audio.dita
    37:77  The content of element type "step" is incomplete, it must match "((note|hazardstatement)*,cmd,(choices|choicetable|info|itemgroup|stepxmp|substeps|tutorialinfo)*,stepresult?,steptroubleshooting?)".
  (run project-health for the full report)
  unused key: start-here  [/home/you/my-audacity-guide/audacity-guide.ditamap:11] — nothing references it
  unused key: digital-audio  [/home/you/my-audacity-guide/audacity-guide.ditamap:18] — nothing references it
Not ready: the project itself has faults. The build and the built output were not checked.
```

The **Project Validation** panel, or `dogsbay-xml project-health .`, gives
the full report. It begins:

```
Invalid files (1 of 13):
  /home/you/my-audacity-guide/topics/trimming-audio.dita:
    37:77  error: The content of element type "step" is incomplete, it must match "((note|hazardstatement)*,cmd,(choices|choicetable|info|itemgroup|stepxmp|substeps|tutorialinfo)*,stepresult?,steptroubleshooting?)".
```

Validation fails before anything is resolved.

Undo the change and check again. Confirm that the check reports `Ready`
before you continue.

## What you learned

- `@conref="file#topic/id"` replaces an element with a copy of another;
  `@conkeyref="key/id"` does the same through the key space.
- `@conrefend` turns a reference into a range of sibling elements.
- Conref push: `conaction="pushbefore"` (or `pushafter`, `pushreplace`) on
  the content, then `conaction="mark"` with a `@conref` to the target, from
  the shared topic's side.
- A referencing element must still validate, so a conref'd `<step>` keeps
  an empty `<cmd/>`.
- Shared topics go in the map as a `<keydef>` or with
  `@processing-role="resource-only"`, and are not published.
- An undefined key behind a `@conkeyref` is a DITA-OT error, `DOTJ046E`.

## Next lesson

**Checkpoint:** `tutorial/11-reuse`. If you use Git, commit your work.

Continue with [Stage 12: glossary](/part-2-maps/stage-12-glossary).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/10-keys...tutorial/11-reuse).
