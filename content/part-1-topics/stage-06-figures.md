---
title: "Stage 06: Figures"
description: Add a figure with an image and alt text, an inline SVG, an image map with clickable areas, and the decibel formula as MathML with a text fallback.
type: tutorial
---

# Stage 06: Figures

In this stage the guide gets its first pictures. *What is digital audio?*
gains a figure with a waveform image, a second figure drawn as inline SVG,
and the decibel formula as an equation. *Recording your first track* gains an
image map: a picture of the Device Toolbar whose regions link to other
topics.

The images are generated illustrations, not screenshots of Audacity, so the
tutorial does not go stale with the product's interface. They live in an
`images/` folder beside `topics/`, which is why every `@href` to them starts
with `../`.

**Time:** about 25 minutes.
**You need:** stage 05 complete, and the two PNG files from the branch (see
step 1).

## Step 1: Add the images

::::steps
1. **Create `images/` and copy the two files**
   Take `images/waveform.png` and `images/device-toolbar.png` from the
   `tutorial/06-figures` branch, or draw your own; the image map coordinates
   in step 3 assume the branch's `device-toolbar.png`.

   ```bash
   mkdir images
   git show tutorial/06-figures:images/waveform.png > images/waveform.png
   git show tutorial/06-figures:images/device-toolbar.png > images/device-toolbar.png
   ```

2. **Write `images/README.md`**
   It records where the images came from and the MathML caveat below.

   ```md title="images/README.md"
   Illustrations for the tutorial, generated (not screenshots): `waveform.png`
   and `device-toolbar.png`. CC BY 4.0 like the rest of the project.

   Note on equations: `what-is-digital-audio.dita` carries the decibel formula as
   MathML inside `<equation-block>`, with a plain-text `<ph>` alternative beside
   it. DITA-OT's html5 transform drops `<mathml>` (it is a `foreign` element)
   unless a MathML plugin is installed, so the HTML shows the text form; a
   processor that renders MathML shows the formula.
   ```
::::

## Step 2: Figures, SVG and an equation

::::steps
1. **Edit `topics/what-is-digital-audio.dita`**
   Two figures go into the *Waveforms* section, and the decibel formula goes
   into the *Amplitude* definition.

   ```diff title="topics/what-is-digital-audio.dita"
   --- a/topics/what-is-digital-audio.dita
   +++ b/topics/what-is-digital-audio.dita
   @@ -13,6 +13,24 @@
          <p>Sound travels through air as a continuous <term>waveform</term> of pressure changes.
          A microphone converts these pressure changes into an electrical signal.
          To store the signal digitally, the computer takes thousands of measurements, called <term>samples</term>, of the signal's amplitude every second.</p>
   +      <fig id="fig-waveform">
   +        <title>A recorded waveform, with clipping at the right</title>
   +        <image href="../images/waveform.png" placement="break" scale="80">
   +          <alt>A waveform whose peaks are cut flat at the right-hand end</alt>
   +        </image>
   +      </fig>
   +      <p>Drawn schematically, one cycle of a pure tone looks like this:</p>
   +      <fig>
   +        <title>One cycle of a sine wave</title>
   +        <svg-container>
   +          <svg:svg xmlns:svg="http://www.w3.org/2000/svg" width="320" height="120" viewBox="0 0 320 120">
   +            <svg:line x1="0" y1="60" x2="320" y2="60" stroke="#999" stroke-width="1"/>
   +            <svg:path d="M0,60 C40,0 80,0 120,60 S200,120 240,60 S300,20 320,45" fill="none" stroke="#3c78c8" stroke-width="3"/>
   +            <svg:text x="8" y="16" font-size="12" fill="#333">amplitude</svg:text>
   +            <svg:text x="270" y="76" font-size="12" fill="#333">time</svg:text>
   +          </svg:svg>
   +        </svg-container>
   +      </fig>
        </section>
        <section>
          <title>Sample rate</title>
   @@ -57,7 +75,22 @@
            <dlentry>
              <dt>Amplitude</dt>
              <dd>The height of a sound wave, corresponding to how loud the sound is.
   -          Measured in decibels (dB).</dd>
   +          Measured in decibels (dB), a ratio against a reference level:
   +          <equation-block>
   +            <mathml>
   +              <m:math xmlns:m="http://www.w3.org/1998/Math/MathML" display="block">
   +                <m:mrow>
   +                  <m:msub><m:mi>L</m:mi><m:mtext>dB</m:mtext></m:msub>
   +                  <m:mo>=</m:mo>
   +                  <m:mn>20</m:mn>
   +                  <m:msub><m:mi>log</m:mi><m:mn>10</m:mn></m:msub>
   +                  <m:mfrac><m:mi>A</m:mi><m:msub><m:mi>A</m:mi><m:mn>0</m:mn></m:msub></m:mfrac>
   +                </m:mrow>
   +              </m:math>
   +            </mathml>
   +            <ph>L (dB) = 20 log10 (A / A0)</ph>
   +          </equation-block>
   +          A level of <equation-inline><mathml><m:math xmlns:m="http://www.w3.org/1998/Math/MathML"><m:mrow><m:mo>−</m:mo><m:mn>6</m:mn><m:mtext>dB</m:mtext></m:mrow></m:math></mathml><ph>−6 dB</ph></equation-inline> is half the amplitude of the reference.</dd>
            </dlentry>
            <dlentry>
              <dt>Clipping</dt>
   ```

