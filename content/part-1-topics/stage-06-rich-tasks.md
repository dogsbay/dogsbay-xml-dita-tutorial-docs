---
title: "Stage 06: Rich tasks"
description: Write four more tasks that use prerequisites, substeps, choices, a choice table, step examples and results, troubleshooting, and unordered steps.
type: tutorial
---

# Stage 06: Rich tasks

Write four tasks: *Installing Audacity*, *Exporting audio*, *Removing
background noise*, and *Preparing to record*. Add prerequisites, choices,
substeps, examples, results, and troubleshooting information where each
task needs them.

Choose elements according to their purpose. Use `<prereq>` for a
requirement that readers must meet before starting, for example, and
`<stepresult>` for the expected result of a step.

**Time:** about 30 minutes.
**You need:** stage 05 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: Installing Audacity

::::steps
1. **Create `topics/installing-audacity.dita`**
   Each topic in this lesson is a task. In the **Explorer**, right-click
   the `topics` folder, choose **New File**, and enter
   `installing-audacity.dita`. Click the **Task** template. Replace the
   title placeholder (**XML** > **Select Element Content**, or
   Ctrl+Shift+E) and type the short description. Then click inside
   `<taskbody>`, select its content the same way, and replace it with the
   body from the finished listing: type it, or paste it. If you use another
   editor, create the file and type the finished listing.

   ```xml title="topics/installing-audacity.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">
   
   <task id="installing-audacity">
     <title>Installing Audacity</title>
     <shortdesc>Download Audacity from the official website, or install it with your package manager, and launch it once to finish setup.</shortdesc>
     <taskbody>
       <prereq>
         <p>You need about 250 MB of free disk space and an account that can install software.</p>
       </prereq>
       <context>
         <p>Audacity is available for Windows, macOS and Linux.
         The steps differ by operating system only at the install step.</p>
       </context>
       <steps>
         <step>
           <cmd>Download Audacity from <filepath>https://www.audacityteam.org/download/</filepath>.</cmd>
           <info>
             <p>Select the installer for your operating system.</p>
           </info>
         </step>
         <step>
           <cmd>Install it.</cmd>
           <choicetable>
             <chhead>
               <choptionhd>Operating system</choptionhd>
               <chdeschd>How to install</chdeschd>
             </chhead>
             <chrow>
               <choption>Windows</choption>
               <chdesc>Double-click the downloaded <filepath>.exe</filepath> file and follow the setup wizard.
               Accept the default location, <filepath>C:\Program Files\Audacity</filepath>, unless you have a reason to change it.</chdesc>
             </chrow>
             <chrow>
               <choption>macOS</choption>
               <chdesc>Open the downloaded <filepath>.dmg</filepath> file and drag Audacity to your <filepath>Applications</filepath> folder.</chdesc>
             </chrow>
             <chrow>
               <choption>Linux</choption>
               <chdesc>Install from your distribution's package manager, for example <codeph>sudo apt install audacity</codeph> on Ubuntu or Debian, <codeph>sudo dnf install audacity</codeph> on Fedora.</chdesc>
             </chrow>
           </choicetable>
         </step>
         <step>
           <cmd>Launch Audacity.</cmd>
           <substeps>
             <substep>
               <cmd>If Audacity asks to scan for audio plugins, click <uicontrol>OK</uicontrol>.</cmd>
               <info>
                 <p>This happens once, on first launch.</p>
               </info>
             </substep>
             <substep importance="optional">
               <cmd>Choose <menucascade><uicontrol>Help</uicontrol><uicontrol>Check for Updates</uicontrol></menucascade> to confirm you have the latest version.</cmd>
             </substep>
           </substeps>
           <stepresult>
             <p>The main window opens with an empty project.</p>
           </stepresult>
         </step>
       </steps>
       <result>
         <p>Audacity is installed and ready to use.</p>
       </result>
       <postreq>
         <p>Connect a microphone before you continue to recording.</p>
       </postreq>
     </taskbody>
   </task>
   ```

