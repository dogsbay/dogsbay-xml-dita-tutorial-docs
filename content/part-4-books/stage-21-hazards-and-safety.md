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


Recorded diagnostic examples below come from earlier runs. File counts, paths, and stage numbers can differ. Run the gate for your current checkout.

## Step 1: The hazard in the shared topic

::::steps
1. **Add `images/hazard-hearing.png`**
   Retrieve the supplied warning-triangle illustration:

   ```bash
   git restore --source=origin/tutorial/21-hazards-and-safety -- images/hazard-hearing.png
   ```

2. **Edit `shared/common-notes.dita`**

   ```diff title="shared/common-notes.dita"
   --- a/shared/common-notes.dita
   +++ b/shared/common-notes.dita
   @@ -23,6 +23,17 @@
        <note id="backup-warning" type="warning">This effect changes the audio data.
        Save the project first; you can undo with <ph platform="windows linux"><uicontrol>Ctrl+Z</uicontrol></ph><ph platform="mac"><uicontrol>Cmd+Z</uicontrol></ph>.</note>
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

   The `silent` key, defined in the full guide in stage 20, was unused
   until now; `dogsbay-xml health` listed it. The topic is print-only, so
   the sentence appears in the PDF, and the PDF is built from a different
   map.

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

   Stage 20 defined `silent` in `audacity-guide.ditamap` only; the book's
   `<appendix>` for the same topic had no `keys`. Without this line the
   notices page of `out/book-pdf/audacity-book.pdf` reads *Recording
   nothing but silence? See .* with nothing after *See*, and the gate
   does not catch it: `project-health` resolves keys against the root
   map, `audacity-guide.ditamap`, where the key exists, and the PDF build
   does not fail on an unresolved key. A key that a topic uses must be
   defined in every map that publishes the topic; with it, the sentence
   in the PDF reads *Recording nothing but silence? See The recording is
   silent on page 37.*

6. **Read the output**
   The gate's HTML5 build of the full guide,
   `topics/recording-your-first-track.html`, has the hazard in place:

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

## Step 3: README and the gate

::::steps
1. **Change the "You are on" line**

   ```diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -2,7 +2,7 @@
    
    Build a guide through 27 stages. Maps begin at stage 02; HTML publication begins at stage 03.
    
   -You are on stage 20: troubleshooting.
   +You are on stage 21: hazards and safety.
    
    ## Stages
    
   ```

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita shared/*.dita *.ditamap
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

Test the hazard statement without its placeholder. Make the reference in the recording task a single
empty element, `<hazardstatement conkeyref="common-notes/hearing-hazard"/>`,
and run the gate with `SKIP_BUILD=1`:

```
== validate-project  /home/you/audacity-guide ==
/home/you/audacity-guide/topics/recording-your-first-track.dita:
  31:65  error: The content of element type "hazardstatement" is incomplete, it must match "(messagepanel+,hazardsymbol*)".
39 file(s): 38 valid, 1 invalid.
FAIL: validation errors
```

The DTD requires a `<messagepanel>` in every `<hazardstatement>`, whether
or not it is going to be replaced. Give a conref'd hazard the empty panel
the same way you give a conref'd step its empty `<cmd/>`.

After each error exercise, undo the deliberate change and rerun the gate.
Confirm that it prints `STAGE OK` before continuing.

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

Continue with [Stage 22: software domains](/part-4-books/stage-22-software-domains).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/20-troubleshooting...tutorial/21-hazards-and-safety).
