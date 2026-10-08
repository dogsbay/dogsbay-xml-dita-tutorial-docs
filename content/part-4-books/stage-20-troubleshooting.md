---
title: "Stage 20: Troubleshooting"
description: Write a troubleshooting topic with a condition, causes and remedies, and turn a warning in the recording task into a trouble note that points at it.
type: tutorial
---

# Stage 20: Troubleshooting

Create *The recording is silent* as a `<troubleshooting>` topic. Describe
the symptom in `<condition>`, then pair each likely cause with a remedy.
One of the three solutions applies only to macOS.

Link to the topic from a `<note type="trouble">` in the recording task
and add it to six maps. This extends the task-level troubleshooting
elements introduced in stage 06.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 25 minutes.
**You need:** stage 19 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: The troubleshooting topic

::::steps
1. **Create `topics/recording-is-silent.dita`**
   Right-click `topics` in the **Explorer**, choose **New File**, enter
   `recording-is-silent.dita`, and choose the **Troubleshooting**
   template. Replace the template's content with this topic:

   ```xml title="topics/recording-is-silent.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE troubleshooting PUBLIC "-//OASIS//DTD DITA Troubleshooting//EN" "troubleshooting.dtd">
   
   <troubleshooting id="recording-is-silent">
     <title>The recording is silent</title>
     <shortdesc>You pressed Record but the waveform is a flat line and playback is silent: the input device, its level or its permissions are wrong.</shortdesc>
     <prolog>
       <author type="creator">The Audacity tutorial team</author>
       <critdates>
         <created date="2026-02-20"/>
       </critdates>
       <metadata>
         <audience type="user" experiencelevel="novice"/>
         <category>troubleshooting</category>
         <keywords>
           <keyword>silent recording</keyword>
           <keyword>flat line</keyword>
           <keyword>microphone</keyword>
           <indexterm>troubleshooting<indexterm>silent recording</indexterm></indexterm>
           <indexterm>microphone<indexterm>not detected</indexterm></indexterm>
         </keywords>
       </metadata>
     </prolog>
     <troublebody>
       <condition>
         <title>Condition</title>
         <p>You click <uicontrol>Record</uicontrol>, the track appears, but the waveform stays a flat line.
         Playing the track produces no sound.</p>
       </condition>
       <troubleSolution>
         <cause>
           <title>The wrong input device is selected</title>
           <p>The <uicontrol>Device Toolbar</uicontrol> is set to a device that is not connected, or to a line input rather than the microphone.</p>
         </cause>
         <remedy>
           <title>Select the microphone</title>
           <steps>
             <step>
               <cmd>Click <uicontrol>Stop</uicontrol>.</cmd>
             </step>
             <step>
               <cmd>In the <uicontrol>Device Toolbar</uicontrol>, open the recording device list and choose your microphone.</cmd>
             </step>
             <step>
               <cmd>Choose <menucascade><uicontrol>Transport</uicontrol><uicontrol>Rescan Audio Devices</uicontrol></menucascade> if the microphone is missing from the list.</cmd>
             </step>
           </steps>
         </remedy>
       </troubleSolution>
       <troubleSolution>
         <cause>
           <title>The input level is zero</title>
           <p>The recording volume slider is at its minimum, so the device is selected but nothing is captured.</p>
         </cause>
         <remedy>
           <title>Raise the recording volume</title>
           <steps>
             <step>
               <cmd>Drag the recording volume slider (the one with the microphone icon) to about three quarters.</cmd>
             </step>
             <step>
               <cmd>Click the recording meter to start monitoring, then speak; the meter should move.</cmd>
             </step>
           </steps>
         </remedy>
       </troubleSolution>
       <troubleSolution platform="mac">
         <cause>
           <title>macOS has not granted microphone access</title>
           <p>The first time an application asks to use the microphone, macOS asks for permission.
           If the request was refused, <keyword keyref="product-name"/> records silence.</p>
         </cause>
         <remedy>
           <title>Allow microphone access</title>
           <steps-informal>
             <p>Open <menucascade><uicontrol>System Settings</uicontrol><uicontrol>Privacy and Security</uicontrol><uicontrol>Microphone</uicontrol></menucascade>, switch on <keyword keyref="product-name"/>, then restart it.</p>
           </steps-informal>
         </remedy>
       </troubleSolution>
     </troublebody>
     <related-links>
       <link href="recording-your-first-track.dita"/>
       <link href="preparing-to-record.dita"/>
     </related-links>
   </troubleshooting>
   ```

