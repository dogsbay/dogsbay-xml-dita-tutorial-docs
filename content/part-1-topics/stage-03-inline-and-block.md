---
title: "Stage 03: Inline and block elements"
description: Mark up UI controls, menu paths, terms and file paths inline; add notes, definition lists, ordered and simple lists, footnotes, simple tables and code blocks.
type: tutorial
---

# Stage 03: Inline and block elements

In this stage you write two more topics, *What is digital audio?* (a concept)
and *Trimming audio* (a task), and go back over the two topics from stage 02
to mark up what was plain text: the names of controls, the terms being
defined, file names. Inline elements say what a piece of text *is*, so that
the stylesheet can render it and a search or a translation tool can find it.

You also meet the block elements that carry most technical writing besides
paragraphs: notes and their types, definition lists, ordered and simple lists,
footnotes, the simple table, and code blocks.

**Time:** about 30 minutes.
**You need:** stage 02 complete.

## Step 1: Write the concept

::::steps
1. **Create `topics/what-is-digital-audio.dita`**

   ```xml title="topics/what-is-digital-audio.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE concept PUBLIC "-//OASIS//DTD DITA Concept//EN" "concept.dtd">

   <concept id="what-is-digital-audio">
     <title>What is digital audio?</title>
     <shortdesc>Digital audio is sound stored as numbers: samples taken thousands of times a second at a chosen precision.</shortdesc>
     <conbody>
       <p>Digital audio is sound that has been converted into a numerical representation.
       A few key concepts help you make better recordings and edits.</p>
       <section>
         <title>Waveforms</title>
         <p>Sound travels through air as a continuous <term>waveform</term> of pressure changes.
         A microphone converts these pressure changes into an electrical signal.
         To store the signal digitally, the computer takes thousands of measurements, called <term>samples</term>, of the signal's amplitude every second.</p>
       </section>
       <section>
         <title>Sample rate</title>
         <p>The <term>sample rate</term> is the number of samples captured per second, measured in hertz (Hz).
         Common sample rates are:</p>
         <sl>
           <sli>44,100 Hz: CD quality, enough for most audio work</sli>
           <sli>48,000 Hz: the standard for video production</sli>
           <sli>96,000 Hz: used in professional music production</sli>
         </sl>
         <p>A higher sample rate captures more detail but produces larger files.<fn>File size grows in proportion to the sample rate: a 96 kHz file is more than twice the size of the same recording at 44.1 kHz.</fn></p>
       </section>
       <section>
         <title>Bit depth</title>
         <p><term>Bit depth</term> determines how precisely each sample's amplitude is recorded.</p>
         <simpletable>
           <sthead>
             <stentry>Bit depth</stentry>
             <stentry>Amplitude values</stentry>
             <stentry>Typical use</stentry>
           </sthead>
           <strow>
             <stentry>16-bit</stentry>
             <stentry>65,536</stentry>
             <stentry>CD audio</stentry>
           </strow>
           <strow>
             <stentry>24-bit</stentry>
             <stentry>over 16 million</stentry>
             <stentry>professional recording</stentry>
           </strow>
           <strow>
             <stentry>32-bit float</stentry>
             <stentry>effectively unlimited</stentry>
             <stentry>editing inside Audacity</stentry>
           </strow>
         </simpletable>
       </section>
       <section>
         <title>Key audio terms</title>
         <dl>
           <dlentry>
             <dt>Amplitude</dt>
             <dd>The height of a sound wave, corresponding to how loud the sound is.
             Measured in decibels (dB).</dd>
           </dlentry>
           <dlentry>
             <dt>Clipping</dt>
             <dd>Distortion that occurs when the signal exceeds the maximum amplitude the system can represent.
             It appears as flat-topped waveforms.</dd>
           </dlentry>
           <dlentry>
             <dt>Mono and stereo</dt>
             <dd>Mono audio uses a single channel.
             Stereo uses two channels, left and right, to create a sense of width.</dd>
           </dlentry>
         </dl>
         <lq reftitle="The Audacity Manual">Audio is measured in decibels because the ear hears ratios, not differences.</lq>
       </section>
     </conbody>
   </concept>
   ```