2. **Read the figure elements**
   - `<fig>` is a figure: a `<title>` and some content, numbered in output.
     Give it an `@id` when something links to it, as `fig-waveform` is
     linked from the recording task.
   - `<image href="…">` places an image. `@placement="break"` puts it on its
     own line rather than inline with the text; `@scale="80"` renders it at
     80 percent of its size. `@href` is relative to the topic file, hence
     `../images/`.
   - `<alt>` is the text alternative, read by screen readers and shown when
     the image cannot be. Write what the picture shows, not "image of".
   - `<svg-container>` holds inline SVG. The SVG elements must carry the
     `svg:` prefix, `<svg:svg xmlns:svg="http://www.w3.org/2000/svg">`,
     because that is how the DITA DTDs declare them; an unprefixed `<svg>`
     does not validate.

3. **Read the equation elements**
   - `<equation-block>` is a displayed equation; `<equation-inline>` is one
     inside a sentence. Both are from the equation domain, which the base
     DTDs include.
   - `<mathml>` holds MathML, here with the `m:` prefix bound to the MathML
     namespace. The formula is `L(dB) = 20 log10 (A / A0)`.
   - The `<ph>` beside each `<mathml>` is a text alternative. The spec allows
     an equation to carry several representations; a processor uses the one
     it can render.
::::

> [!NOTE]
> DITA-OT 4.3.5's html5 transform drops `<mathml>`, which is a `foreign`
> specialisation, unless a MathML plugin is installed. That is why each
> equation keeps a `<ph>` text alternative beside the MathML: the HTML shows
> the text form, and a processor that renders MathML shows the formula. You
> will see this when stage 07 builds the guide for the first time.

## Step 3: An image map

::::steps
1. **Edit `topics/recording-your-first-track.dita`**
   The Device Toolbar step gets an `<imagemap>`, and the waveform step gets a
   cross-reference to the figure by its id.

   ```diff title="topics/recording-your-first-track.dita"
   --- a/topics/recording-your-first-track.dita
   +++ b/topics/recording-your-first-track.dita
   @@ -22,13 +22,29 @@
            <cmd>Select your microphone from the recording device list in the <uicontrol>Device Toolbar</uicontrol>.</cmd>
            <info>
              <p>The <uicontrol>Device Toolbar</uicontrol> is near the top of the window.
   -          The recording device list has a microphone icon next to it.</p>
   +          The recording device list has a microphone icon next to it.
   +          Click a control in the picture to read about it.</p>
   +          <imagemap>
   +            <image href="../images/device-toolbar.png">
   +              <alt>The Device Toolbar: audio host, recording device, channels and playback device</alt>
   +            </image>
   +            <area>
   +              <shape>rect</shape>
   +              <coords>170,20,370,60</coords>
   +              <xref href="preparing-to-record.dita">Recording device</xref>
   +            </area>
   +            <area>
   +              <shape>rect</shape>
   +              <coords>380,20,490,60</coords>
   +              <xref href="what-is-digital-audio.dita">Channels</xref>
   +            </area>
   +          </imagemap>
            </info>
          </step>
          <step>
            <cmd>Click <uicontrol>Record</uicontrol>, or press <uicontrol>R</uicontrol>, to start recording.</cmd>
            <info>
   -          <p>A blue waveform appears as Audacity captures audio from your microphone.</p>
   +          <p>A blue waveform appears as Audacity captures audio from your microphone, like <xref href="what-is-digital-audio.dita#what-is-digital-audio/fig-waveform"/>.</p>
              <note type="warning">A flat line instead of a waveform means the microphone is not selected correctly.
              Click <uicontrol>Stop</uicontrol> and check the recording device.</note>
            </info>
   ```