2. **Read the structure**

   - The DOCTYPE is `-//OASIS//DTD DITA Troubleshooting//EN` with
     `troubleshooting.dtd`; the body is `<troublebody>`.
   - `<condition>` describes the symptom the reader sees. It takes a
     `<title>` and body elements.
   - Each `<troubleSolution>` pairs one `<cause>` with one `<remedy>`, in
     that order; the DTD does not let a remedy come first. A topic may
     have as many solutions as it has plausible causes; put the most
     likely first.
   - A `<remedy>` must say what to do as `<steps>`, `<steps-unordered>`
     or `<steps-informal>`. The first two remedies are proper `<steps>`,
     the same element as in a task; the macOS remedy is one paragraph in
     `<steps-informal>`. A remedy with only a `<p>` does not validate,
     as the demo at the end shows.
   - The third solution carries `platform="mac"`. The profiling
     attributes from stage 14 work on `<troubleSolution>` as on any
     element, so the Windows and Linux builds drop the macOS solution and
     `validate-conditions` checks the value against the subject scheme.
   - The `<prolog>` follows the stage 13 policy: a creator, a created
     date, an audience, a category and keywords, with two nested
     `<indexterm>`s so the topic is in the book's index under both
     *troubleshooting* and *microphone*.

3. **Read the trouble note**
   The recording task changes one note:

   ```diff title="topics/recording-your-first-track.dita"
   --- a/topics/recording-your-first-track.dita
   +++ b/topics/recording-your-first-track.dita
   @@ -63,8 +63,8 @@
            <cmd>Click <uicontrol>Record</uicontrol>, or press <uicontrol>R</uicontrol>, to start recording.</cmd>
            <info>
              <p>A blue waveform appears as <keyword keyref="product-name"/> captures audio from your microphone, like <xref href="what-is-digital-audio.dita#what-is-digital-audio/fig-waveform"/>.</p>
   -          <note type="warning">A flat line instead of a waveform means the microphone is not selected correctly.
   -          Click <uicontrol>Stop</uicontrol> and check the recording device.</note>
   +          <note type="trouble">A flat line instead of a waveform means nothing is being captured.
   +          See <xref keyref="silent" href="recording-is-silent.dita"/>.</note>
            </info>
          </step>
          <step>
   ```

   `<note type="trouble">` is the note type for "if this goes wrong, look
   here". The HTML5 transform labels it *Trouble:*. It replaces a
   `warning`, which is for harm to people or data, and the fix moves out
   of the note into the topic it links to.

   The `<xref>` carries both a `keyref` and an `href`:
   `<xref keyref="silent" href="recording-is-silent.dita"/>`. When the
   map that publishes the topic defines the key `silent`, the key wins and
   the link goes where the key points. When no map defines the key, the
   processor uses the `href` instead. The next step explains why this
   project needs both.
::::

## Step 2: The topic in every map