2. **Read the new elements**

   - `<prereq>` states what must be true before the reader starts. It goes
     before `<context>`.
   - `<postreq>` states what to do after the task is done. It goes after
     `<result>`.
   - The editor's preview labels these sections. The guide's build uses
     DITA-OT's `html5` transtype, which by default shows both sections
     without a heading. To add the labels "Before you begin" and "What to
     do next" to the built pages, set the DITA-OT parameter
     `args.gen.task.lbl` to `YES`.
   - `<choicetable>` is a step whose action depends on a condition, laid out
     as a two-column table: `<chhead>` with `<choptionhd>` and `<chdeschd>`
     for the headings, then one `<chrow>` per option with `<choption>` and
     `<chdesc>`. It sits inside a `<step>` after the `<cmd>`.
   - `<substeps>` breaks a step into `<substep>` elements, each with its own
     `<cmd>` and optional `<info>`. Substeps do not nest further; if they
     would, the step is a separate task.
   - `@importance="optional"` on a `<step>` or `<substep>` marks it as one
     the reader may skip. The `html5` transform prefixes it with
     "Optional:".
   - `<stepresult>` says what happens after one step, as distinct from
     `<result>` for the whole task.
   - `<codeph>` is inline code: the `apt` command inside a sentence.

3. **Preview the task**
   Before you preview, predict how a reader will see the choice table and
   the prerequisite. Then choose **View** > **Preview in Tab**. The
   prerequisite has the label **Before you begin**, and the choice table is
   a table inside step 2, with its headings. Further down, the substeps are
   a list of their own inside the launch step, followed by the step result.
   After the steps come the **Result** and **What to do next** blocks.
::::

## Step 2: Exporting audio

::::steps
1. **Create `topics/exporting-audio.dita`**
   Create `exporting-audio.dita` in the `topics` folder from the **Task**
   template, as for *Installing Audacity*. Replace the title, type the
   short description, and replace the content of `<taskbody>` with the body
   from the listing. If you use another editor,
   create the file and type the finished listing.

   ```xml title="topics/exporting-audio.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">
   
   <task id="exporting-audio">
     <title>Exporting audio</title>
     <shortdesc>Export a finished project as a WAV, MP3, OGG or FLAC file that other programs and devices can play.</shortdesc>
     <taskbody>
       <prereq>
         <p>Finish editing and save the project.
         A project file (<filepath>.aup3</filepath>) can only be opened by Audacity; exporting creates an ordinary audio file.</p>
       </prereq>
       <steps>
         <step>
           <cmd>Choose <menucascade><uicontrol>File</uicontrol><uicontrol>Export Audio</uicontrol></menucascade>.</cmd>
           <stepresult>
             <p>The <wintitle>Export Audio</wintitle> dialog opens.</p>
           </stepresult>
         </step>
         <step>
           <cmd>Choose a format.</cmd>
           <choices>
             <choice>Choose <uicontrol>WAV</uicontrol> or <uicontrol>FLAC</uicontrol> for an archive copy with no quality loss.</choice>
             <choice>Choose <uicontrol>MP3</uicontrol> or <uicontrol>OGG</uicontrol> for a small file to share.</choice>
           </choices>
         </step>
         <step>
           <cmd>Set the format options.</cmd>
           <info>
             <p>For MP3, the bit rate controls the trade-off between size and quality.</p>
           </info>
           <stepxmp>
             <p>A spoken-word podcast exports well at <uicontrol>128 kbps</uicontrol>, <uicontrol>Mono</uicontrol>.
             Music needs <uicontrol>192 kbps</uicontrol> or more, <uicontrol>Stereo</uicontrol>.</p>
           </stepxmp>
         </step>
         <step>
           <cmd>Choose a folder and a file name, then click <uicontrol>Export</uicontrol>.</cmd>
           <steptroubleshooting>
             <p>If the <uicontrol>Export</uicontrol> button is disabled, the file name is empty or the folder is read-only.</p>
           </steptroubleshooting>
         </step>
         <step importance="optional">
           <cmd>Fill in the <wintitle>Edit Metadata Tags</wintitle> dialog and click <uicontrol>OK</uicontrol>.</cmd>
           <info>
             <p>Title, artist and album are shown by most players.</p>
           </info>
         </step>
       </steps>
       <result>
         <p>The exported file is in the folder you chose.
         The project itself is unchanged.</p>
       </result>
       <example>
         <title>Exporting a podcast episode</title>
         <p>Choose <uicontrol>MP3</uicontrol>, set <uicontrol>Bit Rate Mode</uicontrol> to <uicontrol>Constant</uicontrol>, <uicontrol>Quality</uicontrol> to <uicontrol>128 kbps</uicontrol> and <uicontrol>Channel Mode</uicontrol> to <uicontrol>Force export to mono</uicontrol>.
         A 30-minute episode exports to a file of about 27 MB.</p>
       </example>
     </taskbody>
   </task>
   ```

