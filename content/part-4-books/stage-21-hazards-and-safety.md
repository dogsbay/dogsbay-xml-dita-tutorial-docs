---
title: "Stage 21: Hazards and safety"
description: Create a hazard statement with a message panel and symbol, then reuse it in the recording task with conkeyref.
type: tutorial
---

# Stage 21: Hazards and safety

Structure hazard information with `<hazardstatement>`. Its
`<messagepanel>` separates the hazard, consequence, and avoidance
instructions. An optional `<hazardsymbol>` supplies an image.

Store a hearing-hazard example in `shared/common-notes.dita` and reuse it
in the recording task with `conkeyref`. Add a clipping example to the
effects reference. These examples demonstrate markup; they do not establish
compliance with a safety standard.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 25 minutes.
**You need:** stage 20 complete.


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: The hazard in the shared topic

::::steps
1. **Copy `images/hazard-hearing.png` into your project**
   You cannot type an image, so copy the warning-triangle illustration
   from the `images` folder at the `tutorial/21-hazards-and-safety`
   checkpoint of the [sample project](/start-here/set-up#the-sample-project)
   in one of these ways:

   - If you have a copy of the sample project, switch it to the
     `tutorial/21-hazards-and-safety` branch and copy the file from its
     `images` folder.
   - Otherwise, open
     [hazard-hearing.png on the checkpoint](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/blob/tutorial/21-hazards-and-safety/images/hazard-hearing.png)
     on GitHub and click **Download raw file**.

   Save the file in `my-audacity-guide/images`, with the same name. To
   open that folder in your file manager, right-click `images` in the
   **Explorer** and choose **Reveal in System**. The **Explorer** shows
   `hazard-hearing.png` under `images`. If it does not appear, right-click
   `images` and choose **Refresh**.

2. **Edit `shared/common-notes.dita`**

   ```diff title="shared/common-notes.dita"
   --- a/shared/common-notes.dita
   +++ b/shared/common-notes.dita
   @@ -23,6 +23,17 @@
        <note id="backup-warning" type="warning">This effect changes the audio data.
        Save the project first; you can undo with <ph platform="windows linux"><uicontrol>Ctrl+Z</uicontrol></ph><ph platform="mac"> <ph platform="windows linux">or, on macOS,</ph> <uicontrol>Cmd+Z</uicontrol></ph>.</note>
        <note id="quiet-room-tip" type="tip">For best results, record in a quiet room and position the microphone 15 to 30 cm from your mouth.</note>
   +    <hazardstatement id="hearing-hazard" type="caution">
   +      <messagepanel>
   +        <typeofhazard>Loud playback through headphones</typeofhazard>
   +        <consequence>Sustained listening above 85 dB can damage your hearing permanently.</consequence>
   +        <howtoavoid>Set the playback volume low before you press Play and raise it gradually.</howtoavoid>
   +        <howtoavoid>Take the headphones off before applying Amplify or Normalize with the track playing.</howtoavoid>
   +      </messagepanel>
   +      <hazardsymbol href="../images/hazard-hearing.png" height="32px">
   +        <alt>Warning triangle</alt>
   +      </hazardsymbol>
   +    </hazardstatement>
        <p>Recommended starting settings for Noise Reduction:</p>
        <ul id="noise-reduction-settings">
          <li id="nr-first">Noise reduction: 12 dB</li>
   ```

3. **Read it**

   - `<hazardstatement type="caution">` identifies the statement type.
     This example uses `caution` and an `id` so other topics can reuse it.
     Check the hazard domain's allowed values when choosing a type.
   - `<messagepanel>` holds `<typeofhazard>`, the hazard in a few words;
     `<consequence>`, what happens if it is ignored; and one or more
     `<howtoavoid>`, one per measure. The order is fixed and only
     `<typeofhazard>` and `<howtoavoid>` are required.
   - `<hazardsymbol>` is an `<image>` specialization, after the panel. The
     `href` is relative to the shared topic file, hence `../images/`, and
     the `<alt>` is required for the same reason it is on an image.
::::

## Step 2: Reuse it, and write one inline

::::steps
1. **Edit `topics/recording-your-first-track.dita`**

   ```diff title="topics/recording-your-first-track.dita"
   --- a/topics/recording-your-first-track.dita
   +++ b/topics/recording-your-first-track.dita
   @@ -28,6 +28,12 @@
          You need a microphone connected to your computer.
          If you are new to audio terms such as sample rate and clipping, read <xref href="what-is-digital-audio.dita"/> first.</p>
          <note conkeyref="common-notes/quiet-room-tip"/>
   +      <hazardstatement conkeyref="common-notes/hearing-hazard">
   +        <messagepanel>
   +          <typeofhazard/>
   +          <howtoavoid/>
   +        </messagepanel>
   +      </hazardstatement>
        </context>
        <steps>
          <step>
   ```

2. **Read the placeholder panel**
   A conref'd `<note>` is an empty element:
   `<note conkeyref="common-notes/quiet-room-tip"/>`, the line above the
   new one. A conref'd `<hazardstatement>` cannot be, because the DTD
   requires a `<messagepanel>` with at least a `<typeofhazard>` and a
   `<howtoavoid>` in it, and validation checks the referencing element
   before the reference is resolved. So the reference carries an empty
   panel, `<messagepanel><typeofhazard/><howtoavoid/></messagepanel>`,
   exactly as a conref'd `<step>` carries an empty `<cmd/>` (stage 11).
   The build replaces the whole element with the shared topic copy, symbol
   and all. The demo at the end shows the error without the placeholder.

3. **Edit `topics/effects-reference.dita`**

   ```diff title="topics/effects-reference.dita"
   --- a/topics/effects-reference.dita
   +++ b/topics/effects-reference.dita
   @@ -25,6 +25,13 @@
        <section>
          <p><keyword keyref="product-name"/> includes several built-in effects.
            Select a region of audio, then choose an effect from the <uicontrol>Effect</uicontrol> menu.</p>
   +      <hazardstatement type="warning">
   +        <messagepanel>
   +          <typeofhazard>Amplify with <uicontrol>Allow clipping</uicontrol> enabled</typeofhazard>
   +          <consequence>The output can exceed 0 dB and reach your speakers or headphones as a sudden, very loud burst.</consequence>
   +          <howtoavoid>Leave <uicontrol>Allow clipping</uicontrol> off unless you intend to clip, and preview at low volume.</howtoavoid>
   +        </messagepanel>
   +      </hazardstatement>
        </section>
        <table outputclass="compact">
          <title>Built-in audio effects</title>
   ```

   The example uses `warning` and is written at its point of use in the
   Amplify description. The optional symbol is omitted. Inline elements such as `<uicontrol>` are allowed
   inside the panel's children.

4. **Edit `topics/about-this-guide.dita`**

   ```diff title="topics/about-this-guide.dita"
   --- a/topics/about-this-guide.dita
   +++ b/topics/about-this-guide.dita
   @@ -20,7 +20,8 @@
        Its topics are adapted from the Audacity Manual, copyright the Audacity Team and the Manual's authors, under the Creative Commons Attribution 3.0 licence.</p>
        <p>New to <keyword keyref="product-name"/>? Start with <xref keyref="start-here"/>.
        New to audio? Read <xref keyref="digital-audio"/> first.
   -    Here for podcasting? Go straight to <xref keyref="podcast-workflow"/>.</p>
   +    Here for podcasting? Go straight to <xref keyref="podcast-workflow"/>.
   +    Recording nothing but silence? See <xref keyref="silent"/>.</p>
        <p><keyword keyref="product-name"/> is a registered trademark of Dominic Mazzoni.
        The Audacity Team does not endorse this guide and is not affiliated with it.</p>
      </body>
   ```

   The `silent` key comes from stage 20. The topic is print-only, so the
   sentence appears only in the PDF, and the PDF is built from a
   different map.

5. **Edit `audacity-book.ditamap`**

   ```diff title="audacity-book.ditamap"
   --- a/audacity-book.ditamap
   +++ b/audacity-book.ditamap
   @@ -65,7 +65,7 @@
        <chapter href="topics/podcast-production-workflow.dita" keys="podcast-workflow"/>
      </part>
    
   -  <appendix href="topics/recording-is-silent.dita"/>
   +  <appendix href="topics/recording-is-silent.dita" keys="silent"/>
      <appendix href="topics/supported-audio-formats.dita" keys="formats"/>
      <appendix href="topics/effects-reference.dita" keys="effects"/>
      <appendix href="topics/effect-presets.dita"/>
   ```

   Stage 20 defined `silent` in the full guide, the beginner guide and
   the installation variants, but not in the book: the book's
   `<appendix>` for the same topic had no `keys`. The trouble note from
   stage 20 did not need it, because its `href` fallback resolves without
   the key. The new sentence has only a `keyref`. Without this line the
   notices page of `out/book-pdf/audacity-book.pdf` reads *Recording
   nothing but silence? See .* with nothing after *See*, and the check
   still reports `Ready`. Health resolves keys against the root map,
   `audacity-guide.ditamap`, where the key exists, the PDF build does not
   fail on an unresolved key, and the output check reads links in HTML
   pages only. A key that a topic uses must be
   defined in every map that publishes the topic; with it, the sentence
   in the PDF reads *Recording nothing but silence? See The recording is
   silent on page 37.*

6. **Read the output**
   The check's HTML5 build of the full guide,
   `out/full/topics/recording-your-first-track.html`, has the hazard in
   place (the inline SVG is shortened here):

   ```
   <table role="presentation" border="1" class="note hazardstatement"><tr><th colspan="2" class="hazardstatement--caution"><svg class="hazardsymbol" …>…</svg> CAUTION</th></tr><tr><td><img class="image hazardsymbol" height="32" src="../images/hazard-hearing.png" alt="Warning triangle"></td><td><div class="messagepanel">
   <div class="typeofhazard">Loud playback through headphones</div>
   <div class="consequence">Sustained listening above 85 dB can damage your hearing permanently.</div>
   <div class="howtoavoid">Set the playback volume low before you press Play and raise it gradually.</div>
   <div class="howtoavoid">Take the headphones off before applying Amplify or Normalize with the track playing.</div>
   </div></td></tr></table>
   ```

   The signal word is the `class`: `hazardstatement--caution`, with the
   panel's parts each a `div` of their own, for a stylesheet to lay out.
::::

## Step 3: Check your work

::::steps
1. **Format and check your work**

   Format the changed files: choose **XML** > **Format** in each changed
   file and save it, or choose **Project** > **Project Tools** >
   **Format Project** to format every file at once. From the command line,
   run:

   ```bash
   dogsbay-xml format -i topics/*.dita shared/*.dita *.ditamap
   ```

   Then check the project. In the editor, choose **Project** >
   **Check Project** and read the result in the **Project Validation**
   panel. From the command line, run:

   ```bash
   dogsbay-xml check .
   ```

   Example output:

   ```
   health   clean
   build    full                 ok  /home/you/my-audacity-guide/out/full
   build    beginner-mac         ok  /home/you/my-audacity-guide/out/beginner-mac
   build    beginner-windows     ok  /home/you/my-audacity-guide/out/beginner-windows
   build    podcaster-linux      ok  /home/you/my-audacity-guide/out/podcaster-linux
   build    review               ok  /home/you/my-audacity-guide/out/review
   build    install-variants     ok  /home/you/my-audacity-guide/out/install-variants
   build    collection           ok  /home/you/my-audacity-guide/out/collection
   build    book-pdf             ok  /home/you/my-audacity-guide/out/book-pdf
     PDF rendering reported 17 warnings (11 The following feature isn't implemented by Apache FOP, yet: table-layout=… (on fo:table) (…, 2 The contents of fo:block line n exceed the available area in the inline-progression direct…, 1 The contents of fo:external-graphic line n exceed the available area in the inline-progres…, and 3 other kinds)
   output   wrote a file, no pages to check links in book-pdf
   output   clean (full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection)
   Ready: the project is healthy, every deliverable built, and the output of full, beginner-mac, beginner-windows, podcaster-linux, review, install-variants, collection holds together.
   ```
::::

Test the hazard statement without its placeholder. Make the reference in the recording task a single
empty element, `<hazardstatement conkeyref="common-notes/hearing-hazard"/>`,
and check your work. The check stops at health. Example output:

```
health   NOT CLEAN
  invalid: /home/you/my-audacity-guide/topics/recording-your-first-track.dita
    31:65  The content of element type "hazardstatement" is incomplete, it must match "(messagepanel+,hazardsymbol*)".
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

The DTD requires a `<messagepanel>` in every `<hazardstatement>`, whether
or not it is going to be replaced. Give a conref'd hazard the empty panel
the same way you give a conref'd step its empty `<cmd/>`.

Undo the change and check again. Confirm that the check reports `Ready`
before you continue.

## What you learned

- `<hazardstatement type>` with `<messagepanel>`: `<typeofhazard>`,
  `<consequence>`, one or more `<howtoavoid>`; `<hazardsymbol>` with
  `<alt>`.
- A hazard statement separates the hazard, consequence, and avoidance
  instructions in a panel.
- A conref'd hazard statement needs a placeholder panel, like the `<cmd/>`
  of a conref'd step.
- The HTML5 class `hazardstatement--<type>`.
- Every key defined gets used: `silent`, and a key a topic uses is
  defined in every map that publishes the topic.

## Next lesson

**Checkpoint:** `tutorial/21-hazards-and-safety`. If you use Git, commit your work.

Continue with [Stage 22: software domains](/part-4-books/stage-22-software-domains).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/20-troubleshooting...tutorial/21-hazards-and-safety).