::::steps
1. **Edit `audacity-guide.ditamap`**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -53,6 +53,10 @@
        </topicset>
      </topichead>
    
   +  <topichead navtitle="Troubleshooting" id="troubleshooting">
   +    <topicref href="topics/recording-is-silent.dita" keys="silent"/>
   +  </topichead>
   +
      <topichead navtitle="Podcast production" audience="podcaster">
        <topicref href="topics/podcast-production-workflow.dita" keys="podcast-workflow"/>
      </topichead>
   ```

2. **Edit `audacity-book.ditamap`**

   ```diff title="audacity-book.ditamap"
   --- a/audacity-book.ditamap
   +++ b/audacity-book.ditamap
   @@ -65,6 +65,7 @@
        <chapter href="topics/podcast-production-workflow.dita" keys="podcast-workflow"/>
      </part>
    
   +  <appendix href="topics/recording-is-silent.dita"/>
      <appendix href="topics/supported-audio-formats.dita" keys="formats"/>
      <appendix href="topics/effects-reference.dita" keys="effects"/>
      <appendix href="topics/effect-presets.dita"/>
   ```

3. **Edit `beginner-guide.ditamap`**

   ```diff title="beginner-guide.ditamap"
   --- a/beginner-guide.ditamap
   +++ b/beginner-guide.ditamap
   @@ -22,6 +22,9 @@
        <topicref href="topics/trimming-audio.dita"/>
        <topicref href="topics/exporting-audio.dita"/>
      </topichead>
   +  <topichead navtitle="Troubleshooting">
   +    <topicref href="topics/recording-is-silent.dita" keys="silent"/>
   +  </topichead>
      <topichead navtitle="Quick reference">
        <!-- The same source topic, published under a different file name in this guide. -->
        <topicref href="topics/supported-audio-formats.dita" keys="formats" copy-to="topics/formats-quick-reference.dita"/>
   ```

4. **Edit `podcaster-guide.ditamap`**

   ```diff title="podcaster-guide.ditamap"
   --- a/podcaster-guide.ditamap
   +++ b/podcaster-guide.ditamap
   @@ -26,6 +26,9 @@
        <topicsetref href="audacity-guide.ditamap#cleanup"/>
        <topicref href="topics/exporting-audio.dita"/>
      </topichead>
   +  <topichead navtitle="Troubleshooting">
   +    <topicref href="topics/recording-is-silent.dita"/>
   +  </topichead>
      <topichead navtitle="Podcast production">
        <topicref href="topics/podcast-production-workflow.dita" keys="podcast-workflow"/>
      </topichead>
   ```

5. **Edit `installation-variants.ditamap`**

   ```diff title="installation-variants.ditamap"
   --- a/installation-variants.ditamap
   +++ b/installation-variants.ditamap
   @@ -35,6 +35,8 @@
            <dvrKeyscopePrefix>linux.</dvrKeyscopePrefix>
          </ditavalmeta>
        </ditavalref>
   -    <topicref href="topics/recording-your-first-track.dita" keys="first-track"/>
   +    <topicref href="topics/recording-your-first-track.dita" keys="first-track">
   +      <topicref href="topics/recording-is-silent.dita" keys="silent"/>
   +    </topicref>
      </topicref>
    </map>
   ```

6. **Edit `audacity-collection.ditamap`**
   Add the key to the collection's root scope, beside `formats` from stage
   17, so the root-scope copy of the recording task resolves its trouble
   note too.

   ````diff title="audacity-collection.ditamap"
   --- a/audacity-collection.ditamap
   +++ b/audacity-collection.ditamap
   @@ -12,6 +12,7 @@
      <mapref href="keydefs-glossary.ditamap"/>
      <keydef keys="common-notes" href="shared/common-notes.dita"/>
      <keydef keys="formats" href="topics/supported-audio-formats.dita"/>
   +  <keydef keys="silent" href="topics/recording-is-silent.dita"/>
      <!-- Each guide defines its own "start-here" key. Combined flat they would
           collide (first definition wins). A key scope on each mapref keeps them
           apart: userguide.start-here, beginner.start-here, podcaster.start-here. -->
   ````

7. **Read the six references**

   - The full guide gives the topic a `<topichead>` of its own, with an
     `id` so a later stage can reference the branch, and the key `silent`.
   - In the book it is an appendix, before the three reference appendixes.
   - The beginner and podcaster guides put it in a *Troubleshooting*
     head of their own. The beginner guide also defines the key `silent`.
   - The installation variants map nests it under the recording task, so
     each of the three platform branches from stage 16 carries its own
     filtered copy: the Mac branch keeps the macOS solution, the other
     two drop it. The `install-variants` build writes the copies as
     `topics/win-recording-is-silent.html`,
     `topics/mac-recording-is-silent.html` and
     `topics/recording-is-silent-linux.html`.
   - The nested reference defines the key `silent` inside the branch.
     Each branch has its own key scope (`win.`, `mac.` and `linux.`, from
     the `<dvrKeyscopePrefix>` in stage 16), so each branch has its own
     `silent` key, which points at that branch's copy. The `keyref` in the
     trouble note resolves within the scope of the copy that contains it:
     `win-recording-your-first-track.html` links to
     `win-recording-is-silent.html`, the macOS copy to
     `mac-recording-is-silent.html`, and the Linux copy to
     `recording-is-silent-linux.html`. A plain `href` cannot do this. It
     resolves to one copy of the troubleshooting topic for every branch,
     so the Windows and Linux readers would land on the macOS page.
   - The book and the podcaster guide do not define `silent`, so the
     `href` fallback applies there. Both contain the topic, so the link
     resolves in every deliverable.
   - The collection defines `silent` in its root scope. The guides it
     aggregates keep their own `silent` keys in their scopes.
::::

## Step 3: Check your work

::::steps
1. **Format and build**

   Eight files have changes: the new topic, the recording task, and six
   maps. Save them all. Format the changed files: choose **XML** > **Format** in each changed
   file and save it, or choose **Project** > **Project Tools** >
   **Format Project** to format every file at once. From the command line,
   run:

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   ```

   Before you build, predict how many copies of the troubleshooting topic
   the `install-variants` build makes, and which of them keep the macOS
   solution. Then choose **Project** > **Build Deliverables...** and click
   **Build All** to build all eight deliverables. **Project** > **Check
   Project** also builds every deliverable, and checks the project and the
   built pages as well. Read its result in the **Project Validation**
   panel. From the command line, the same full check is:

   ```bash
   dogsbay-xml check .
   ```

   Example output:

   ```
   health   clean
   build    full                 ok  /home/you/my-audacity-guide/out/full
     5 note(s) — run with --verbose to see them
   build    beginner-mac         ok  /home/you/my-audacity-guide/out/beginner-mac
   build    beginner-windows     ok  /home/you/my-audacity-guide/out/beginner-windows
   build    podcaster-linux      ok  /home/you/my-audacity-guide/out/podcaster-linux
     1 note(s) — run with --verbose to see them
   build    review               ok  /home/you/my-audacity-guide/out/review
     4 note(s) — run with --verbose to see them
   build    install-variants     ok  /home/you/my-audacity-guide/out/install-variants
     6 note(s) — run with --verbose to see them
   build    collection           ok  /home/you/my-audacity-guide/out/collection
     5 note(s) — run with --verbose to see them
   build    book-pdf             ok  /home/you/my-audacity-guide/out/book-pdf
     WARN  /home/you/my-audacity-guide/audacity-book.ditamap  PDF rendering reported 13 warnings (9 The following feature isn't implemented by Apache FOP, yet: table-layout=… (on fo:table) (…, 1 The contents of fo:external-graphic line n exceed the available area in the inline-progres…, 1 The contents of fo:instream-foreign-object line n exceed the available area in the inline-…, and 2 other kinds)
     6 note(s) — run with --verbose to see them
   output   wrote a file, no pages to check links in book-pdf
   output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection)
   Ready: the project is healthy, every deliverable built, and the output of full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection holds together.
   ```

   The checks on the `install-variants` pages found no problems: each
   platform copy of the recording task links to the troubleshooting copy
   in its own branch.

