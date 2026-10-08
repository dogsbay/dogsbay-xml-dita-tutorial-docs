---
title: "Stage 08: Figures"
description: Add a figure with an image and alt text, an inline SVG, an image map with clickable areas, and the decibel formula as MathML with a text fallback.
type: tutorial
---

# Stage 08: Figures

Add two figures and an equation to *What is digital audio?*. Use a waveform
image for the first figure and inline SVG for the second. Add an image map
to *Recording your first track* with links from regions of a toolbar image.

The supplied images are generated illustrations of audio concepts and
controls. Store them in `images/`, beside `topics/`. References from the
topics therefore start with `../images/`.

**Optional module.** See [Choose a learning path](/start-here/learning-path)
for the starting checkpoint and the next core lesson.

**Time:** about 25 minutes.
**You need:** stage 07 complete, and the two PNG files from the sample
project (see step 1).


Recorded output below is an example. File counts, paths, and stage numbers can differ. [Check your work](/start-here/run-the-gate) to see the result for your own project.

## Step 1: Add the images

You cannot type an image, so add the two PNG files from the sample
project. The image map coordinates in step 3 assume the sample project's
`device-toolbar.png`. If you draw your own images, adjust the coordinates.

::::steps
1. **Create the `images` folder**
   In the **Explorer**, right-click `my-audacity-guide` and choose
   **New Folder**. Enter `images` and press Enter.

2. **Download the two images**
   Get `waveform.png` and `device-toolbar.png` from the `images` folder at
   the `tutorial/08-figures` checkpoint of the
   [sample project](/start-here/set-up#the-sample-project) in one of these
   ways:

   - If you have a copy of the sample project, switch it to the
     `tutorial/08-figures` branch and use the two files in its `images`
     folder.
   - Otherwise, open the
     [images folder of the checkpoint](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/tree/tutorial/08-figures/images)
     on GitHub, open each PNG file, and click **Download raw file**.

3. **Add the images to the `images` folder**
   In the **Explorer**, right-click `images` and choose **Add Files...**.
   In the file chooser, use **Look In** to go to the folder that holds the
   two PNG files, select both files, and click **Add**. The editor copies
   the files into `images` with the same names.

   You can also drag the two files from your desktop onto `images` in the
   **Explorer**.

   The **Explorer** shows `waveform.png` and `device-toolbar.png` under
   `images`.
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

   - `<fig>` is a figure: a `<title>`, which is the caption, and some
     content. The guide's build numbers the caption.
     Give it an `@id` when something links to it, as `fig-waveform` is
     linked from the recording task.
   - `<image href="…">` places an image. `@placement="break"` puts it on its
     own line rather than inline with the text; `@scale="80"` renders it at
     80 percent of its size. `@href` is relative to the topic file, hence
     `../images/`.
   - `<alt>` is the text alternative, read by screen readers and shown when
     the image cannot be. Describe the information that the image conveys.
   - `<svg-container>` holds inline SVG. The SVG elements must carry the
     `svg:` prefix, `<svg:svg xmlns:svg="http://www.w3.org/2000/svg">`,
     because that is how the DITA DTDs declare them; an unprefixed `<svg>`
     does not validate.

3. **Preview the topic**
   Save the file and choose **View** > **Preview in Tab**. The waveform is
   on its own line, with its caption, in the *Waveforms* section. The
   alternative text is not shown, because the image loaded; it is in the
   page for readers who cannot see the image. The editor's preview shows
   the SVG drawing as text. You see it drawn in the built guide in step 4.

4. **Read the equation elements**

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
> DITA-OT 4.3.5's HTML5 transform drops `<mathml>`, which is a `foreign`
> specialization, unless a MathML plugin is installed. That is why each
> equation keeps a `<ph>` text alternative beside the MathML: the HTML shows
> the text form, and a processor that renders MathML shows the formula. You
> can inspect this by rebuilding the guide after adding the figures.

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
   - The cross-reference to the figure has no text of its own. When the
     guide is built, the link takes its text from the figure.
   - The cross-reference `what-is-digital-audio.dita#what-is-digital-audio/fig-waveform`
     reaches the figure by its `@id`, the same `file#topic/element` form as a
     section link in stage 07.
::::

## Step 4: Build the guide

::::steps
1. **Format and build**
   Format the topics: choose **XML** > **Format** in each changed topic and
   save it, or choose **Project** > **Project Tools** > **Format Project** to
   format every file at once. From the command line, run:

   ```bash
   dogsbay-xml format -i topics/*.dita
   ```

   Then choose **Project** > **Build Deliverables...** and click
   **Build All**. When the build finishes, click **OK**. The build copies
   the images into the output and turns both figures into HTML.

   To make sure that every image and link also resolves, choose
   **Project** > **Check Project** in the editor, or run
   `dogsbay-xml check .` from the project root. The check also builds every
   deliverable. The output looks like this example:

   ```
   health   clean
   build    full                 ok  /home/you/my-audacity-guide/out/full
   output   clean (full)
   Ready: the project is healthy, every deliverable built, and the output of full holds together.
   ```

2. **See the figures built**
   The cross-reference to the figure had no text in the source. Predict
   what the link says in the built recording task. Then open
   `out/full/topics/recording-your-first-track.html`, choose **View** >
   **Preview in Tab**, and scroll down to the steps. In the **Record**
   step, the link says *Figure 1*: its text comes from the figure's
   number, not its caption. In the step above it, the
   toolbar picture is an image map, and its two regions link to
   *Preparing to record* and *What is digital audio?*.

   Open `out/full/topics/what-is-digital-audio.html`, preview it, and
   scroll down to the *Waveforms* section. The waveform figure has a
   numbered caption, and its image comes from the PNG file that the build
   copied into the output. The second figure is the sine wave, drawn from
   the SVG in the topic, with a numbered caption of its own.
::::

The health stage checks image references too. Misspell `waveform.png` in the
`<fig>` as `wavefrom.png` and check your work. The check reports the broken
reference with the file and line, and stops. The following output is an
example:

```
health   NOT CLEAN
  /home/you/my-audacity-guide/topics/what-is-digital-audio.dita:18  @href="../images/wavefrom.png" — target not found
  (run project-health for the full report)
Not ready: the project itself has faults. The build and the built output were not checked.
```

Undo the change and check again. Confirm that the check reports `Ready`
before you continue.

## The map at this checkpoint

This lesson adds no topics, so the map does not change. For reference, the
complete map at this checkpoint is:

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

## What you learned

- `<fig>` with `<title>` and an `@id`; `<image>` with `@href`, `@placement`,
  `@scale` and an `<alt>`.
- Image paths are relative to the topic, so a sibling `images/` folder is
  `../images/`.
- Inline SVG goes in `<svg-container>` with the `svg:` prefix.
- `<equation-block>` and `<equation-inline>` carry `<mathml>` plus a `<ph>`
  text alternative; the guide's `html5` build shows the alternative.
- `<imagemap>`: an image, then `<area>` elements with `<shape>`, `<coords>`
  and an `<xref>`.
- End of Part 1: nine topics, every base element you need for the body of a
  guide, published through the map and build introduced in stages 02 and 03.

## Next lesson

**Checkpoint:** `tutorial/08-figures`. If you use Git, commit your work.

Continue with [Stage 09: map structure](/part-2-maps/stage-09-map-structure).

For the core course, use the [learning path](/start-here/learning-path) to skip optional modules.

[Compare this stage with its predecessor](https://github.com/dogsbay/dogsbay-xml-dita-tutorial/compare/tutorial/07-links...tutorial/08-figures).
