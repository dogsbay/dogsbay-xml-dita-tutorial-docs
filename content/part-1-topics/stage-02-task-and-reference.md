---
title: "Stage 02: A task and a reference"
description: Write Recording your first track as a task and Supported audio formats as a reference, and learn why DITA has three information types.
type: tutorial
---

# Stage 02: A task and a reference

In this stage you add the other two base information types. *Recording your
first track* is a `<task>`: numbered steps a reader follows to reach a result.
*Supported audio formats* is a `<reference>`: facts to look up, here a table of
formats.

DITA separates concept, task and reference because readers come to
documentation to do one of three things: understand, act, or look something
up. Each type has a body whose content model matches that purpose, so a task
cannot drift into an essay and a reference cannot hide a procedure. The
validator enforces the split, which is the point of choosing a type.

**Time:** about 20 minutes.
**You need:** stage 01 complete.

## Step 1: Write the task

::::steps
1. **Create `topics/recording-your-first-track.dita`**

   ```xml title="topics/recording-your-first-track.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">

   <task id="recording-your-first-track">
     <title>Recording your first track</title>
     <shortdesc>Make your first recording in Audacity: pick a microphone, record, stop and play it back.</shortdesc>
     <taskbody>
       <context>
         <p>This task walks you through making your first audio recording.
         You need a microphone connected to your computer.</p>
       </context>
       <steps>
         <step>
           <cmd>Open Audacity.</cmd>
           <info>
             <p>On first launch Audacity opens an empty project window with a gray waveform area.</p>
           </info>
         </step>
         <step>
           <cmd>Select your microphone from the recording device list in the Device Toolbar.</cmd>
           <info>
             <p>The Device Toolbar is near the top of the window.
             The recording device list has a microphone icon next to it.</p>
           </info>
         </step>
         <step>
           <cmd>Click Record to start recording.</cmd>
           <info>
             <p>A blue waveform appears as Audacity captures audio from your microphone.
             A flat line means the microphone is not selected correctly: click Stop and check the recording device.</p>
           </info>
         </step>
         <step>
           <cmd>Speak into your microphone, or play the audio you want to capture.</cmd>
         </step>
         <step>
           <cmd>Click Stop when you are finished.</cmd>
         </step>
         <step>
           <cmd>Click Play to listen to your recording.</cmd>
         </step>
       </steps>
       <result>
         <p>You now have a recorded audio track in your project.
         The waveform shows the shape of the recorded audio: louder sounds appear as taller waves.</p>
       </result>
     </taskbody>
   </task>
   ```

2. **Read the task elements**
   - `<task>` and `<taskbody>` take the place of `<concept>` and `<conbody>`.
     The DOCTYPE changes with them: `-//OASIS//DTD DITA Task//EN`.
   - `<context>` says why and when the reader does this task. It comes before
     the steps and holds ordinary paragraphs.
   - `<steps>` is the procedure; each `<step>` is one action. The task body
     allows one `<steps>` (or `<steps-unordered>`, which stage 04 shows).
   - `<cmd>` is the action itself, and is the one required child of a step.
     Write it as an imperative sentence. A reader who only reads the `<cmd>`
     elements should be able to complete the task.
   - `<info>` is extra information about a step: what to expect, where to
     find the control. It is optional and sits after the `<cmd>`.
   - `<result>` says what is true when the steps are done. It comes after the
     steps.
::::

## Step 2: Write the reference

::::steps
1. **Create `topics/supported-audio-formats.dita`**

   ```xml title="topics/supported-audio-formats.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE reference PUBLIC "-//OASIS//DTD DITA Reference//EN" "reference.dtd">

   <reference id="supported-audio-formats">
     <title>Supported audio formats</title>
     <shortdesc>The audio formats Audacity imports and exports, and when to use each one.</shortdesc>
     <refbody>
       <section>
         <p>Audacity imports and exports audio in several formats.
         Lossless formats keep every sample of the source; lossy formats discard detail to make smaller files.</p>
       </section>
       <table>
         <title>Common audio formats</title>
         <tgroup cols="4">
           <colspec colname="format" colwidth="1*"/>
           <colspec colname="type" colwidth="1*"/>
           <colspec colname="quality" colwidth="1*"/>
           <colspec colname="notes" colwidth="2*"/>
           <thead>
             <row>
               <entry>Format</entry>
               <entry>Type</entry>
               <entry>Quality</entry>
               <entry>Notes</entry>
             </row>
           </thead>
           <tbody>
             <row>
               <entry>WAV</entry>
               <entry>Uncompressed</entry>
               <entry>Lossless</entry>
               <entry>Large files, no quality loss.
               Best for archiving and further editing.</entry>
             </row>
             <row>
               <entry>MP3</entry>
               <entry>Compressed</entry>
               <entry>Lossy</entry>
               <entry>Widely supported, small files.
               Good for sharing and the web.</entry>
             </row>
             <row>
               <entry>OGG Vorbis</entry>
               <entry>Compressed</entry>
               <entry>Lossy</entry>
               <entry>Open alternative to MP3.
               Good quality at low bitrates.</entry>
             </row>
             <row>
               <entry>FLAC</entry>
               <entry>Compressed</entry>
               <entry>Lossless</entry>
               <entry>Smaller than WAV with no quality loss.
               Ideal for archiving when space matters.</entry>
             </row>
           </tbody>
         </tgroup>
       </table>
     </refbody>
   </reference>
   ```

2. **Read the reference elements**
   - `<reference>` and `<refbody>`, with the DOCTYPE
     `-//OASIS//DTD DITA Reference//EN`. A reference body is made of
     sections, tables and property lists rather than free paragraphs, so the
     introductory `<p>` sits inside a `<section>`.
   - `<table>` is the CALS table model, the one used for anything with column
     widths, spanning or a header row. Its `<title>` is optional and makes the
     table a numbered, linkable figure in output.
   - `<tgroup cols="4">` declares the number of columns and holds everything
     else. One table can have several `tgroup`s with different column
     layouts.
   - `<colspec>` names each column and sets its width. `colwidth="2*"` is a
     proportional width: this column gets twice the share of the `1*`
     columns.
   - `<thead>` and `<tbody>` hold header and body rows; each `<row>` holds one
     `<entry>` per column.
::::

## Step 3: Update the README and run the gate

::::steps
1. **Change the "You are on" line**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 01 — concept**: one concept topic, and the gate proves it validates.
   +You are on **stage 02 — task and reference**: the three information types side by side.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   3 file(s): 3 valid, 0 invalid.
   …
   STAGE OK
   ```
::::

To see the type split enforced, move the `<p>` in the reference out of its
`<section>` so it is a direct child of `<refbody>`, and run the gate. The
reference body does not allow it. A concept body would.

## What you learned

- A task is `<task>`, `<taskbody>`, `<context>`, `<steps>`, `<step>`,
  `<cmd>`, `<info>`, `<result>`; the `<cmd>` is the step.
- A reference is `<reference>`, `<refbody>`, sections and tables; no loose
  paragraphs.
- The CALS table: `<table>`, `<tgroup cols>`, `<colspec>`, `<thead>`,
  `<tbody>`, `<row>`, `<entry>`.
- The three types exist so the grammar enforces the purpose of a topic.

## Where to go next

:::cards
- **[Stage 03: Inline and block elements](/part-1-topics/stage-03-inline-and-block)** {icon="arrow-right"}
  Mark up UI controls, terms, file paths, notes, lists and code.

- **[Compare 01 to 02 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/01-concept...tutorial/02-task-and-reference)** {icon="github"}
  Exactly what this stage added.
:::