2. **Read the new elements**

   - `<choices>` is the simpler alternative to `<choicetable>`: a list of
     `<choice>` elements, each one option, when no description column is
     needed.
   - `<stepxmp>` is an example for one step: what the settings look like in a
     concrete case.
   - `<steptroubleshooting>` says what to do if the step does not produce the
     expected result. It comes last in the step.
   - `<example>` is an example for the whole task, with its own `<title>`. It
     sits after `<result>` in the task body.
::::

## Step 3: Removing background noise

::::steps
1. **Create `topics/removing-background-noise.dita`**
   Create `removing-background-noise.dita` in the `topics` folder from the
   **Task** template, as for *Installing Audacity*. Replace the title, type
   the short description, and replace the content of `<taskbody>` with the
   body from the listing. If you use another
   editor, create the file and type the finished listing.

   ```xml title="topics/removing-background-noise.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">
   
   <task id="removing-background-noise">
     <title>Removing background noise</title>
     <shortdesc>Take a noise profile from a quiet section, then apply Noise Reduction to the whole track to remove hum, hiss and fan noise.</shortdesc>
     <taskbody>
       <context>
         <p>Background noise such as hum from electronics, hiss from a microphone or fan noise degrades a recording.
         The Noise Reduction effect learns what the noise sounds like from a section that contains nothing else, then removes it everywhere.</p>
         <note type="warning">This effect changes the audio data.
         Save the project first; you can undo with <uicontrol>Ctrl+Z</uicontrol> (Windows and Linux) or <uicontrol>Cmd+Z</uicontrol> (macOS).</note>
       </context>
       <steps>
         <step>
           <cmd>Find a section of the recording that contains only the background noise.</cmd>
           <info>
             <p>Look for a quiet gap at the beginning or end.
             You need at least half a second of noise-only audio.</p>
           </info>
         </step>
         <step>
           <cmd>Select that section by clicking and dragging in the waveform.</cmd>
         </step>
         <step>
           <cmd>Choose <menucascade><uicontrol>Effect</uicontrol><uicontrol>Noise Removal and Repair</uicontrol><uicontrol>Noise Reduction</uicontrol></menucascade>, then click <uicontrol>Get Noise Profile</uicontrol>.</cmd>
           <stepresult>
             <p>Audacity captures the character of the noise and closes the dialog.</p>
           </stepresult>
         </step>
         <step>
           <cmd>Select the whole track with <uicontrol>Ctrl+A</uicontrol> or <uicontrol>Cmd+A</uicontrol>.</cmd>
         </step>
         <step>
           <cmd>Open <uicontrol>Noise Reduction</uicontrol> again, adjust the settings and click <uicontrol>OK</uicontrol>.</cmd>
           <info>
             <p>Recommended starting settings:</p>
             <ul>
               <li>Noise reduction: 12 dB</li>
               <li>Sensitivity: 6</li>
               <li>Frequency smoothing: 3</li>
             </ul>
             <note type="caution">Too much noise reduction makes voices sound metallic or underwater.
             Start moderate, increase gradually, and use <uicontrol>Preview</uicontrol> before applying.</note>
           </info>
         </step>
       </steps>
       <result>
         <p>The background noise is reduced across the whole track.
         Play it back to check that the voice still sounds natural.</p>
       </result>
       <tasktroubleshooting>
         <p>If the voice sounds hollow after the effect, undo, then lower <uicontrol>Noise reduction</uicontrol> to 6 dB and try again.
         If the noise is a constant hum at one pitch, the <uicontrol>Notch Filter</uicontrol> effect removes it with less damage to the voice.</p>
       </tasktroubleshooting>
     </taskbody>
   </task>
   ```

