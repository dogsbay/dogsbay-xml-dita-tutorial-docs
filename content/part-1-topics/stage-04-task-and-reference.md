---
title: "Stage 04: A task and a reference"
description: Write Recording your first track as a task and Supported audio formats as a reference, and learn why DITA has three information types.
type: tutorial
---

# Stage 04: A task and a reference

Add two information types to the guide. *Recording your first track* is a
`<task>` with steps that lead to a result. *Supported audio formats* is a
`<reference>` with a table of facts.

Concepts explain a subject, tasks describe actions, and references provide
information to look up. Each type has a content model that controls its
structure. Validation checks that structure; you must also review whether
the content serves the topic's purpose.

**Time:** about 20 minutes.
**You need:** stage 03 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: Write the task

::::steps
1. **Create `topics/recording-your-first-track.dita`**
   In the **Explorer**, right-click the `topics` folder, choose
   **New File**, and enter `recording-your-first-track.dita`. In the
   **New XML Document** dialog, choose **Task** and click **OK**. As in
   stage 01, the editor sets the `@id` from the file name. The template
   gives you the task skeleton:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">

   <task id="recording-your-first-track">
     <title>Task Title</title>
     <shortdesc></shortdesc>
     <taskbody>
       <steps>
         <step>
           <cmd></cmd>
         </step>
       </steps>
     </taskbody>
   </task>
   ```

   Replace the title placeholder (**XML** > **Select Element Content**)
   and type the short description. Add the `<context>` before `<steps>`,
   type the first action in the empty `<cmd></cmd>`, then add the
   `<info>` elements, the other steps, and the `<result>`. If you use another editor, create the file and
   type the finished listing. The finished task:

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
     allows one `<steps>` (or `<steps-unordered>`, which stage 06 shows).
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
   Create the file in the `topics` folder in the same way, and choose the
   **Reference** template:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE reference PUBLIC "-//OASIS//DTD DITA Reference//EN" "reference.dtd">

   <reference id="supported-audio-formats">
     <title>Reference Title</title>
     <shortdesc></shortdesc>
     <refbody>
       <section>
         <title>Section Title</title>
         <p></p>
       </section>
     </refbody>
   </reference>
   ```

   Replace the title and type the short description. This reference's
   introductory section has no title, so delete
   `<title>Section Title</title>`, and type the introduction in the empty
   `<p></p>`. Then add the `<table>` after the `</section>`. If you use
   another editor, create the file and type the finished listing. The
   finished reference:

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
   - `<table>` uses the CALS table model. Use it for row or column spanning,
     a table title, or detailed column specifications. Its optional
     `<title>` supplies the table caption. Simple tables also support
     headers and relative column widths, as stage 05 explains.
   - `<tgroup cols="4">` declares the number of columns and holds everything
     else. One table can have several `tgroup`s with different column
     layouts.
   - `<colspec>` names each column and sets its width. `colwidth="2*"` is a
     proportional width: this column gets twice the share of the `1*`
     columns.
   - `<thead>` and `<tbody>` hold header and body rows; each `<row>` holds one
     `<entry>` per column.
::::

## Step 3: Update the map and check your work

::::steps
1. **Add the topics to the map**
   Add a `<topicref>` for each new topic to `audacity-guide.ditamap`.
   Do this before you check your work: the check reports a topic that
   no map refers to as an orphan topic. The complete map at this
   checkpoint is:

   ```xml title="audacity-guide.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">
   
   <map>
     <title>Audio user guide</title>
     <topicref href="topics/recording-your-first-track.dita"/>
     <topicref href="topics/supported-audio-formats.dita"/>
     <topicref href="topics/what-is-audacity.dita"/>
   </map>
   ```

2. **Format and check your work**
   Format the map and the topics: choose **XML** > **Format** in each
   changed file and save it, or choose **Project** > **Project Tools** >
   **Format Project** to format every file at once. From the command line,
   run:

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   ```

   Then choose **Project** > **Check Project** in the editor, or run
   `dogsbay-xml check .` from the project root. The check rebuilds the
   guide in `out/full/`. The output looks like this example:

   ```
   health   clean
   build    full                 ok  /home/you/my-audacity-guide/out/full
   output   clean (full)
   Ready: the project is healthy, every deliverable built, and the output of full holds together.
   ```
::::

To see the type split enforced, move the `<p>` in the reference out of its
`<section>` so it is a direct child of `<refbody>`, and check your work. The
reference body does not allow it. A concept body would. The check names the
file with the line, column, and message of the error, and stops. The output
looks like this example:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/topics/supported-audio-formats.dita
    57:13  The content of element type "refbody" does not match its content model.
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

The **Project Validation** panel, or `dogsbay-xml project-health .`, gives
the full report. For example:

```
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Invalid files (1 of 4):
  /home/you/my-audacity-guide/topics/supported-audio-formats.dita:
    57:13  error: The content of element type "refbody" does not match its content model.

Summary
  Invalid files                 1  of 4
```

Undo the change and check again. Confirm that the check reports `Ready`
before you continue.

## What you learned

- A task is `<task>`, `<taskbody>`, `<context>`, `<steps>`, `<step>`,
  `<cmd>`, `<info>`, `<result>`; the `<cmd>` is the step.
- A reference is `<reference>`, `<refbody>`, sections and tables; no loose
  paragraphs.
- The CALS table: `<table>`, `<tgroup cols>`, `<colspec>`, `<thead>`,
  `<tbody>`, `<row>`, `<entry>`.
- The three types provide content models for different reader needs.

## Next lesson

**Checkpoint:** `tutorial/04-task-and-reference`. If you use Git, commit your work.

Continue with [Stage 05: inline and block](/part-1-topics/stage-05-inline-and-block).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/03-first-build...tutorial/04-task-and-reference).