2. **Read the inline elements**
   - `<term>` marks a word at the point where it is being defined or used
     as a defined term. Stage 11 links terms to glossary entries; marking them
     now is what makes that possible.
   - Notice what is *not* marked. "CD quality" is not a term and "44,100 Hz"
     is not a `<uicontrol>`. Mark meaning, not emphasis.

3. **Read the block elements**
   - `<sl>` with `<sli>` is a simple list: short items, one line each, no
     bullets in most outputs. Use `<ul>` when items are sentences.
   - `<fn>` is a footnote. It sits inline where the reference goes, and
     output moves the text to the foot of the page and leaves a marker.
   - `<simpletable>` is the lightweight table: `<sthead>`, `<strow>` and
     `<stentry>`, no column specs, no spanning. Use it when every cell is a
     short value; use the CALS `<table>` from stage 02 when you need widths,
     a title or spanning.
   - `<dl>` is a definition list: each `<dlentry>` pairs a `<dt>` (the term)
     with a `<dd>` (its definition). It is the element for a list of named
     things with descriptions, which a `<ul>` of "Name: description" items
     would leave unmarked.
   - `<lq>` is a long quotation, set off as a block. `@reftitle` names the
     source.
::::

## Step 2: Write the task

::::steps
1. **Create `topics/trimming-audio.dita`**

   ```xml title="topics/trimming-audio.dita"
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE task PUBLIC "-//OASIS//DTD DITA Task//EN" "task.dtd">

   <task id="trimming-audio">
     <title>Trimming audio</title>
     <shortdesc>Remove silence, false starts and mistakes by selecting a region of the waveform and deleting it.</shortdesc>
     <taskbody>
       <context>
         <p>Trimming removes unwanted audio from the start, end or middle of a track.
         When you delete a region, the audio on either side closes the gap.</p>
         <note type="tip">Zoom in with <uicontrol>Ctrl+1</uicontrol> to make precise selections; zoom back out with <uicontrol>Ctrl+3</uicontrol>.</note>
       </context>
       <steps>
         <step>
           <cmd>Click and drag in the waveform to select the region you want to remove.</cmd>
           <info>
             <p>The selected region is highlighted.
             To select the whole track, press <uicontrol>Ctrl+A</uicontrol> (Windows and Linux) or <uicontrol>Cmd+A</uicontrol> (macOS).</p>
           </info>
         </step>
         <step>
           <cmd>Press <uicontrol>Delete</uicontrol>, or choose <menucascade><uicontrol>Edit</uicontrol><uicontrol><shortcut>D</shortcut>elete</uicontrol></menucascade>.</cmd>
           <info>
             <note type="important">Deleting changes the audio in the project.
             Undo with <uicontrol>Ctrl+Z</uicontrol> if you removed the wrong region.</note>
           </info>
         </step>
         <step>
           <cmd>To keep only the selection and discard everything else, choose <menucascade><uicontrol>Edit</uicontrol><uicontrol>Remove Special</uicontrol><uicontrol>Trim Audio</uicontrol></menucascade> instead.</cmd>
         </step>
         <step>
           <cmd>Play the track from just before the edit to check the join sounds natural.</cmd>
           <info>
             <p>If you hear a click at the join, select a few milliseconds around it and apply <menucascade><uicontrol>Effect</uicontrol><uicontrol>Fade In</uicontrol></menucascade> or <uicontrol>Fade Out</uicontrol>.</p>
           </info>
         </step>
         <step>
           <cmd>Save the project with <menucascade><uicontrol>File</uicontrol><uicontrol>Save Project</uicontrol><uicontrol>Save Project</uicontrol></menucascade>.</cmd>
           <info>
             <p>Audacity saves the project as a single <filepath>.aup3</filepath> file in the folder you choose; the <wintitle>Save Project</wintitle> dialog suggests your Documents folder.</p>
           </info>
         </step>
       </steps>
       <result>
         <p>The unwanted audio is gone and the track plays through without a gap.</p>
       </result>
     </taskbody>
   </task>
   ```