2. **Read the new elements**

   - `<tasktroubleshooting>` is the task-level counterpart of
     `<steptroubleshooting>`: what to do when the whole task did not give the
     expected result. It comes after `<result>` and before `<example>`.
   - `<note type="caution">` inside a step's `<info>`, beside the `<ul>` of
     settings: a note can share `<info>` with other blocks.
::::

## Step 4: Preparing to record

::::steps
1. **Create `topics/preparing-to-record.dita`**
   Create `preparing-to-record.dita` in the `topics` folder from the
   **Task** template, as for *Installing Audacity*. Replace the title and
   type the short description. This task uses `<steps-unordered>` instead
   of `<steps>`, because numbered steps would tell the reader that the
   order matters. Replace the content of `<taskbody>` with the body from
   the listing, or change the template's `<steps>` and `</steps>` tags to
   `<steps-unordered>` and `</steps-unordered>` and type the rest. If you
   use another
   editor, create the file and type the finished listing.

   ```xml title="topics/preparing-to-record.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">
   
   <task id="preparing-to-record">
     <title>Preparing to record</title>
     <shortdesc>Checks to make in any order before you press Record: input level, sample rate and disk space.</shortdesc>
     <taskbody>
       <context>
         <p>None of these checks depends on another, so do them in whatever order suits you.</p>
       </context>
       <steps-unordered>
         <step>
           <cmd>Speak at normal volume and watch the recording meter; aim for peaks around -12 dB.</cmd>
           <tutorialinfo>
             <p>Leaving headroom avoids clipping, the flat-topped distortion that no effect can repair.</p>
           </tutorialinfo>
         </step>
         <step>
           <cmd>Set the project sample rate to 44,100 Hz in the bottom-left corner of the window.</cmd>
         </step>
         <step>
           <cmd>Check that the drive holding the project has at least 1 GB free.</cmd>
           <info>
             <p>An hour of stereo audio at 44,100 Hz and 32-bit float needs about 1.2 GB.</p>
           </info>
         </step>
       </steps-unordered>
     </taskbody>
   </task>
   ```

2. **Read the new elements**

   - `<steps-unordered>` replaces `<steps>` when the order does not matter.
     The children are still `<step>` elements. The `html5` transform
     renders them as a bulleted list instead of a numbered one. Say in the `<context>` that the
     order is free, as this topic does.
   - `<tutorialinfo>` is additional information for a reader who is learning,
     as opposed to `<info>`, which is for anyone doing the step. A stylesheet
     for expert readers can drop it.
::::

## Step 5: Update the map and build

::::steps
1. **Add the topics to the map**
   Add a `<topicref>` for each new topic at the end of
   `audacity-guide.ditamap`, in the order you wrote them, so that the build
   includes them. Do this before you check your work: the check reports a topic that
   no map refers to as an orphan topic. The complete map at this
   checkpoint is:

   ```xml title="audacity-guide.ditamap"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE map PUBLIC "-//OASIS//DTD DITA Map//EN" "map.dtd">
   
   <map>
     <title>Audio user guide</title>
     <topicref href="topics/what-is-audacity.dita"/>
     <topicref href="topics/recording-your-first-track.dita"/>
     <topicref href="topics/supported-audio-formats.dita"/>
     <topicref href="topics/what-is-digital-audio.dita"/>
     <topicref href="topics/trimming-audio.dita"/>
     <topicref href="topics/installing-audacity.dita"/>
     <topicref href="topics/exporting-audio.dita"/>
     <topicref href="topics/removing-background-noise.dita"/>
     <topicref href="topics/preparing-to-record.dita"/>
   </map>
   ```

