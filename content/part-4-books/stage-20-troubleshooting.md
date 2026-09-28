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
and add it to five maps. This extends the task-level troubleshooting
elements introduced in stage 06.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 25 minutes.
**You need:** stage 19 complete.


Recorded diagnostic examples below come from earlier runs. File counts, paths, and stage numbers can differ. Run the gate for your current checkout.

## Step 1: The troubleshooting topic

::::steps
1. **Create `topics/recording-is-silent.dita`**

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
   +          See <xref href="recording-is-silent.dita"/>.</note>
            </info>
          </step>
          <step>
   ```

   `<note type="trouble">` is the note type for "if this goes wrong, look
   here". The HTML5 transform labels it *Trouble:*. It replaces a
   `warning`, which is for harm to people or data, and the fix moves out
   of the note into the topic it links to. The `<xref>` is a plain local
   reference; the book, the beginner guide and the podcaster guide all
   contain the target, so it resolves in every deliverable.
::::

## Step 2: The topic in every map

::::steps
1. **Edit `audacity-guide.ditamap`**

   ```diff title="audacity-guide.ditamap"
   --- a/audacity-guide.ditamap
   +++ b/audacity-guide.ditamap
   @@ -51,6 +51,10 @@
          <topicref href="topics/removing-background-noise.dita" linking="targetonly"/>
          <topicref href="topics/effect-order.dita"/>
        </topicset>
   +  </topichead>
   +
   +  <topichead navtitle="Troubleshooting" id="troubleshooting">
   +    <topicref href="topics/recording-is-silent.dita" keys="silent"/>
      </topichead>
    
      <topichead navtitle="Podcast production" audience="podcaster">
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
   @@ -20,6 +20,9 @@
        <topicref href="topics/trimming-audio.dita"/>
        <topicref href="topics/exporting-audio.dita"/>
      </topichead>
   +  <topichead navtitle="Troubleshooting">
   +    <topicref href="topics/recording-is-silent.dita"/>
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
   @@ -31,6 +31,8 @@
            <dvrKeyscopePrefix>linux.</dvrKeyscopePrefix>
          </ditavalmeta>
        </ditavalref>
   -    <topicref href="topics/recording-your-first-track.dita" keys="first-track"/>
   +    <topicref href="topics/recording-your-first-track.dita" keys="first-track">
   +      <topicref href="topics/recording-is-silent.dita"/>
   +    </topicref>
      </topicref>
    </map>
   ```

6. **Read the five references**

   - The full guide gives the topic a `<topichead>` of its own, with an
     `id` so a later stage can reference the branch, and the key `silent`.
     The key is defined here and used in stage 21.
   - In the book it is an appendix, before the two reference appendixes.
   - The beginner and podcaster guides put it in a *Troubleshooting*
     head of their own.
   - The installation variants map nests it under the recording task, so
     each of the three platform branches from stage 16 carries its own
     filtered copy: the Mac branch keeps the macOS solution, the other
     two drop it. `dogsbay-xml list-branches installation-variants.ditamap`
     shows the topic under each prefix.
::::

## Step 3: README and the gate

::::steps
1. **Change the "You are on" line**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -2,7 +2,7 @@
    
    Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
    
   -You are on stage 19: bookmap.
   +You are on stage 20: troubleshooting.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   39 file(s): 39 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == validate-conditions  /home/you/audacity-guide  (scheme: subject-scheme.ditamap) ==
   39 file(s): 39 pass, 0 with violations.

   == build  project.json -> /tmp/check-stage-<pid> ==
   all deliverables built

   STAGE OK
   ```
::::

Test the content model for a remedy. In the macOS solution, replace the
`<steps-informal>` element with its `<p>` alone, so that `<remedy>` holds a
title and a paragraph, and run the gate with `SKIP_BUILD=1`:

```
== validate-project  /home/you/audacity-guide ==
/home/you/audacity-guide/topics/recording-is-silent.dita:
  76:16  error: The content of element type "remedy" does not match its content model.
39 file(s): 38 valid, 1 invalid.
FAIL: validation errors
```

`<remedy>` allows an optional `<title>`, then one of `<steps>`,
`<steps-unordered>` or `<steps-informal>`, and nothing else. What to do is
always a steps element, even when it is one sentence: that is what
`<steps-informal>` is for.

After each error exercise, undo the deliberate change and rerun the gate.
Confirm that it prints `STAGE OK` before continuing.

## What you learned

- `<troubleshooting>`, `<troublebody>`, `<condition>`.
- `<troubleSolution>` with `<cause>` then `<remedy>`; a remedy's content
  is `<steps>`, `<steps-unordered>` or `<steps-informal>`.
- A profiling attribute on a `<troubleSolution>`.
- `<note type="trouble">` with an `<xref>` to the troubleshooting topic,
  in place of a warning that carried the fix itself.
- One topic referenced by five maps, including a nested reference in a
  branch-filtered map.

## Next lesson

Continue with [Stage 21: hazards and safety](/part-4-books/stage-21-hazards-and-safety).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/19-bookmap...tutorial/20-troubleshooting).