2. **Read the user-interface elements**
   - `<uicontrol>` is the name of a button, menu item, field or key as it
     appears in the interface: `Delete`, `Ctrl+A`, `Edit`.
   - `<menucascade>` is a path through menus. Its children are `<uicontrol>`
     elements in order, and output joins them with an arrow:
     **Edit > Remove Special > Trim Audio**.
   - `<shortcut>` marks the accelerator letter inside a control name, the
     underlined letter in a menu: `<uicontrol><shortcut>D</shortcut>elete</uicontrol>`.
   - `<wintitle>` is the title of a window or dialog: the *Save Project*
     dialog.
   - `<filepath>` is a file name, path or extension: `.aup3`.

3. **Read the notes**
   - `<note>` is a block that stands out from the text. `@type` says why:
     `tip` for a shortcut or a better way, `important` for something the
     reader must not skip, `warning` when something can go wrong, `caution`
     for a milder version, `note` (the default) for anything else. Stage 19
     adds `trouble`. A note may sit in `<context>` or, inside a step, in
     `<info>`; it may not be a direct child of `<step>`.
::::

> [!NOTE]
> `<shortcut>` is only valid inside `<uicontrol>` in DITA 1.3. It marks the
> accelerator letter of a control, not a key combination. Key combinations
> such as `Ctrl+A` are plain `<uicontrol>` text. If you write
> `<cmd>Press <shortcut>Delete</shortcut></cmd>`, the gate reports:
>
> ```
> topics/trimming-audio.dita:
>   22:170  error: The content of element type "cmd" does not match its content model.
> 5 file(s): 4 valid, 1 invalid.
> FAIL: validation errors
> ```

## Step 3: Mark up the stage 02 topics

::::steps
1. **Edit `topics/recording-your-first-track.dita`**
   The control names become `<uicontrol>`, a tip goes into the context, and
   the sentence about the flat line becomes a `<note type="warning">` inside
   the step's `<info>`.

   ```diff title="topics/recording-your-first-track.dita"
   --- a/topics/recording-your-first-track.dita
   +++ b/topics/recording-your-first-track.dita
   @@ -8,6 +8,7 @@
        <context>
          <p>This task walks you through making your first audio recording.
          You need a microphone connected to your computer.</p>
   +      <note type="tip">For best results, record in a quiet room and position the microphone 15 to 30 cm from your mouth.</note>
        </context>
        <steps>
          <step>
   @@ -17,27 +18,28 @@
            </info>
          </step>
          <step>
   -        <cmd>Select your microphone from the recording device list in the Device Toolbar.</cmd>
   +        <cmd>Select your microphone from the recording device list in the <uicontrol>Device Toolbar</uicontrol>.</cmd>
            <info>
   -          <p>The Device Toolbar is near the top of the window.
   +          <p>The <uicontrol>Device Toolbar</uicontrol> is near the top of the window.
              The recording device list has a microphone icon next to it.</p>
            </info>
          </step>
          <step>
   -        <cmd>Click Record to start recording.</cmd>
   +        <cmd>Click <uicontrol>Record</uicontrol>, or press <uicontrol>R</uicontrol>, to start recording.</cmd>
            <info>
   -          <p>A blue waveform appears as Audacity captures audio from your microphone.
   -          A flat line means the microphone is not selected correctly: click Stop and check the recording device.</p>
   +          <p>A blue waveform appears as Audacity captures audio from your microphone.</p>
   +          <note type="warning">A flat line instead of a waveform means the microphone is not selected correctly.
   +          Click <uicontrol>Stop</uicontrol> and check the recording device.</note>
            </info>
          </step>
          <step>
            <cmd>Speak into your microphone, or play the audio you want to capture.</cmd>
          </step>
          <step>
   -        <cmd>Click Stop when you are finished.</cmd>
   +        <cmd>Click <uicontrol>Stop</uicontrol> when you are finished.</cmd>
          </step>
          <step>
   -        <cmd>Click Play to listen to your recording.</cmd>
   +        <cmd>Click <uicontrol>Play</uicontrol> to listen to your recording.</cmd>
          </step>
        </steps>
        <result>
   ```