2. **Read the output**
   In the **Explorer**, open `out/install-variants/topics/`. It has three
   filtered copies of the topic, one per branch, beside that branch's copy
   of the recording task. It also has an unfiltered
   `recording-is-silent.html`, like the unprefixed pages in stage 16. Open `win-recording-is-silent.html` and choose
   **View** > **Preview in Tab**. The symptom comes first, under
   *Condition*. Each cause follows as a heading, with its remedy under
   it.

   To see which copies kept the macOS solution, run these commands in the
   **Terminal** panel:

   ```bash
   cd out/install-variants/topics
   grep -c "microphone access" *-recording-is-silent.html recording-is-silent-linux.html
   ```

   Example output:

   ```
   mac-recording-is-silent.html:2
   win-recording-is-silent.html:0
   recording-is-silent-linux.html:0
   ```

   Only the `mac-` copy has matching lines. The Windows and Linux filters
   exclude content marked `platform="mac"`. To see where each copy of the
   trouble note links, run:

   ```bash
   grep -o 'href="[^"]*silent[^"]*"' *-recording-your-first-track.html recording-your-first-track-linux.html | sort -u
   ```

   Example output:

   ```
   mac-recording-your-first-track.html:href="mac-recording-is-silent.html"
   recording-your-first-track-linux.html:href="recording-is-silent-linux.html"
   win-recording-your-first-track.html:href="win-recording-is-silent.html"
   ```

   Each copy links within its own branch, as described in Step 2.
::::

Test the content model for a remedy. In the macOS solution, replace the
`<steps-informal>` element with its `<p>` alone, so that `<remedy>` holds a
title and a paragraph. Save, and choose **Project** > **Check Project**.
The check stops at health.
Example output:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/topics/recording-is-silent.dita
    76:16  The content of element type "remedy" does not match its content model.
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

`<remedy>` allows an optional `<title>`, then one of `<steps>`,
`<steps-unordered>` or `<steps-informal>`, and nothing else. What to do is
always a steps element, even when it is one sentence: that is what
`<steps-informal>` is for.

Undo the change with **Edit** > **Undo**, and save. Choose **XML** >
**Validate**: the **Errors** panel reports **Valid Document**, so the
remedy has its steps element again. To rerun the full check, choose
**Project** > **Check Project** and confirm that it reports `Ready`.

## What you learned

- `<troubleshooting>`, `<troublebody>`, `<condition>`.
- `<troubleSolution>` with `<cause>` then `<remedy>`; a remedy's content
  is `<steps>`, `<steps-unordered>` or `<steps-informal>`.
- A profiling attribute on a `<troubleSolution>`.
- `<note type="trouble">` with an `<xref>` to the troubleshooting topic,
  in place of a warning that carried the fix itself.
- An `<xref>` with a `keyref` and an `href` fallback, so that each
  branch-filtered copy links within its own key scope.
- One topic referenced by six maps, including a nested reference in a
  branch-filtered map.

## Next lesson

**Checkpoint:** `tutorial/20-troubleshooting`. If you use Git, commit your work.

Continue with [Stage 21: hazards and safety](/part-4-books/stage-21-hazards-and-safety).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/19-bookmap...tutorial/20-troubleshooting).
