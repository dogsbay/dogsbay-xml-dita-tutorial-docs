---
title: "Stage 20: Hazards and safety"
description: Write a hazard statement with a message panel and a symbol, warehouse it, pull it into the recording task by conkeyref, and add an inline warning.
type: tutorial
---

# Stage 20: Hazards and safety

A `<note type="warning">` is a sentence. A safety statement in a printed
manual is more: a signal word, a symbol, what the hazard is, what it does to
you and how to avoid it, laid out the same way every time so that the reader
recognises it. DITA's hazard statement domain gives that structure:
`<hazardstatement>` with a `<messagepanel>` that separates the type of
hazard, its consequence and each way to avoid it, and an optional
`<hazardsymbol>`.

This stage adds two. A hearing hazard about loud playback goes into the
`shared/common-notes.dita` warehouse from stage 10, with a symbol image,
and is pulled into the recording task by `conkeyref`. A clipping warning is
written inline in the effects reference. The `silent` key from stage 19 gets
its use.

**Time:** about 25 minutes.
**You need:** stage 19 complete.

## Step 1: The hazard in the warehouse

::::steps
1. **Add `images/hazard-hearing.png`**
   A small warning triangle, 32 pixels high. The one on the branch is a
   generated illustration, as the stage 06 images are; any PNG will do.

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
   - `<hazardstatement type="caution">` takes the same `@type` values as
     a note. `danger`, `warning` and `caution` are the signal words of
     the ANSI Z535 safety standard, from most to least severe, and the
     hearing hazard gets the mildest. An `id`, as on every reusable
     element in the warehouse.
   - `<messagepanel>` holds `<typeofhazard>`, the hazard in a few words;
     `<consequence>`, what happens if it is ignored; and one or more
     `<howtoavoid>`, one per measure. The order is fixed and only
     `<typeofhazard>` and `<howtoavoid>` are required.
   - `<hazardsymbol>` is an `<image>` specialisation, after the panel. The
     `href` is relative to the warehouse file, hence `../images/`, and
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
   exactly as a conref'd `<step>` carries an empty `<cmd/>` (stage 10).
   The build replaces the whole element with the warehouse copy, symbol
   and all. The demo at the end shows the error without the placeholder.

3. **Edit `topics/effects-reference.dita`**

   ```diff title="topics/effects-reference.dita"
   --- a/topics/effects-reference.dita
   +++ b/topics/effects-reference.dita
   @@ -27,6 +27,13 @@
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

   A `warning`, one step up from `caution`, written where it is used
   because only the Amplify effect has this hazard. No symbol: the
   element is optional. Inline elements such as `<uicontrol>` are allowed
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

   The `silent` key, defined in the full guide in stage 19, was unused
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
    
   ```

   Stage 19 defined `silent` in `audacity-guide.ditamap` only; the book's
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
   The gate's html5 build of the full guide,
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
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 19 — troubleshooting**: a troubleshooting topic with causes and remedies, and trouble notes that point to it.
   +You are on **stage 20 — hazards and safety**: hazard statements with a message panel and a symbol, reused by conkeyref.
    
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

Now drop the placeholder. Make the reference in the recording task a single
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

## What you learned

- `<hazardstatement type>` with `<messagepanel>`: `<typeofhazard>`,
  `<consequence>`, one or more `<howtoavoid>`; `<hazardsymbol>` with
  `<alt>`.
- When a hazard is not a note: harm to a person, laid out as a panel with
  a signal word.
- A conref'd hazard statement needs a placeholder panel, like the `<cmd/>`
  of a conref'd step.
- The html5 class `hazardstatement--<type>`.
- Every key defined gets used: `silent`, and a key a topic uses is
  defined in every map that publishes the topic.

## Where to go next

:::cards
- **[Stage 21: Software domains](/part-4-books/stage-21-software-domains)** {icon="arrow-right"}
  Commands, parameters, messages, a syntax diagram and code pulled from a
  file.

- **[Compare 19 to 20 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/19-troubleshooting...tutorial/20-hazards-and-safety)** {icon="github"}
  Exactly what this stage added.
:::