2. **Read the image map elements**
   - `<imagemap>` is an `<image>` followed by one or more `<area>` elements.
     It is from the utilities domain.
   - Each `<area>` has a `<shape>` (`rect`, `circle` or `poly`), `<coords>`
     in pixels of the image, and an `<xref>` for the link. Here the `<xref>`
     elements have content, so the link text is the given label rather than
     the target's title.
   - The cross-reference `what-is-digital-audio.dita#what-is-digital-audio/fig-waveform`
     reaches the figure by its `@id`, the same `file#topic/element` form as a
     section link in stage 05.
::::

## Step 4: Update the README and run the gate

::::steps
1. **Change the "You are on" line and the layout**
   The layout section gains the `images/` folder.

   ````diff title="README.md"
   --- a/README.md
   +++ b/README.md
   @@ -5,7 +5,7 @@ branch is the previous branch plus one lesson, from a single concept topic to a
    complete, publish-ready **Audacity User Guide**. The diff between two branches
    is the lesson.
    
   -You are on **stage 05 — links**: cross-references, related links, link lists and external links.
   +You are on **stage 06 — figures**: images with alt text, an inline SVG, an image map and a MathML equation.
    
    ## Stages
    
   @@ -39,6 +39,7 @@ override the defaults). A stage is done when it prints `STAGE OK`.
    .dogsbay/config.xml   shared editor project settings (project type, framework, format style)
    scripts/              the gate
    topics/               topics
   +images/               illustrations referenced by <image>
    ```
    
    ## Licence and attribution
   ````

2. **Format and check**

   ```bash
   dogsbay-xml format -i topics/*.dita
   scripts/check-stage.sh
   ```

   ```
   == validate-project  /home/you/audacity-guide ==
   9 file(s): 9 valid, 0 invalid.

   == project-health  /home/you/audacity-guide ==
   Root map: none (none configured); house rules: none (none configured)
   Project is healthy: valid, no broken references, keys, orphans, or broken element ids.

   == build  skipped (no project.json yet) ==

   STAGE OK
   ```
::::

`project-health` checks image references too. Misspell `waveform.png` in the
`<fig>` and run the gate to see the broken reference.

## What you learned

- `<fig>` with `<title>` and an `@id`; `<image>` with `@href`, `@placement`,
  `@scale` and an `<alt>`.
- Image paths are relative to the topic, so a sibling `images/` folder is
  `../images/`.
- Inline SVG goes in `<svg-container>` with the `svg:` prefix.
- `<equation-block>` and `<equation-inline>` carry `<mathml>` plus a `<ph>`
  text alternative; html5 output needs the alternative.
- `<imagemap>`: an image, then `<area>` elements with `<shape>`, `<coords>`
  and an `<xref>`.
- End of Part 1: nine topics, every base element you need for the body of a
  guide, and no map yet.

## Where to go next

:::cards
- **[Stage 07: Your first map](/part-2-maps/stage-07-first-map)** {icon="arrow-right"}
  Part 2 starts with the map: the nine topics become a guide, and DITA-OT
  builds it for the first time.

- **[Compare 05 to 06 on GitHub](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/05-links...tutorial/06-figures)** {icon="github"}
  Exactly what this stage added.
:::
