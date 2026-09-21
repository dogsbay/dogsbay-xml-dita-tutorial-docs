---
title: "Stage 04: Rich tasks"
description: Write four more tasks that use prerequisites, substeps, choices, a choice table, step examples and results, troubleshooting, and unordered steps.
type: tutorial
---

# Stage 04: Rich tasks

In this stage you write four tasks that between them use the rest of the task
model: *Installing Audacity*, *Exporting audio*, *Removing background noise*
and *Preparing to record*. The task in stage 02 was `<context>`, `<steps>` and
`<result>`; real procedures need to say what must be true first, what to do
next, what happens after a step, where the reader chooses, and what to do
when it goes wrong.

Each element here has a place the DTD allows and a purpose. The point of
learning them is not to use them all in every task; it is to recognise when a
paragraph of prose is a `<prereq>` or a `<stepresult>` in disguise.

**Time:** about 30 minutes.
**You need:** stage 03 complete.

## Step 1: Installing Audacity

::::steps
1. **Create `topics/installing-audacity.dita`**

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
     before `<context>`, and output labels it "Before you begin".
   - `<postreq>` states what to do after the task is done. It goes after
     `<result>`, and output labels it "What to do next".
   - `<choicetable>` is a step whose action depends on a condition, laid out
     as a two-column table: `<chhead>` with `<choptionhd>` and `<chdeschd>`
     for the headings, then one `<chrow>` per option with `<choption>` and
     `<chdesc>`. It sits inside a `<step>` after the `<cmd>`.
   - `<substeps>` breaks a step into `<substep>` elements, each with its own
     `<cmd>` and optional `<info>`. Substeps do not nest further; if they
     would, the step is a separate task.
   - `@importance="optional"` on a `<step>` or `<substep>` marks it as one
     the reader may skip. Output prefixes it with "Optional:".
   - `<stepresult>` says what happens after one step, as distinct from
     `<result>` for the whole task.
   - `<codeph>` is inline code: the `apt` command inside a sentence.
::::

## Step 2: Exporting audio

::::steps
1. **Create `topics/exporting-audio.dita`**

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
     The children are still `<step>` elements; output renders them as a
     bulleted rather than numbered list. Say in the `<context>` that the
     order is free, as this topic does.
   - `<tutorialinfo>` is additional information for a reader who is learning,
     as opposed to `<info>`, which is for anyone doing the step. A stylesheet
     for expert readers can drop it.
::::

## Step 5: Update the README and run the gate

::::steps
1. **Change the "You are on" line**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 03 — inline and block**: UI controls, shortcuts, terms, notes, lists, tables and code.
   +You are on **stage 04 — rich tasks**: prerequisites, choices, substeps, examples, troubleshooting and unordered steps.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   9 file(s): 9 valid, 0 invalid.
   …
   STAGE OK
   ```
::::

The order inside `<taskbody>` is fixed: `<prereq>` and `<context>`, then
`<steps>`, `<result>`, `<tasktroubleshooting>`, `<example>`, `<postreq>`. To see the validator hold that order,
move the `<postreq>` in *Installing Audacity* above `<result>` and run the
gate.

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

## Where to go next

:::cards
- **[Stage 05: Links](/part-1-topics/stage-05-links)** {icon="arrow-right"}
  Cross-references and related links between the nine topics.

- **[Compare 03 to 04 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/03-inline-and-block...tutorial/04-rich-tasks)** {icon="github"}
  Exactly what this stage added.
:::