2. **Format and build**
   Save your files. Format the map and the topics: choose **Project** >
   **Project Tools** > **Format Project** to format every file at once, or
   choose **XML** > **Format** in each changed file and save it. From the
   command line, run:

   ```bash
   dogsbay-xml format -i topics/*.dita *.ditamap
   ```

   Then choose **Project** > **Build Deliverables...** and click
   **Build All**. When the build finishes, click **OK**. The build rebuilds
   the guide in `out/full/`, now with nine topics, each a page of its own.

   To make sure that the project is also healthy, choose **Project** >
   **Check Project** in the editor, or run `dogsbay-xml check .` from the
   project root. The check also builds every deliverable. The output looks
   like this example:

   ```
   health   clean
   build    full                 ok  /home/you/my-audacity-guide/out/full
   output   clean (full)
   Ready: the project is healthy, every deliverable built, and every link in the pages of full leads somewhere.
   ```

3. **See the built guide**
   Predict how the built guide shows the steps of *Preparing to record*:
   numbered, or some other way. Then open
   `out/full/topics/preparing-to-record.html` and choose **View** >
   **Preview in Tab** to see the page as a browser shows it. The steps are
   bullets, not numbers. You chose `<steps-unordered>` for its purpose, and
   the transform, DITA-OT's `html5` transtype, shows that purpose as
   bullets.
::::

The order inside `<taskbody>` is fixed: `<prereq>` and `<context>`, then
`<steps>`, `<result>`, `<tasktroubleshooting>`, `<example>`, `<postreq>`. To see the validator hold that order,
open *Installing Audacity* and swap `<result>` and `<postreq>`, so that
`<postreq>` comes first. Save the file. Predict what validation says, then
choose **XML** > **Validate**. The topic is not valid: the **Errors** panel
names the line and the error, which says that the content of `<taskbody>`
does not match its content model.

**Project** > **Check Project** and `dogsbay-xml check .` find the same
error. The check names the file with the line, column, and message of the
error, and stops. The output looks like this example:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/topics/installing-audacity.dita
    68:14  The content of element type "taskbody" does not match its content model.
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

The **Project Validation** panel, or `dogsbay-xml project-health .`, gives
the full report. For example:

```
Root map: audacity-guide.ditamap (project config); house rules: none (none configured)
Invalid files (1 of 10):
  /home/you/my-audacity-guide/topics/installing-audacity.dita:
    68:14  error: The content of element type "taskbody" does not match its content model.

Summary
  Invalid files                 1  of 10
```

Swap the two elements back, so that `<result>` comes first, and save. Choose
**XML** > **Validate** again and confirm that the topic is valid before you
continue. If you ran the check, run it again and confirm that it reports
`Ready`.

## What you learned

- Before and after: `<prereq>` and `<postreq>`.
- Inside a step: `<cmd>` first; then `<info>`, `<tutorialinfo>`,
  `<substeps>`, `<choices>` or `<choicetable>` and `<stepxmp>` in any order;
  then `<stepresult>`, then `<steptroubleshooting>`.
- After the steps, in order: `<result>`, `<tasktroubleshooting>`, `<example>`,
  `<postreq>`.
- `<steps-unordered>` for checks in any order; `@importance="optional"` for
  steps that can be skipped.
- `<tutorialinfo>` for the learner, `<info>` for everyone.

## Next lesson

**Checkpoint:** `tutorial/06-rich-tasks`. If you use Git, commit your work.

Continue with [Stage 07: links](/part-1-topics/stage-07-links).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/05-inline-and-block...tutorial/06-rich-tasks).