2. **Edit `topics/supported-audio-formats.dita`**
   The reference gains a second section with an ordered list, a command name
   and a code block, and the terms and file names are marked.

   ```diff title="topics/supported-audio-formats.dita"
   --- a/topics/supported-audio-formats.dita
   +++ b/topics/supported-audio-formats.dita
   @@ -7,7 +7,19 @@
      <refbody>
        <section>
          <p>Audacity imports and exports audio in several formats.
   -      Lossless formats keep every sample of the source; lossy formats discard detail to make smaller files.</p>
   +      <term>Lossless</term> formats keep every sample of the source; <term>lossy</term> formats discard detail to make smaller files.
   +      Exported files are named after the project, so <filepath>podcast-episode-1.aup3</filepath> exports as <filepath>podcast-episode-1.mp3</filepath>.</p>
   +    </section>
   +    <section>
   +      <title>Choosing a format</title>
   +      <p>Keep a lossless master and export a lossy copy for distribution:</p>
   +      <ol>
   +        <li>Save the project (<filepath>.aup3</filepath>) while you are still editing.</li>
   +        <li>Export a WAV or FLAC file as the archive copy.</li>
   +        <li>Export an MP3 or OGG file to share.</li>
   +      </ol>
   +      <p>On Linux you can convert between formats afterwards with <cmdname>ffmpeg</cmdname>:</p>
   +      <codeblock>ffmpeg -i episode.wav -b:a 128k episode.mp3</codeblock>
        </section>
        <table>
          <title>Common audio formats</title>
   @@ -37,6 +49,7 @@
                <entry>Compressed</entry>
                <entry>Lossy</entry>
                <entry>Widely supported, small files.
   +            Needs the <i>LAME</i> encoder, bundled since version 2.3.2.
                Good for sharing and the web.</entry>
              </row>
              <row>
   ```

3. **Read the new elements**
   - `<ol>` with `<li>` is an ordered list. In a concept or reference it is
     for a sequence that is not a procedure; a procedure belongs in a task's
     `<steps>`.
   - `<cmdname>` is the name of a command-line program. `<codeph>` (in the
     next stage) is a fragment of code inline; `<codeblock>` is a block of
     code, lines and spaces preserved. `<pre>` is the same for preformatted
     text that is not code.
   - `<i>` and `<b>` exist, and *LAME* uses one because the encoder's name is
     conventionally set in italics. That is the case for them: typographic
     convention with no semantic element. When there is a semantic element,
     `<term>`, `<uicontrol>`, `<cmdname>`, `<filepath>`, `<ph>`, use it
     instead; `<b>` says nothing a stylesheet or a translator can use.
   - `<ph>` is a phrase with no more specific meaning. It carries attributes
     (conditions in stage 13, ids for reuse in stage 10) when you need a
     handle on a run of text and none of the other inline elements fits.
::::

## Step 4: Update the README and run the gate

::::steps
1. **Change the "You are on" line**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 02 — task and reference**: the three information types side by side.
   +You are on **stage 03 — inline and block**: UI controls, shortcuts, terms, notes, lists, tables and code.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   5 file(s): 5 valid, 0 invalid.
   …
   STAGE OK
   ```
::::

## What you learned

- Inline elements name what text is: `<uicontrol>`, `<menucascade>`,
  `<shortcut>`, `<wintitle>`, `<filepath>`, `<term>`, `<cmdname>`, `<ph>`.
- `<b>` and `<i>` are for typographic convention only; prefer a semantic
  element.
- `<note>` has types, and lives in `<context>` or `<info>`, not directly in a
  step.
- Lists: `<ul>`, `<ol>`, `<sl>`, `<dl>`; each is a different kind of list.
- `<simpletable>` for short values, CALS `<table>` for everything else.
- `<fn>`, `<lq>`, `<codeblock>` and `<pre>` for footnotes, quotations and
  verbatim text.
- `<shortcut>` only inside `<uicontrol>`.

## Where to go next

:::cards
- **[Stage 04: Rich tasks](/part-1-topics/stage-04-rich-tasks)** {icon="arrow-right"}
  Prerequisites, substeps, choices, examples and unordered steps.

- **[Compare 02 to 03 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/02-task-and-reference...tutorial/03-inline-and-block)** {icon="github"}
  Exactly what this stage added.
:::
